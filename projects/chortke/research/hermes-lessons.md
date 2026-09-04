# درس‌های استقرار قبلی هرمس

**از کجا آمد.** مالک این را از جلسه‌ای گرفت که حدود یک ماه (مرداد ۱۴۰۵)
یک نصب هرمس را اداره کرده بود: نسخهٔ `v0.20.0`، در یک کانتینر اینکوس
روی اوبونتو ۲۴٫۰۴، کنار یک تونل، با فرمان‌دادن فارسی از تلگرام. آن
ماشین حالا خاموش است.

**چرا اینجا نشست.** تنها سند این پروژه که از یک **ماشین واقعی** آمده،
نه از تحقیق. هر بند برچسب دارد: دیده‌شده، از سند، یا تصمیم اپراتور.

**سه چیزی که برای چرتکه همه‌چیز را عوض می‌کند:**

۱. **مهارت دیوار نیست، متنِ پرامپت است.** عامل از دستور صریحِ مهارت
   منحرف شد و زیر فشار سبک، امتناعش را پس گرفت. مهارت برای ارزان و
   روان‌کردن یک مدل کوچک است، **نه برای امن‌کردنش.**

۲. **دیوار واقعی، برنامهٔ ریشه‌ای روی میزبان است** — الگوی «در» در بخش
   یک. دو بار ساخته شد و جواب داد. قاعده‌اش: اعتبارسنجی در همان برنامه
   می‌نشیند، نه در مهارت، نه در پرامپت، نه در مدل.

۳. **قاعده‌ای که روی یک مدل ثابت شد، روی مدل دیگر ثابت نیست.** همان
   قواعدی که روی یک مدل جواب داده بود، روی مدل دیگر سه بار در یک جلسه
   شکست.

**و هشدار خود سند، که باید رعایت شود:** هیچ‌کدام از این‌ها دربارهٔ ماشین
تازه واقعیت نیست تا وقتی از خودِ آن ماشین خوانده شود.

---

# Hermes Agent — Lessons from a Previous Deployment

Written 4 September 2026 for a new Hermes deployment being set up in Claude Code.

**Where this comes from.** One deployment, run for about a month (August 2026):
Hermes v0.20.0 in an unprivileged Incus container on an Ubuntu 24.04 VPS that
also hosted an AmneziaWG VPN tunnel. Operator on an iPad via Termius, agent
addressed in Persian over Telegram. That machine has since been powered off.

**How to read it.** Every fact here is a fact about *that* machine, *that*
Hermes version and *those* models. The new deployment will differ. Items marked
`[observed]` were seen on that machine; `[docs]` came from Hermes/Incus docs or
research documents and were not all verified there; `[decided]` were operator
decisions that shaped the design. Nothing here is a fact about the new machine
until it has been read from the new machine.

---

## 1. The architecture that was built

### Shape

```
VPS host (root-only tunnel + Incus)
 └── Incus container "hermes" (unprivileged)
      └── Hermes gateway (per-user systemd unit), Telegram adapter
           terminal.backend = local   → runs INSIDE the container
```

The agent's terminal never reached the host directly. The only routes out were
two SSH "doors". `[observed]`

### A "door" — the pattern for letting the agent touch the host safely

Proven twice. Copy this shape, not the specifics:

1. One small **root-owned program** on the host that does exactly one job.
2. A **dedicated unprivileged host account** for that door (`hostview`,
   `issuer`) — no password login, no supplementary groups.
3. That account's `authorized_keys` holds **exactly one line** with a forced
   command: `command="/usr/local/bin/<program>",restrict ssh-ed25519 …`
   Whatever the agent sends is discarded; only the program runs.
4. If the program needs root for one or two read-only commands, a **sudoers
   allowlist** (`/etc/sudoers.d/<door>`, mode 440, NOPASSWD, those commands
   and nothing else).
5. If the door takes arguments, they arrive via `SSH_ORIGINAL_COMMAND`, a
   readable wrapper reads them and passes them to a mode-700 root program that
   **re-validates them itself** and does not trust the wrapper.
6. A door is never widened to carry a second job. Build a second door.
7. Failure modes are loud: the program prints an explicit `read=ok` /
   `read=fail` first line. A program that prints zeros on a failed read looks
   exactly like a healthy empty result — this happened once and cost a false
   reading. `[observed]`

Door 1 returned a fixed five-line status report (`read=`, peer counts by
handshake window, `svc=`, `disk_pct=`, `uptime_s=`). The agent's job was to
call it and repeat it in Persian. `[observed]`

