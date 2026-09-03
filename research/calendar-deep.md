# تحقیق تقویم — پاس دوم (دیپ)

**۱۲ شهریور ۱۴۰۵. مورد ۹ دفتر اصلاحات.** مالک پاس اول را کافی ندانست و
گفت گسترده‌تر بگرد، ولی طوری که توکن و وقت زیادی نبرد.

**شکل اجرا:** اسکریپت را ناظر نوشت تا مهار شود — شش زاویهٔ مستقل، یک
پاس وارسی خصمانه روی همهٔ یافته‌ها با هم (نه رأی‌گیری سه‌تایی روی
تک‌تک ادعاها، که دفعهٔ قبل هزینه را منفجر کرد)، و یک جمع‌بندی.

**هزینهٔ واقعی: هشت ایجنت، ۷۲۶ هزار توکن، ۱۸۴ فراخوان ابزار، سی‌ودو
دقیقه.** برآورد ناظر ۱۵۰ تا ۳۰۰ هزار بود — **بیش از دو برابر تخمین.**
برای مقایسه: دیپ ریسرچ دفعهٔ قبل ۱۰۷ ایجنت و ۳٫۸۶ میلیون توکن بود و
گزارش نهایی‌اش اصلاً نوشته نشد. این یکی یک‌پنجم آن و کامل.

**درسِ تخمین:** آنچه در برآورد جا افتاد این بود که هر ایجنتِ گشتن،
علاوه بر وب، فایل‌های خود مخزن را هم باز کرد و اندازه گرفت. همان کار
ارزشمندترین بخش نتیجه شد، ولی در عدد دیده نشده بود.

---

## تأیید مستقل ناظر روی ادعای اصلی

گزارش می‌گوید ایراد در `daymoney.js` است. **خودم شمردم و درست است —
تندتر از آنچه گزارش نوشته:**

بی‌تاریخچه، `capsFor` سقف را می‌گذارد `sum * 0.25` که `sum` جمع کل ماه
همان ارز و همان جهت است. پس:

- **یک قلم در یک ارز:** نسبت می‌شود «مبلغ تقسیم بر یک‌چهارم خودش» یعنی
  چهار، و به یک بریده می‌شود. **همیشه بیشینه.**
- **دو قلم هم‌اندازه:** هر کدام نسبت دو می‌گیرند، هر دو بریده به یک.
  **هر دو بیشینه.** تا وقتی یکی بیش از سه برابر دیگری نباشد، هر دو
  بیشینه‌اند.
- **ده تا سی قلم:** نسبت‌ها حدود ۰٫۲ می‌شوند و طرح درست کار می‌کند.

**یعنی طرح در مقیاس پر کار می‌کند و در مقیاس کم می‌شکند — و مقیاس کم
همان جایی است که هر کاربر شروع می‌کند و مالک آزمود.**

---