### Design rule that everything rests on

**Validation lives in the root-owned host program. Never in a skill, never in
a prompt, never in the model.** `[decided]`

### The alternative that was researched but not built

A host-side root script on a systemd timer writes a sanitized, numbers-only
status file into a directory bind-mounted **read-only** into the container
(`incus config device add hermes hoststatus disk source=… path=… readonly=true`).
Zero tokens for polling, minimal blast radius. Simplest variant: make the file
world-readable and skip idmap/`shift` entirely. `[docs]` — worth considering
first for any read-only need on a new machine.

---

## 2. What Hermes actually enforces, and what it does not

This was the single most important thing learned.

### Real walls (the model cannot get past these)

- SSH forced commands and sudoers allowlists on the host. `[observed]`
- The container boundary with `terminal.backend=local`. `[observed]`
- File ownership inside the container: a root-owned file with no write bit
  for the agent's user cannot be written by any agent tool. Proven by a failed
  write on the email config (`root:hermes`, mode 640). `[observed]`
- The hardline blocklist in `tools/approval.py` (rm -rf /, fork bombs, etc.).
  `[docs]`

### Partial

- **The spend guard on scheduled jobs.** Hermes refuses to run a cron job
  whose stored provider/model differs from the global config unless the job
  is pinned. It refuses *before* any inference call and sends a failure
  notification to Telegram. Code-enforced, fired for real. But the agent can
  re-pin a job itself, so it guards against silent drift, not against the
  agent's decisions. `[observed]`
- **The approval gate** for dangerous tool calls. Visible in Telegram, four
  answers (once / session / always / deny). Caught every code-execution
  attempt in one session. Scope is the tool call, not the intent. `[observed]`
- **`approvals.deny`** blocks matching *terminal* commands. It did **not**
  cover the agent's own file-writing tool: a file was edited through that tool
  while a deny pattern for its exact path was in place. `[observed]`

### Not walls at all

- **A skill (`SKILL.md`) is prompt text.** It constrains nothing. The agent
  deviated from explicit skill instructions and dropped a refusal under light
  pushback. There is no per-skill tool allowlist and no per-skill model
  routing in the shipped release (feature requests closed, unmerged). `[observed]` `[docs]`
- **The agent's memory files are prompt text too**, and the agent rewrites
  them on its own (see §4).
- **Pre-tool shell hooks fail OPEN.** A hook that errors, times out or emits
  malformed JSON logs a warning and the tool proceeds. `[docs]`
- `delegate_task` does not take a toolset parameter; a child inherits the
  parent's toolsets. `[docs]`

**Consequence:** use skills to make a cheap model efficient and on-rails.
Never use them to make it safe.

---

## 3. Hermes mechanics learned the hard way

- **Skills index alone does not route.** On that install, a terse Persian
  question triggered no skill twice even though the skill was listed. Routing
  had to be written into the agent's permanent memory (`memories/USER.md`),
  one line naming which question goes to which skill. `[observed]`
- **A Telegram session snapshots memory and skills at creation.** Changes on
  disk (memory restore, new skill) are not seen by an existing session; test
  with `/new`. A hand-placed skill *was* picked up without a gateway restart,
  but only in a fresh session. `[observed]`
- **`hermes config set` in a long-running gateway:** whether it takes effect
  without restart is unknown; one config change made after gateway start did
  not change the running model. Restart the gateway after config changes and
  verify by reading, not by assuming. `[observed, one case]`
- **Cron jobs carry their own provider/model.** Pin every scheduled job at
  creation. Edit with `hermes cron edit <job_id> --provider … --model …`.
  **The remediation command printed inside the spend-guard error text does not
  exist** (`cronjob action=update`). A machine's error message is not
  documentation. `[observed]`
- **Verify cron state server-side**, e.g. `grep -c` on the raw
  `~/.hermes/cron/jobs.json`, not by asking the agent. `[observed]`
- **`hermes prompt-size` reports `platform=cli`**, not the Telegram runtime;
  it showed `memory 0 B` while the memory file held content. Ratios at best.
  `hermes insights` token totals were internally inconsistent. Do not use
  either for spend. `[observed]`
- **`hermes tools list` and `prompt-size` disagree** on which toolsets carry
  bytes. Do not bank token savings from disabling a toolset until measured.
  `[observed]`
- **`hermes version` saying "custom build"** only means local HEAD differs
  from the tagged commit. Not tampering. `[observed]`
- **Telegram delivery:** text pasted into Telegram containing bulleted lists
  got no reaction from the agent; the same content as plain prose worked.
  Cause never read. Rule adopted: every prompt sent to the agent is plain
  prose, no list markers. `[observed, four sends]`
- `/restart` from Telegram restarts the gateway and reports success. `[observed]`
- The gateway ran as a per-user systemd unit in the container. `[observed]`
- Prompt caching is always-on for Claude on native Anthropic; the system
  prompt + skills index is frozen per session, so mid-session changes to
  toolsets/skills would break the cache. `[docs]`

---

## 4. Agent behaviour observed

- **The agent rewrote its own `USER.md` memory** during a "self-improvement
  review" and destroyed the routing for all three skills. A fresh session
  then behaved memory-blind (answered in English, offered to delete cron jobs
  it did not remember building). **Hermes keeps no backup of its own memory.**
  The fix was restore-from-a-hand-made-backup. Make dated copies of
  `memories/USER.md` and `MEMORY.md` yourself, regularly. `[observed]`
- **The agent wrote itself a skill** containing a copy-ready recipe to
  generate a fresh SSH key at the live key path. Self-skill-writing was left
  on; the watch was a periodic host-side `ls -lat` of the skills tree. `[observed]`
- **Stop-on-error did not hold on every model.** Rules proven on
  `claude-haiku-4-5` (one attempt, report verbatim, stop) failed three times
  in one session on a different model (`ox-alpha`): it chained self-written
  code blocks, read its own source instead of running the named command, and
  only stopped on `/stop`. **A rule proven on one model is not proven on
  another.** Re-test behaviour after any model change before unattended or
  write-capable work. `[observed]`
- **Agent output is not verbatim.** It reformatted a field before. Anything
  that must be exact (a command that edits a config, a key path) should not
  pass through the model — write it to a file host-side and point the operator
  at the path. `[observed]`
- **Agent self-reports are not verification.** It said "memory full" with the
  file at 958 bytes and merged entries to make room. `[observed]`
- The agent discovered the correct `hermes cron edit` tool on its own and read
  the raw job store accurately when told to. Given precise, one-command
  instructions with an expected output, Haiku followed them well. `[observed]`

---

## 5. Operating discipline that saved round trips

These are about the human/assistant workflow, but they are why the deployment
stayed intact. They apply just as much when Claude Code is the assistant.

- **Read before writing.** Predicting file layout, config structure, unit
  paths or file ownership from "how the technology usually works" was wrong
  repeatedly on that machine (systemd unit path, file ownership under
  `/var/lib/incus`, config layout vs. reference docs). Read the live state
  first, then act.
- **Every check must be able to fail.** Before sending a verification, prove
  in a sandbox that it distinguishes success from the specific failure it
  guards against. A size check after a `mv` that would pass either way proves
  nothing. A check placed after a pipeline reports the pipeline's last stage,
  not the command — use `echo "EXIT=$?"` carefully, or `set -o pipefail`.
- **Sensitive-file edits:** backup → staging copy → verify (line count,
  content spot-check, `md5sum`) → install. For one-line fixes, `sed` by line
  number, not by pattern rebuilt from a screenshot. Confirm the old text is
  gone with `grep … ; echo "GREP_EXIT=$?"` (exit 1 is the success signal).
- **Audit every command for side effects.** A "read-only" `lxc list || incus
  list` silently installed the LXD snap on the production host (`lxc` was
  aliased to a snap installer). Also, `||` skips the second command when the
  first exits 0, so it is not a reliable "try both" pattern.
- **Never print secrets.** Every check prints counts, sizes, md5 prefixes and
  verdict words — never key contents, never whole config files. Screenshots
  end up in chats.
- **One command at a time, with the expected output written next to it and a
  STOP condition.** This is what made a small model reliable.
- **Every command names its host and its executor** (host shell vs. inside
  the container). Same command, different filesystems.
- **Capability is not cut on suspicion.** `[decided]` A Hermes capability was
  never disabled because it *might* be dangerous; where a real cost occurred,
  the remedy was a recovery path (backup) rather than removal. Whether to
  carry this rule into the new project is the operator's call — it is
  recorded here because it shaped every decision.
- **Silence is not consent.** The assistant proposes; the operator decides;
  the assistant never executes its own proposal.

---

## 6. Backup and restore — what was proven and what was not