{'summary': 'Bounded deep research: how to draw a month grid on a phone when each day carries a money magnitude — six angles, one verify pass, one synthesis. Max 8 agents.', 'agentCount': 8, 'logs': ['شش زاویه، هر کدام یک ایجنت — بدون رأی‌گیری سه‌تایی، برای مهار هزینه', '44 یافته از 6 زاویه'], 'result': {'report': '# تقویم چرتکه — نتیجهٔ پژوهش

## دنبال چه بودیم

تو گفتی تقویم «ابتدایی و نامفهوم» است. سه شکل جایگزین روی میز بود:

**الف** — مبلغ را داخل خانه بنویسیم، جای عدد میلادی.
**ب** — فقط یک نشانهٔ کوچک در خانه، و مبلغ فقط در پنل زیر شبکه.
**پ** — کل خانه ته‌رنگ بگیرد، در چهار پله.

سؤال این بود: کدام‌شان را بسازیم، و چرا.

جواب کوتاه: **هیچ‌کدام‌شان مشکل را حل نمی‌کند، چون مشکل جای دیگری است.** اول آن را می‌گویم، بعد بقیه.

---

## ۱ — چیزی که از همهٔ یافته‌ها مهم‌تر درآمد

من فایل حساب پول تقویم را باز کردم: `/home/user/Chortke/app/src/engine/daymoney.js`.

آنجا برای هر ارز یک «سقف» حساب می‌شود. سقف یعنی: میانگین سه ماه قبل، ضربدر یک‌چهارم. اگر تاریخچه‌ای نباشد، جمعِ خودِ همان ماه ضربدر یک‌چهارم.

حالا فرض کن تو دو تعهد در یک ماه وارد کرده‌ای. سهم هر روز حدود نصف جمع ماه است. سقف یک‌چهارم جمع ماه است. یعنی هر دو روز از سقف رد می‌شوند، و کد هر دو را روی بیشینه می‌بندد.

**نتیجه: هر دو روز نوار بیشینه و ته‌رنگ بیشینهٔ یکسان می‌گیرند.** دو مبلغ کاملاً متفاوت، دقیقاً یک شکل.

این دقیقاً همان حالتی است که تو آزمودی. رمزگذاری از نظر ریاضی خراب بود و هیچ اطلاعاتی حمل نمی‌کرد — مستقل از اینکه نوار باشد یا نقطه یا ته‌رنگ.

هیچ‌کدام از سه شکل الف و ب و پ این را درست نمی‌کند. اگر قاعدهٔ سقف عوض نشود، هر سه در همان آزمون دوتعهدی دوباره رد می‌شوند.

نکتهٔ دوم از همان فایل: نوار پرداختی فقط شش ارتفاع ممکن دارد (از ۲ تا ۷ پیکسل) و نوار دریافتی سه‌تا (از ۲ تا ۴). یعنی اختلاف یک پیکسل، در خانه‌هایی که ۴۹ پیکسل از هم فاصله دارند. این را پایین توضیح می‌دهم که چرا بدترین حالت ممکن است.

---

## ۲ — خانه جا دارد؛ خیلی بیشتر از آنچه فکر می‌کردیم

اول یک عدد غلط را تصحیح کنم. در دور اول پژوهش گفته شده بود «سه سطر متن در خانه جا نمی‌شود، چون فاصلهٔ خطوط وزیرمتن زیاد است». این غلط بود. مخزن خودت سال‌هاست فاصلهٔ خط را روی یک قفل کرده (`styles.css` خط ۶۶۹ و ۶۸۲، و `tokens.css` خط ۸۴).

بودجهٔ واقعی این است:

خانه ۶۶ پیکسل ارتفاع دارد. عدد شمسی ۱۹ پیکسل، فاصله ۸ پیکسل، عدد میلادی ۱۱ پیکسل. جمعاً ۳۸ پیکسل از ۶۴ پیکسل محتوا.

**یعنی ۲۶ پیکسل همین حالا خالی است.** و اگر عدد میلادی از خانه برود، ۴۵ پیکسل خالی می‌شود.

پس «جا نیست» بهانه نیست. خانهٔ چرتکه از خانهٔ اپ‌های واقعی بزرگ‌تر است: اپ دفترچه‌حساب ژاپنی `kakeibo_v3` تاریخ و دو سطر پول را در خانهٔ ۴۶ در ۵۴ پیکسل جا داده. کتابخانهٔ تقویم شمسی رایج ری‌اکت، خانه را زیر عرض ۴۲۰ پیکسل به ۳۲ در ۳۲ می‌برد.

مشکل کمبود جا نیست. مشکل این است که سه چیز هم‌زمان دارند سر یک نقش دعوا می‌کنند.

**عدد میلادی گران‌ترین چیز داخل خانه است.** دو مرجع فارسی مستقل، هیچ‌کدام آن را طوری که ما گذاشته‌ایم نمی‌گذارند:

- اپ مرجع تقویم فارسی (`persian-calendar`) عدد میلادی را کوچک و **بالای** عدد اصلی می‌گذارد، و فضای **زیر** عدد اصلی را برای نشانه‌ها نگه می‌دارد. ما دقیقاً برعکسش کرده‌ایم — عدد میلادی پایین، پول ته خانه.
- `persian-datepicker` عدد میلادی را ۸٫۵ پیکسل در گوشه می‌گذارد، به رنگ تقریباً نامرئی، و **پیش‌فرض خاموش**.

---

## ۳ — مبلغ کامل داخل خانه جا نمی‌شود؛ این را اندازه گرفتم

روی همان فایل قلمی که اپ استفاده می‌کند (`Vazirmatn-Regular.woff2`) پهنای واقعی را اندازه گرفتم. جعبهٔ داخلی خانه ۴۵ پیکسل است.

| مبلغ | پهنا (پیکسل) |
|---|---|
| ۴٬۵۰۰٬۰۰۰ | ۵۶٫۴ |
| ۳٬۳۳۳٬۳۳۳ | ۵۶٫۴ |
| ۲٬۸۷۵٬۰۰۰ | ۴۴٫۸ |
| ۴٬۵۰۰ | ۲۴٫۹ |

یعنی مبلغ هفت‌رقمی تومان در خانه جا نمی‌شود. و بدتر: **جاشدنش به خودِ رقم‌ها بستگی دارد.** بعضی مبلغ‌ها جا می‌شوند و بعضی نه، و پهنا ستون‌به‌ستون تا ۳۵ درصد نوسان می‌کند. یعنی حتی اگر یک مبلغ جا شود، ستون تقویم ناهموار می‌شود.

راه فرارِ معمول هم برای فارسی بسته است. در انگلیسی «4.5M» چهار نویسه است. در ژاپنی «450万» چهار نویسه است. در فارسی استاندارد یونیکد اصلاً شکل کوتاه ندارد — خروجی می‌شود «۴٫۵ میلیون»، ده نویسه، که از خودِ عدد بلندتر است. برای ۸۵۰٬۰۰۰ هم «۸۵۰ هزار» هشت نویسه است در برابر هفت نویسهٔ عدد خام. کوتاه‌سازی در فارسی متن را **بلندتر** می‌کند.

می‌ماند یک راه: **واحد را عوض کنیم.** یعنی در سربرگ ماه یک بار بنویسیم «ارقام به هزار تومان» و در خانه‌ها بنویسیم ۴٬۵۰۰. آن عدد ۲۴٫۹ پیکسل است و راحت جا می‌شود.

ولی این یک تصمیم متنی است، نه یک جزئیات فنی. امضای تو را می‌خواهد.

---

## ۴ — ته‌رنگ در چرتکه یک کانال مرده است

اینجا یک اشتباه دیگر را هم تصحیح کنم: من فکر می‌کردم ته‌رنگ برنجی است. نیست. `styles.css` خط ۶۹۷ می‌گوید ته‌رنگ از رنگ `--bar-todo` می‌آید، که یک خاکستری است.

قاعدهٔ دسترس‌پذیری می‌گوید هر شکلی که معنی دارد باید دست‌کم سه به یک با پس‌زمینه‌اش فرق داشته باشد. عددهای واقعی ته‌رنگ چرتکه:

| شفافیت | تم تیره | تم روشن |
|---|---|---|
| ۰٫۳۰ | ۱٫۱۴ | ۱٫۰۷ |
| ۰٫۵۰ | ۱٫۲۶ | ۱٫۱۳ |
| ۰٫۷۰ | ۱٫۴۰ | ۱٫۱۸ |
| ۱٫۰۰ | ۱٫۶۵ | ۱٫۲۸ |

**ته‌رنگ در هیچ شفافیتی، حتی صد درصد، به سه به یک نمی‌رسد.** نه فقط پله‌های پایین — کل کانال مرده است. تو داری به چیزی نگاه می‌کنی که تقریباً نامرئی است.

نوارها برعکس‌اند: رنگشان نسبت به خانه ۵٫۵۱ در تیره و ۴٫۶۷ در روشن است، یعنی رنگ نوار کاملاً سالم است. مشکل نوار، اختلاف یک‌پیکسلی ارتفاعش است.

(یک استثنا: نوار «انجام‌شده» با رنگ فعلی‌اش ۱٫۶۵ و ۱٫۳۵ است، یعنی آن هم عملاً نامرئی است.)

و اگر بخواهیم ته‌رنگ را با برنج بسازیم، باز محدودیم: چهار پلهٔ برنجی، اختلاف پله‌های کنار هم می‌شود ۱٫۵۸ و ۱٫۵۲ و ۱٫۴۵. هیچ‌کدام نزدیک سه به یک نیست. با یک رنگ، ته‌رنگ فقط می‌تواند بگوید «هست یا نیست» — نمی‌تواند بگوید «چقدر».

---

## ۵ — نوارِ متغیرِ ارتفاع، بدترین انتخاب ممکن است

دو مقالهٔ آزمایشگاهی این را با عدد می‌گویند.

**اول:** مطالعه‌ای در سال ۲۰۰۹ اندازه گرفت که آدم‌ها تا چه اندازهٔ کوچکی می‌توانند ارتفاع را بخوانند. جواب: **کف ۲۴ پیکسل.** زیر آن، خطای تخمین با هر نصف‌شدن اندازه خطی بالا می‌رود. نوار چرتکه بین ۲ تا ۷ پیکسل است — کل بازه‌اش زیر آن کف است.

جالب اینکه ۲۴ پیکسل الان جا می‌شود (۲۶ پیکسل آزاد داریم). ولی آن‌وقت در هر خانه یک نوار غول‌پیکر داریم. یعنی «نوار را بلندتر کن» جواب نیست.

**دوم:** مطالعه‌ای در سال ۲۰۱۴ مقایسهٔ نوارها را آزمود. کوتاه‌ترین نواری که تا حالا آزموده شده ۱۲۵ پیکسل است، و همان‌جا هم می‌گوید وقتی نوارها از هم جدا باشند مقایسه‌شان سخت است. نوارهای چرتکه هجده تا شصت برابر کوتاه‌ترند و هر کدام در خانهٔ خودشان، جدا از هم. سه جریمه روی هم.

**سوم، از همان مقالهٔ ۲۰۰۹:** تعداد پله‌های شدت را هم آزمودند. دو یا سه پله خطای یکسان داد (میانگین ۴٫۱۲ و ۴٫۰۴)، ولی **چهار پله به‌طور معنادار بدتر شد** (میانگین ۵٫۶۴) و شرکت‌کننده‌ها گفتند خسته‌کننده است.

این مستقیم به شکل «پ» می‌خورد، که دقیقاً چهار پله است.

(نکتهٔ صداقت: عدد آماری که در نقل‌قول این یافته آمده بود از نظر ریاضی ناممکن است — یک جای رونویسی خراب شده. خودِ ادعا و میانگین‌ها معتبرند؛ آن یک عدد را جایی نقل نکنیم.)

**چهارم:** همان مقاله ته‌رنگ را مستقیم با ارتفاع مقایسه کرد، و **ته‌رنگ باخت**. یعنی اگر نوار را حذف کنیم و به‌جایش ته‌رنگ را پررنگ‌تر کنیم، عقب رفته‌ایم نه جلو.

---

## ۶ — اپ‌های واقعی چه می‌کنند

کتابخانه‌های تقویم عمومی هیچ‌کدام مبلغ در خانه نمی‌گذارند. خانه یا می‌گوید «چیزی هست» یا می‌گوید «چندتا». هیچ‌کدام نمی‌گوید «چقدر».

ولی اپ‌های دفترچه‌حساب خانگی این کار را می‌کنند، و ارزش نگاه‌کردن دارند:

- `kakeibo_v3` (ژاپنی): مبلغ را هم‌اندازهٔ تاریخ و پررنگ‌تر از آن می‌گذارد. یعنی چشم اول روی پول می‌نشیند نه روی تاریخ. ولی مبلغ روزانهٔ ژاپنی چهار تا شش رقم است، نه هفت.
- `harumoa` (کره‌ای): مبلغ کامل را ساخت، بعد **زیر عرض ۷۸۰ پیکسل کاملاً خاموشش کرد** و خانه را به یک نقطهٔ خاکستری ۱۰ در ۱۰ تقلیل داد. آن‌ها در دو برابر عرض ما تسلیم شدند.
- `ikuyo_finance`: تعداد قلم‌های روز را با یک تا سه نقطهٔ ۴ در ۴ پیکسل نشان می‌دهد، و بالای سه‌تا یک برچسب «۳+».
- `habp-main`: خواست سه عدد در خانه بگذارد و مجبور شد قلم را به ۸ و بعد ۷ پیکسل ببرد. یعنی هر مقدار اضافه حدود سه پیکسل از اندازهٔ قلم می‌خورد.

و یک نکتهٔ مهم دربارهٔ جهت پول: **هیچ‌کدام از این چهارتا جهت را با موقعیت نشان نمی‌دهد.** همه یک نشانهٔ کوچک قبل از عدد می‌گذارند. چرتکه الان جهت را از جای نوار می‌گیرد (کف خانه یعنی پرداختی، بالای خانه یعنی دریافتی — `styles.css` خط ۶۸۷ تا ۷۰۳). با دو تعهد، این کد یادگرفتنی نیست: کاربر یک نوار بالا و یک نوار پایین می‌بیند و هیچ چیزی به او نمی‌گوید چرا.

---

## توصیه دربارهٔ الف و ب و پ

### الف — مبلغ داخل خانه

**رد.** با اندازه‌گیری مرده است. مبلغ هفت‌رقمی تومان در ۴۵ پیکسل جا نمی‌شود، نه جدولی و نه تناسبی. کوتاه‌سازی فارسی متن را بلندتر می‌کند. و «۴٫۵م» را من هیچ‌جا سابقه‌دار پیدا نکردم — اختراع خودمان می‌شود.

فقط یک شکلش زنده است: **تغییر واحد.** عدد حداکثر پنج‌نویسه‌ای مثل ۴٬۵۰۰، با واحدی که یک بار در سربرگ ماه گفته می‌شود. این تصمیمِ توست، نه تصمیم من.

### پ — ته‌رنگ در چهار پله

**رد، و با فاصلهٔ زیاد.** شش خط شاهد مخالفش است: چهار پله آزمایشگاهی بدتر از سه است؛ ته‌رنگ فعلی در هیچ شفافیتی دیده نمی‌شود؛ ته‌رنگ برنجی هم پله‌هایش از هم قابل تفکیک نیست؛ ته‌رنگ در مقایسهٔ مستقیم از ارتفاع باخت؛ و تنها اپی که این کار را می‌کند (`Timepage`) آن را در یک صفحهٔ جدا گذاشته و منتقدی که سعی کرد توضیحش دهد نتوانست با کلمه توصیفش کند.

تنها یک خط شاهد موافقش بود، همان `Timepage`، و ضعیف.

### ب — نشانهٔ کوچک در خانه، مبلغ در پنل

**این را بساز — ولی نه نقطهٔ خالی.**

قوی‌ترین پایهٔ شاهد را دارد. ولی یک هشدار جدی هست: `harumoa` دقیقاً همین کار را کرد — نقطهٔ بی‌کمیت به‌علاوهٔ پنل — و آن همان چیزی است که تو «ابتدایی» خواندی. اپ مرجع فارسی هم نشانه‌های خانه را برای «نوع» به کار می‌برد نه برای «مقدار»، یعنی کاربر ایرانی نقطهٔ خانه را «چیزی هست» می‌خواند.

پس «ب» فقط در یک شکل زنده است: **نشانه باید شمارش‌پذیر باشد.**

یعنی: یک تا سه نشانهٔ ۴ تا ۵ پیکسلی، که تعدادشان **تعداد قلم‌های آن روز** را بگوید نه مبلغ را. بالای سه‌تا، یک نشانه با علامت اضافه. این عدد در مقیاس تو (ده تا سی تعهد در ماه، اوج چهل) تقریباً همیشه یک یا دو یا سه است — یعنی دقیقاً بازه‌ای که شمردنی است و ته‌رنگ در آن هیچ حرفی ندارد.

و در حالت راست‌به‌چپ، نشانه‌ها از راست به چپ رشد می‌کنند.

### مشترک بین هر سه

جهت پول باید از موقعیت عمودی برداشته شود و به یک نشانهٔ ۸ تا ۱۰ پیکسلی کنار عدد منتقل شود.

### ترتیب کار

۱. **اول قاعدهٔ سقف را درست کن** (`daymoney.js`). بدون این، هر سه شکل دوباره رد می‌شوند.
۲. ته‌رنگ را کامل بردار. کانال مرده است.
۳. نوارِ متغیرِ ارتفاع را بردار و جایش نشانهٔ شمارش‌پذیر بگذار.
۴. عدد میلادی را از خانه بردار یا به گوشه ببر — هر دو مرجع فارسی همین کار را می‌کنند، و ۱۹ پیکسل آزاد می‌شود.
۵. جهت پول را با نشانه نشان بده نه با جای نوار.
۶. تغییر واحد مبلغ (شکل الف) را جدا و بعداً تصمیم بگیر، چون امضای متنی می‌خواهد.

---

## چه چیزی را نتوانستیم ثابت کنیم

- **هیچ صفحه‌ای را نتوانستیم ببینیم.** مرورگر این ماشین به اینترنت وصل نمی‌شود، پس همه‌چیز از روی متن است: کد منبع، سند کتابخانه، مقاله. هیچ اسکرین‌شاتی در کار نبود.
- **نمی‌دانیم خوانندهٔ ایرانی «۴٫۵م» را ۴٫۵ میلیون می‌خواند یا نه.** هیچ کتابخانهٔ فارسی، و خود دادهٔ استاندارد یونیکد، شکل تک‌حرفی ندارد. این یعنی اگر بسازیمش، اختراع خودمان است.
- **ادعای «کف اندازهٔ قلم ۱۱ پیکسل است» را نتوانستیم به منبع رسمی وصل کنیم.** صفحهٔ مرجع بدون متن برگشت. اندازه‌ها را نگه می‌داریم ولی به‌عنوان استاندارد نقلش نکنیم.
- **هیچ اپ منتشرشده‌ای پیدا نشد که مبلغ پول را در خانهٔ تقویم به عرض گوشی بگذارد.** نه اینکه بد است — اینکه سابقه ندارد.
- **دربارهٔ شکل «پ» فقط یک شاهد موافق پیدا شد**، و آن هم اپی است که همان کار را در صفحه‌ای جدا انجام می‌دهد. اگر می‌خواهی «پ» بماند، شاهدش این‌قدر است و بیشتر نشد.

---

## منابع

اندازه‌گیری‌های قلم، تضادهای رنگ و منطق سقف، همه روی فایل‌های خود مخزن انجام شد:

```
/home/user/Chortke/app/src/engine/daymoney.js
/home/user/Chortke/app/src/ui/CalendarView.jsx
/home/user/Chortke/app/src/styles.css
/home/user/Chortke/app/src/tokens.css
/home/user/Chortke/app/public/fonts/Vazirmatn-Regular.woff2
```

منابع بیرونی:

- کف ۲۴ پیکسل، مقایسهٔ ته‌رنگ با ارتفاع، و تعداد پله‌ها
  `https://idl.cs.washington.edu/files/2009-TimeSeries-CHI.pdf`

- سختی مقایسهٔ نوارهای کوتاه و جدا از هم
  `http://vis.cs.ucdavis.edu/vis2014papers/TVCG/papers/2152_20tvcg12-talbot-2346320.pdf`

- قاعدهٔ سه به یک برای شکل‌های معنادار
  `https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html`

- قاعدهٔ اختلاف با رنگ همسایه و شکاف هم‌رنگ سطح
  `https://design.gitlab.com/data-visualization/color`

- اپ مرجع تقویم فارسی — جای عدد میلادی و نشانه‌ها
  `https://raw.githubusercontent.com/persian-calendar/persian-calendar/main/PersianCalendar/src/main/kotlin/com/byagowi/persiancalendar/ui/calendar/calendarpager/DayPainter.kt`

- عدد میلادی به‌عنوان زیرنویس گوشه، پیش‌فرض خاموش
  `https://cdn.jsdelivr.net/npm/persian-datepicker@1.2.0/dist/css/persian-datepicker.css`

- خانهٔ ۳۲ پیکسلی زیر عرض ۴۲۰
  `https://cdn.jsdelivr.net/npm/react-multi-date-picker@4/styles/layouts/mobile.css`

- اپ ژاپنی — تاریخ و دو سطر پول در خانهٔ ۴۶ در ۵۴
  `https://github.com/puruwo/kakeibo_v3/blob/main/lib/view/historical_calendar_page/calendar_area/date_box.dart`

- اپ کره‌ای — خاموش‌کردن مبلغ زیر عرض ۷۸۰
  `https://github.com/wjdgml3092/harumoa/blob/main/src/components/calendar/DateBox.tsx`

- شمارش قلم‌ها با یک تا سه نقطه
  `https://github.com/Rizz404/ikuyo_finance/blob/main/lib/features/transaction/widgets/calendar_transaction_view.dart`

- هزینهٔ سه عدد در یک خانه — قلم ۸ و ۷ پیکسل
  `https://github.com/skyinthe-sea/habp-main/blob/main/lib/features/calendar/presentation/widgets/month_calendar.dart`

- سقف سه نقطه و هندسهٔ نقطهٔ ۴ در ۴
  `https://github.com/wix/react-native-calendars/issues/523`

- شمارش در خانه به‌عنوان جایگزین مستند نقطه
  `https://demo.mobiscroll.com/javascript/eventcalendar/mobile-month-view`

- یک نشانه در هر خانه، در کتابخانهٔ تقویم اپل
  `https://developer.apple.com/documentation/uikit/uicalendarview/decoration`

- نبودِ شکل کوتاه عدد در فارسی
  `https://raw.githubusercontent.com/unicode-org/cldr-json/main/cldr-json/cldr-numbers-full/main/fa/numbers.json`

- ته‌رنگ فقط در نمای سال، نشانهٔ شمردنی در نمای ماه
  `https://flexibits.com/fantastical-ios/help/calendar-views`

- تنها اپی که ته‌رنگ ماهانه دارد، و آن هم در صفحه‌ای جدا
  `https://bonobolabs.com/support/timepage/navigation/navigating-heat-map/`

- الگوی دسترس‌پذیر: خانه حالت کلی، عدد در فهرست کنارش
  `https://github.blog/changelog/2023-03-02-accessibility-improvements-for-the-contribution-graph/`

- افت پلهٔ ته‌رنگ در نمودار مشارکت گیت‌هاب
  `http://adrianroselli.com/2018/02/github-contributions-chart.html`', 'verified': '## کشته‌ها — ۹ یافته که وزن ندارند

**۲ — `dayMaxEventRows` و شمردن لینک `+more`.** واقعیت کتابخانه درست است ولی برای چرتکه بی‌اثر: چرتکه هیچ‌وقت ردیف رویداد در خانه نمی‌گذارد، پنل زیر شبکه دارد. خودِ پژوهشگر هم اعتراف کرده عدد «۲۴ تا ۳۰ پیکسل» را از حساب خودش درآورده نه از کتابخانه. و آن حساب حالا با اندازه‌گیری مستقیم جایگزین شده (پایین ببین).

**۳۵ — «`line-height` طبیعی وزیرمتن ۱٫۵۶ تا ۱٫۷۱ ‌ام است، سه سطر جا نمی‌شود».** غلط برای این مخزن. `styles.css:669` و `:682` هر دو `line-height: var(--leading-2)` دارند و `--leading-2: 1` است (`tokens.css:84`). اهرمی که پیشنهاد داده، سال‌هاست کشیده شده. بودجهٔ واقعی: `19 + 8 + 11 = 38px` از `64px` محتوا — یعنی **`26px` همین حالا آزاد است**. نتیجه‌گیری‌اش («جا برای سطر سوم نیست») ضد واقعیت مخزن است.

**۲۷ — «`tabular-nums` ارث می‌رسد، عنصر پول باید `proportional-nums` اعلام کند».** بررسی کردم: `font-variant-numeric` فقط روی کلاس‌های برگ نشسته (`.link-code` `.tile-main` `.tile-second` `.tile-approx` `.row-amount` `.cal-day` `.cal-greg-day` `.cal-sum-amounts` `.item-amount` `.item-toman`) و روی هیچ جدِ `.cal-cell` نیست. یک `.cal-money` تازه خودبه‌خود تناسبی می‌شود. مهم‌تر: ادعای «مبلغ کامل جا می‌شود» فقط برای رقم‌های صفردار درست است. اندازه گرفتم روی همان `Vazirmatn-Regular.woff2`: `۴٬۵۰۰٬۰۰۰` تناسبی `41.7px` ولی `۳٬۳۳۳٬۳۳۳` تناسبی `56.4px` و `۲٬۸۷۵٬۰۰۰` تناسبی `44.8px` — جعبه `45px` است. یعنی جاشدن به رقم‌ها بستگی دارد و نوسان ستون‌به‌ستون `۳۵٪` است. نتیجه‌گیری‌اش («ایراد پهنا نیست، خوانایی است») باطل.

**۳۷ — `WCAG 2.5.8` و `24×24`.** به سؤالی جواب می‌دهد که کسی نپرسیده. هیچ‌کس پیشنهاد بزرگ/کوچک‌کردن خانه نداده.

**۳۱ — تکرار عینی ۹.** **۴۰ — تکرار ۳ و ۴.** وزن مستقل ندارند.

**۲۸ — `react-multi-date-picker` با خانهٔ `32px`.** خانهٔ یک date-picker از اساس هیچ محتوایی ندارد؛ نمی‌تواند دربارهٔ ظرفیت محتوا شهادت بدهد. یافتهٔ ۲۰ همین حرف را با پایهٔ بهتری می‌زند.

**۴۳ — «شبکه را نگه دار».** هر سه شکل کاندیدا شبکه را نگه می‌دارند. تفکیک‌کننده نیست.

**۳۰ — `ss01`.** درست است، ولی یادداشت نگهداری مخزن است نه ورودی این تصمیم.

## اصلاح‌شده‌ها — عدد غلط، نتیجه درست‌تر از خودشان

**۳۳ — ته‌رنگ روی رنگ اشتباه حساب شده.** ته‌رنگ چرتکه برنجی نیست؛ `styles.css:697` می‌گوید `background: var(--bar-todo)` و آن `#3A4245` تیره و `#C9CAC5` روشن است — یک خاکستری. نوارها هم `--ink-2` اند نه برنج. عددهای واقعی که خودم حساب کردم، نسبت به `--cell`:

| حالت | `0.30` | `0.50` | `0.70` | `1.00` |
|---|---|---|---|---|
| تیره | `1.14` | `1.26` | `1.40` | `1.65` |
| روشن | `1.07` | `1.13` | `1.18` | `1.28` |

یعنی ته‌رنگ **در هیچ شفافیتی، حتی صد درصد، به `3:1` نمی‌رسد**. یافتهٔ ۳۳ گفته بود کف بازه مرده است؛ حقیقت این است که کل کانال مرده است. (نوارها برعکس دیده می‌شوند: `--ink-2` نسبت به خانه `5.51` تیره و `4.67` روشن است — پس مشکل نوار، رنگش نیست، اختلاف ۱‌پیکسلی ارتفاعش است. نوارِ «انجام‌شده» با `--mark-paid` باز `1.65` و `1.35` است، یعنی آن هم نامرئی.)

**۱۷ — کف `24px` برای رمزگذاری ارتفاع.** خودِ آستانه از مقالهٔ `Heer` معتبر است، ولی حساب کاربردش غلط بود چون فرض کرده بود `line-height` طبیعی است. `26px` همین حالا آزاد است و اگر عدد میلادی برود `45px`. پس «`24px` جا نمی‌شود» درست نیست — «`24px` جا می‌شود ولی آن‌وقت یک نوار غول‌پیکر در هر خانه داری» درست است.

**۷ — «هیچ کتابخانه‌ای مبلغ را در خانه نمی‌گذارد».** با یافته‌های ۱۰ و ۱۳ و ۱۴ در همین فهرست تناقض دارد: `kakeibo_v3` و `ikuyo_finance` و `habp-main` دقیقاً همین کار را می‌کنند. حل تناقض: این حکم دربارهٔ **تقویم‌های عمومی و کتابخانه‌ها** درست است و دربارهٔ **اپ‌های دفترچه‌حساب خانگی** غلط. چرتکه از نوع دوم است. پس ۷ باید باریک شود و وزنش علیه «الف» کم می‌شود.

**۱۹ — چهار پله بدتر از سه.** میانگین‌ها (`4.12` / `4.04` / `5.64`) ادعا را می‌برند، ولی آمارِ نقل‌شده ناممکن است: `F(2,34) = 58.27` هرگز `p = 0.013` نمی‌دهد. یک جای رونویسی خراب شده. ادعا می‌ماند، عدد `p` را نقل نکنید.

**۳۶ — کف `11px`.** اندازه‌ها درست‌اند، ولی استناد به `Apple HIG` را همان تیم دیگر رد کرده (صفحه بدون متن برگشته بود). کف را به‌عنوان استاندارد نقل نکنید.

**۱ و ۱۲ و ۴ — تضعیف.** یافتهٔ ۱ محدودیت `API` است نه شاهد ادراکی، و بین هر سه شکل تفکیک نمی‌کند (هر سه تک‌نشانه‌اند)؛ فقط وضع موجود را می‌کشد. یافتهٔ ۱۲ خانه‌ای را خاموش کرده که فهرست و جمع و دفترچه و دکمه داشت، نه یک عدد چهارحرفی — پس شکست «یک عدد» را ثابت نمی‌کند. یافتهٔ ۴ نقطه‌شکن‌ها را قاعدهٔ خوانایی گرفته درحالی‌که ردهٔ پیکربندی‌اند.

## بازمانده‌های پروزن

۸، ۹، ۱۰، ۱۱، ۱۳، ۱۴، ۱۵، ۱۶، ۱۸، ۲۰، ۲۱، ۲۲، ۲۳، ۲۴، ۲۵، ۲۶، ۲۹، ۳۲، ۳۴، ۳۸، ۳۹، ۴۲.

سه‌تایشان را خودم روی فایل‌های خود مخزن تأیید کردم و درست‌اند: پهنای رقم‌های وزیرمتن (`۱ = 0.2886` تا `۳ = 0.65869 em`، جداکنندهٔ `٬ = 0.26025`)، تبدیل `tnum` به `0.65869` برای همه (پس `۱۱` و `۳۱` هر دو دقیقاً `25.03px` در `19px`)، و کنتراست‌های پلهٔ برنجی (چهار پله: `1.58` و `1.52` و `1.45` — هیچ‌کدام نزدیک `3:1`).

## تصمیم برای الف، ب، پ

**الف — مبلغ داخل خانه، به‌جای عدد میلادی**
- شاهد موافق: ۱۰ (`kakeibo` در `46×54` تاریخ و دو سطر پول را جا داده)، ۱۳، ۱۵، ۲۴، ۲۶، و اندازه‌گیری خودم که می‌گوید جای عمودی هست (`26px` حالا، `45px` بدون عدد میلادی).
- شاهد مخالف: ۸ و ۳۲ و ۳۶ — مبلغ تومانی هفت‌رقمی در `45px` جا نمی‌شود، نه جدولی (`56.4px`) و نه در بدترین حالت تناسبی (`56.4px`). ۹ — فارسی فرم فشرده ندارد و «۴٫۵ میلیون» از خود عدد بلندتر است. ۳۸ — «۴٫۵م» اختراع است و سابقه ندارد. ۱۴ — بیش از یک کمیت، اندازهٔ قلم را به `8px` می‌برد.
- **حکم: الف در شکل تحت‌اللفظی‌اش (چاپ مبلغ تومان) با اندازه‌گیری مرده است.** فقط در یک شکل زنده است: تغییر واحد، عدد حداکثر پنج‌حرفی مثل `۴٬۵۰۰` با واحد که یک‌بار در سربرگ ماه گفته شود. و آن یک تصمیم متنی است که امضای مالک می‌خواهد، نه یک جزئیات رندر.

**ب — یک نقطهٔ کوچک، مبلغ فقط در پنل**
- شاهد موافق و پرتعداد: ۱، ۳، ۵، ۶، ۱۳، ۲۳، ۲۴، ۲۹، ۳۹، ۷.
- شاهد مخالف: خودِ مالک. فالبکِ `harumoa` در یافتهٔ ۱۲ دقیقاً «نقطهٔ بی‌کمیت + پنل» است و همان چیزی است که او «ابتدایی» خواند. یافتهٔ ۲۹ هم هشدار می‌دهد که کاربر ایرانی نقطهٔ خانه را «چیزی هست» می‌خواند، نه مقیاس.
- **حکم: قوی‌ترین پایهٔ شاهد را دارد، ولی فقط اگر نقطه شمارش‌پذیر باشد** (یک تا سه نشانه، یا یک رقم شمارش) نه نقطهٔ حضورِ تهی. `4px` تا `4.9px` قطر و سقف سه‌تا، از دو منبع مستقل (۶ و ۲۴) و راست‌به‌چپ‌شدنش از ۲۹.

**پ — ته‌رنگ کل خانه در چهار پله**
- مخالف: ۱۹ (دقیقاً چهار پله، به‌طور معنادار بدتر از سه)، ۱۸، ۲۱، ۲۲، ۳۴، ۳۹، و ۳۳ در نسخهٔ اصلاح‌شده‌اش که سقف `1.65` و `1.28` را نشان می‌دهد.
- موافق: فقط ۴۱ (`Timepage`) — آن هم در صفحه‌ای جدا، و منتقدی که نتوانست رمزگذاری‌اش را با کلمه توصیف کند.
- **حکم: بدترین‌پشتیبانی‌شدهٔ هر سه. شش خط شاهد مخالف، یک خط ضعیف موافق.** اگر بماند، فقط دو حالت، با رنگ برنجی نه `--bar-todo`، با شکاف هم‌رنگ سطح بین خانه‌ها، و دو رمپِ جدا برای تم روشن و تیره.

**مشترک بین هر سه:** یافتهٔ ۱۱ روی مخزن تأیید می‌شود (`styles.css:687-703`: `cal-pay` کف، `cal-receive` سقف). هر شکلی که برنده شود، جهت پول باید با یک نشانهٔ `8-10px` بیاید نه با موقعیت عمودی.

## چیزی که هیچ‌کدام از ۴۳ یافته ندید — و از همه‌شان تصمیم‌سازتر است

مشکل در نگاشت نیست، در **سقف** است. `daymoney.js` سقف هر ارز را چنین می‌گیرد: میانگین سه ماه جلالی قبل، ضربدر `CAP_SHARE = 0.25`؛ و اگر تاریخچه نباشد، جمعِ خودِ ماهِ نمایش ضربدر `0.25`. با دو تعهد در یک ماه، سهم هر روز حدود `۵۰٪` جمع ماه است، `Math.min(1, sum / cap)` هر دو را روی `1` می‌بندد، و **هر دو روز نوار بیشینه و ته‌رنگ بیشینهٔ یکسان می‌گیرند**. یعنی در همان شرایطی که مالک آزمود، رمزگذاری از نظر ریاضی تباه است و هیچ اطلاعاتی حمل نمی‌کند — مستقل از اینکه نوار باشد یا نقطه یا ته‌رنگ.

نوارِ پرداختی هم فقط شش مقدار ممکن دارد (`PAY_BAR 2..7`، با `Math.round`) و دریافتی سه مقدار (`2..4`). اختلاف یک پیکسل، در خانه‌های `49px` از هم جدا — همان چیزی که یافتهٔ ۱۶ می‌گوید بدترین حالت ممکن است.

**اگر قاعدهٔ سقف عوض نشود، هر سه شکل الف و ب و پ در آزمون دو‌تعهدیِ مالک دوباره رد می‌شوند.**

فایل‌های مربوط: `/home/user/Chortke/app/src/engine/daymoney.js`، `/home/user/Chortke/app/src/ui/CalendarView.jsx`، `/home/user/Chortke/app/src/styles.css`، `/home/user/Chortke/app/src/tokens.css`.', 'findingCount': 44, 'angles': ['mobile-month', 'money-in-cell', 'magnitude-encoding', 'persian-rtl', 'small-space', 'alternatives']}, 'workflowProgress': [{'type': 'workflow_phase', 'index': 1, 'title': 'گشتن'}, {'type': 'workflow_phase', 'index': 2, 'title': 'وارسی'}, {'type': 'workflow_phase', 'index': 3, 'title': 'جمع‌بندی'}, {'type': 'workflow_agent', 'index': 1, 'label': 'زاویه: mobile-month', 'phaseIndex': 1, 'phaseTitle': 'گشتن', 'agentId': 'af9d9a314694309df', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788461968746, 'queuedAt': 1788461967067, 'attempt': 1, 'lastToolName': 'StructuredOutput', 'lastToolSummary': 'Three things I looked for and did not find. (1) No source s…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788462201016, 'tokens': 68932, 'toolCalls': 22, 'durationMs': 231200, 'resultPreview': '{"findings":[{"claim":"Apple\'s own system month-calendar component permits exactly ONE decoration per day cell — the API cannot express two stacked visual encodings for one day.","specifics":"UICalendarViewDelegate exposes a single method, calendarView(_:decorationFor:) -> UICalendarView.Decoration?, returning one optional Decoration per date. A Decoration is one of three kinds only: default (a co…'}, {'type': 'workflow_agent', 'index': 2, 'label': 'زاویه: money-in-cell', 'phaseIndex': 1, 'phaseTitle': 'گشتن', 'agentId': 'a9b949310d11f5c74', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788461968706, 'queuedAt': 1788461967067, 'attempt': 1, 'lastToolName': 'StructuredOutput', 'lastToolSummary': 'Four things this angle looked for and did not get. 1. NO us…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788462556221, 'tokens': 114980, 'toolCalls': 39, 'durationMs': 587514, 'resultPreview': '{"findings":[{"claim":"A full 7-digit grouped toman amount physically cannot fit a 49px cell in Chortke\'s own font at any legible size. Measured from the repo\'s Vazirmatn: tabular digits are 0.6694 em EACH (the width of the widest digit ۳), and the separator ٬ is 0.2676 em, so ۴٬۵۰۰٬۰۰۰ = 5.207 em wide.","specifics":"Parsed /home/user/Chortke/app/node_modules/vazirmatn/misc/Non-Latin/fonts/ttf/Vaz…'}, {'type': 'workflow_agent', 'index': 3, 'label': 'زاویه: magnitude-encoding', 'phaseIndex': 1, 'phaseTitle': 'گشتن', 'agentId': 'a6c85ff21fd078750', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788462204574, 'queuedAt': 1788461967067, 'attempt': 1, 'lastToolName': 'StructuredOutput', 'lastToolSummary': 'Five real gaps, stated plainly rather than papered over. (1…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788462626623, 'tokens': 87657, 'toolCalls': 30, 'durationMs': 422047, 'resultPreview': '{"findings":[{"claim":"The 2–7px bar is the single worst-supported choice available: the bar-chart perception literature\'s SHORTEST tested bar is 125 pixels, and even there, separation already degrades accuracy substantially. Chortke\'s bars are 18–60× shorter than anything ever measured, and separated.","specifics":"Talbot, Setlur & Anand, IEEE TVCG 20(12):2152–2160, 2014, Experiment 1. Reference …'}, {'type': 'workflow_agent', 'index': 4, 'label': 'زاویه: persian-rtl', 'phaseIndex': 1, 'phaseTitle': 'گشتن', 'agentId': 'a2e301da514749dd3', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788462559200, 'queuedAt': 1788461967067, 'attempt': 1, 'lastToolName': 'StructuredOutput', 'lastToolSummary': 'Four honest gaps. 1. No documented convention for dual-cale…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788462984985, 'tokens': 100263, 'toolCalls': 34, 'durationMs': 425783, 'resultPreview': '{"findings":[{"claim":"The reference Iranian calendar app (persian-calendar, ~12k commits, F-Droid + Play) puts the SECONDARY (Gregorian) day number ABOVE the main number as a small \'header\', and reserves the space BELOW the main number for commitment indicators. Chortke has this exactly inverted — Gregorian below, money at the bottom — so the Gregorian number is occupying the slot the reference d…'}, {'type': 'workflow_agent', 'index': 5, 'label': 'زاویه: small-space', 'phaseIndex': 1, 'phaseTitle': 'گشتن', 'agentId': 'a6d8fbadeb342bf92', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788462629541, 'queuedAt': 1788461967067, 'attempt': 1, 'lastToolName': 'StructuredOutput', 'lastToolSummary': 'Three things I looked for and genuinely did not find, state…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788463042347, 'tokens': 86527, 'toolCalls': 27, 'durationMs': 412806, 'resultPreview': '{"findings":[{"claim":"Persian has NO compact number abbreviation in CLDR — Intl.NumberFormat(\'fa\', {notation:\'compact\'}) returns the identical string for compactDisplay \'short\' and \'long\'. The standard i18n escape hatch that gives English \'4.5M\' gives Persian \'۴٫۵ میلیون\', which is wider than the entire day cell.","specifics":"Measured locally, Node 22 / ICU 78.2, locales fa and fa-IR both resolv…'}, {'type': 'workflow_agent', 'index': 6, 'label': 'زاویه: alternatives', 'phaseIndex': 1, 'phaseTitle': 'گشتن', 'agentId': 'a9c03a251ef7622de', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788462987641, 'queuedAt': 1788461967068, 'attempt': 1, 'lastToolName': 'StructuredOutput', 'lastToolSummary': 'Several things this angle looked for and did not find, stat…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788463288921, 'tokens': 71502, 'toolCalls': 17, 'durationMs': 301279, 'resultPreview': '{"findings":[{"claim":"Fantastical — the iOS calendar most decorated for its design — uses opacity/heat encoding ONLY in its Year view, and uses discrete countable marks (colour-coded dots) at Month scale. Chortke has the two inverted.","specifics":"Flexibits\' own iOS help, page \"Calendar Views\": the Year view \"Uses a \'heat map\' where \'Days with a darker color will have more events than those w…'}, {'type': 'workflow_agent', 'index': 7, 'label': 'وارسی خصمانه', 'phaseIndex': 2, 'phaseTitle': 'وارسی', 'agentId': 'a97a3259764115b1f', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788463294902, 'queuedAt': 1788463290793, 'attempt': 1, 'lastToolName': 'Bash', 'lastToolSummary': 'cd /home/user/Chortke && for l in 264 316 323 340 508 790 1…', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788463695036, 'tokens': 105095, 'toolCalls': 13, 'durationMs': 400133, 'resultPreview': '## کشته‌ها — ۹ یافته که وزن ندارند

**۲ — `dayMaxEventRows` و شمردن لینک `+more`.** واقعیت کتابخانه درست است ولی برای چرتکه بی‌اثر: چرتکه هیچ‌وقت ردیف رویداد در خانه نمی‌گذارد، پنل زیر شبکه دارد. خودِ پژوهشگر هم اعتراف کرده عدد «۲۴ تا ۳۰ پیکسل» را از حساب خودش درآورده نه از کتابخانه. و آن حساب حالا با اندازه‌گیری مستقیم جایگزین شده (پایین ببین).

**۳۵ — «`line-height` طبیعی وزیرمتن ۱٫۵۶ تا ۱٫۷۱ ‌ا…'}, {'type': 'workflow_agent', 'index': 8, 'label': 'سند فارسی', 'phaseIndex': 3, 'phaseTitle': 'جمع‌بندی', 'agentId': 'a48cb3df1d2f25749', 'model': 'claude-opus-5', 'state': 'done', 'startedAt': 1788463698237, 'queuedAt': 1788463696447, 'attempt': 1, 'lastToolName': 'Read', 'lastToolSummary': '/home/user/Chortke/app/src/engine/daymoney.js', 'promptPreview': 'CONTEXT — a real app called Chortke (چرتکه). Findings must be judged against THESE constraints, not in the abstract:

- Persian-language, right-to-left, dark and light themes. Target device is a PHONE (tested at 360 and 390 CSS px wide), never a tablet.
- It has a month calendar tab. At 390px viewport, each day cell measures about 49 px wide by 66 px tall. That is measured, not estimated.
- Each c…', 'lastProgressAt': 1788463921117, 'tokens': 91330, 'toolCalls': 2, 'durationMs': 222879, 'resultPreview': '# تقویم چرتکه — نتیجهٔ پژوهش

## دنبال چه بودیم

تو گفتی تقویم «ابتدایی و نامفهوم» است. سه شکل جایگزین روی میز بود:

**الف** — مبلغ را داخل خانه بنویسیم، جای عدد میلادی.
**ب** — فقط یک نشانهٔ کوچک در خانه، و مبلغ فقط در پنل زیر شبکه.
**پ** — کل خانه ته‌رنگ بگیرد، در چهار پله.

سؤال این بود: کدام‌شان را بسازیم، و چرا.

جواب کوتاه: **هیچ‌کدام‌شان مشکل را حل نمی‌کند، چون مشکل جای دیگری است.** اول آن …'}], 'totalTokens': 726286, 'totalToolCalls': 184}