- `incus export hermes <file>.tar.gz --instance-only` produced a portable
  ~1.9 GB archive. `[observed]` Restore was proven **to file level** by
  importing under a different name (`incus import … hermes-restoretest`),
  inspecting, then `incus delete` — **never started**, because it carries the
  same Telegram token and network config as the live one. `[observed]`
- A **boot test of a restored container** (network device removed first) was
  never done. `[not proven]`
- On a **dir**-driver Incus pool, `incus snapshot` is a full copy in the same
  pool — not a backup. `[docs]`
- Provider (Vultr) snapshots image the whole machine and cannot be downloaded;
  recovery means spinning up a temporary server and pulling files via SFTP. `[docs]`
- Host-side configs (`/etc/amnezia`, `/etc/ufw`, door programs, sudoers,
  `authorized_keys`) were tarred separately — Incus export does not cover the
  host. Those restores were never exercised. `[not proven]`
- `~/.hermes/` inside the container is small (MBs) and is the part worth
  copying often: `config.yaml`, `.env`, `memories/`, `skills/`, `cron/jobs.json`.

---

## 7. Traps that cost something (do not assume these on the new machine)

- That a capability proven once is durably installed. Container-to-host SSH
  worked one day and failed the next: the container had no `known_hosts`.
  Rebuild it from the host's own `/etc/ssh/ssh_host_ed25519_key.pub`, never
  from the network.
- That "Host key verification failed" means tampering. There it meant no record.
- That "Permission denied (publickey)" should be fixed by generating a key.
  **Never let the agent generate or replace a key.** It cannot install one on
  the host, so a new key destroys working access and gains nothing.
- That a sudo prompt not appearing means the command did not run (sudo caches
  ~15 min).
- That a reference document describes *this* build. Two research documents
  were wrong about the config layout and about `approvals.deny`'s scope.
- That an aggregate recomputed from memory across screenshots is right.
  Recompute from the reading or do not state a total.
- That a measurement's platform label can be ignored (`prompt-size` → cli).
- That an agent gone memory-blind means the config broke. Check memory file
  dates before the config.
- That `hermes` (the container user) being in the `sudo` group is a route to
  root — it was password-locked there. Check, don't assume either way.

---

## 8. Things researched but never used — may fit the new project

- **Read-only status file via read-only bind mount** (§1). Cheapest safe
  visibility.
- **MCP server on the host over the Incus bridge** — needs
  `security.allow_private_urls: true`, which weakens SSRF protection. Not
  recommended as a default. `[docs]`
- **Subagents (`delegate_task`) with a narrow toolset** — the child's toolset
  restriction *is* runtime-enforced, so a narrow executor subagent is one of
  the few real constraint mechanisms. `[docs]`
- **Pre-tool-use hooks** in `~/.hermes/hooks/` — can block a tool call, but
  fail open on error. `[docs]`
- `tool_loop_guardrails.hard_stop_enabled: true` for unattended gateways
  (default false). `[docs]`
- `security.redact_secrets` — default may be off; set explicitly. `[docs]`
- `hermes prompt-size` / `hermes tools` to trim toolsets and shrink the
  per-call payload — measure before and after; see §3 for its limits.

---

## 9. A short checklist for standing up the new instance

Reasoned from the above, not a procedure that was run in this order.

1. Decide up front: what may the agent *read*, what may it *write*, and where
   does the enforcement for each live (host program / file ownership /
   container boundary)? Write that down before installing anything.
2. Install in a container or VM, `terminal.backend=local`, so the agent's
   shell is inside the boundary by default.
3. Set `approvals.mode: manual`, `approvals.cron_mode: deny`, an
   `approvals.deny` list for the paths and verbs that matter, and
   `hard_stop_enabled: true`. Restart the gateway and read the config back.
4. For any host access, build a door (§1). Read-only first. Exercise the
   read-only path repeatedly before building a write path.
5. Put skill routing into `memories/USER.md`, then back that file up with a
   dated copy. Repeat the backup after any session where the agent "reviewed"
   itself.
6. Pin every cron job's provider/model at creation; verify with `grep -c` on
   `cron/jobs.json`.
7. After any model change, re-run the behavioural tests (stop-on-error, one
   command only, verbatim report) before trusting unattended runs.
8. Take an `incus export` after setup and copy it off-host; prove restore to
   file level under a different name and never start it.
9. Keep a `DO-NOT-ASSUME` file and a `RUNBOOK` from day one. The previous
   project deferred them for six sessions and paid for it.
