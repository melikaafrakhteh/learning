# Fundamentals of Software Engineering From Coder to Engineer

## 1 Programmer to Engineer

توانایی کدنویسی فقط نقطه‌ی شروع است، نه تعریف مهندس نرم‌افزار بودن. مهندس باید کل چرخه‌ی توسعه نرم‌افزار، مسئله‌ی کسب‌وکار، کیفیت، نگهداری، معماری، امنیت و آدم‌های دیگری که با کد سروکار دارند را ببیند

Coding → Problem Solving → Design → Testing → Architecture → Production → Business → Communication

خواندن کد دیگران، فهمیدن codebase ناآشنا، تحمل ابهام، نوشتن کد قابل‌فهم و قابل‌نگهداری، یادگیری تکنولوژی جدید و مهارت‌های ارتباطی.

نکته‌ی مهم دیگر این است که لازم نیست قبل از تغییر یک codebase، صددرصد آن را بفهمی. در پروژه‌های واقعی تقریباً هیچ‌کس تمام جزئیات سیستم را نمی‌داند.

«برنامه‌نویس تنبل» باش، ولی از نوع مفیدش -> منظورش تنبلی واقعی نیست. منظور این است که قبل از اینکه فوراً شروع به کدنویسی کنی، کمی فکر کن.
Problem → Understand → Research → Consider alternatives → Design → Code

وقتی Task یا Bug می‌گیری، فوراً دنبال implementation نرو.

اول بفهم:

مشکل واقعی چیست؟ چرا وجود دارد؟ محدودیت‌ها چیست؟ کاربر واقعاً چه چیزی نیاز دارد؟ Root Cause چیست؟

Quick Fix ممکن است امروز مسئله را حل کند ولی فردا چند مشکل دیگر بسازد. نویسنده همچنین روی Shift Left تأکید می‌کند: review، testing، integration و security هرچه زودتر وارد فرایند شوند، پیدا کردن و اصلاح مشکلات ارزان‌تر می‌شود.


۹. از Five Whys استفاده کن

برای رسیدن به Root Cause، چند بار بپرس:

چرا؟

مثلاً:

Problem → چرا؟
↓
Cause → چرا؟
↓
Deeper Cause → چرا؟
↓
...
↓
Root Cause

هدف این است که symptom را fix نکنی و علت اصلی را پیدا کنی


خلاصه‌ی کل فصل در یک مدل ذهنی

وقتی یک Task جدید گرفتی، به جای اینکه مستقیم IDE را باز کنی:

1. Problem را بفهم.
دقیقاً قرار است چه چیزی حل شود؟

2. Why را پیدا کن.
چرا این feature لازم است؟

3. Context و Constraints را بفهم.

4. Root Cause را پیدا کن.

5. Codebase موجود را بخوان.

6. Research کن.
آیا قبلاً راه‌حل یا library مناسبی وجود دارد؟

7. چند Solution در نظر بگیر.

8. ساده‌ترین راه‌حل مناسب را انتخاب کن.

9. درباره‌ی Testing / Security / Reliability / Maintainability فکر کن.

10. کدی بنویس که نفر بعدی بتواند بفهمد.

...................................

## 2 Reading Code


مهندس نرم‌افزار بیشتر از اینکه کد بنویسد، کد می‌خواند.



وقتی وارد کد موجود می‌شوی، در واقع باید چهار مسئله را حل کنی:

Business Problem را بفهمی.

بفهمی Developer قبلی چطور به مسئله نگاه کرده است.
بررسی کنی آیا Abstraction و Model فعلی مناسب‌اند.
لایه‌های Technical Debt و تصمیم‌های تاریخی را بفهمی.

یعنی قبل از اینکه بگویی «این معماری افتضاحه»، بهتر است بفهمی این تصمیم چه زمانی و چرا گرفته شده.




وقتی وارد Codebase ناآشنا شدی، از کجا شروع کنی؟

اشتباه رایج این است:

Open project → Random class → Scroll → Confusion

روش پیشنهادی کتاب ساختارمندتر است.

مرحله ۱: از آدم‌ها شروع کن

اول از اعضای تیم overview بگیر.

بپرس:

سیستم چه کاری انجام می‌دهد؟
قسمت‌های اصلی کدام‌اند؟
معماری کلی چیست؟
مهم‌ترین flowها چیست؟
مرحله ۲: Documentation

بخوان:

README
Wiki
Architecture Documentation
ADR
Coding Standards

خصوصاً ADR یا Architecture Decision Record مهم است چون فقط نمی‌گوید چه تصمیمی گرفته شده، بلکه می‌تواند توضیح دهد چرا گرفته شده است.

اگر Documentation قدیمی یا ناقص است، هنگام یادگیری آن را اصلاح کن.

حداقل باید بتوانی پاسخ این سؤال‌ها را پیدا کنی:

این Service چه کاری انجام می‌دهد؟
چطور کار می‌کند؟
به چه چیزهایی وابسته است؟
چطور پروژه را اجرا کنیم؟


یکی از مفاهیم اصلی فصل Software Archaeology است.

وقتی Codebase جدیدی می‌بینی، مثل باستان‌شناس عمل کن.

اول تصویر بزرگ را پیدا کن:

Project Structure → Modules → Packages → Dependencies → Domain Concepts

ببین:

Monolith است یا Distributed؟
Domainها کجا هستند؟
Tests کجا هستند؟
Moduleها چطور با هم ارتباط دارند؟

بعد یک Landmark پیدا کن.

مثلاً در Android:

روی Login کلیک می‌کنم.

بعد مسیر را دنبال کن:

Login Screen
↓
ViewModel
↓
UseCase
↓
Repository
↓
API
↓
Response
↓
State
↓
UI

به جای تلاش برای فهم کل پروژه، یک Flow واقعی را از ابتدا تا انتها دنبال کن.

هدف نهایی ساختن یک ental Model از سیستم است.

روش مؤثر:

Run → Action → Breakpoint → Trace

مثلاً:

کاربر Search می‌زند.

Breakpoint بگذار و ببین:

UI → Controller/ViewModel → Business Logic → Repository → DB/API

اگر چیزی که انتظار داشتی اتفاق نیفتاد، دوباره مسیر را بررسی کن.

کم‌کم اتصال بین رفتار واقعی برنامه و کد در ذهنت شکل می‌گیرد.


برای Code Reading از این‌ها زیاد استفاده کن:

Find Usages
Find References
Go to Definition
Find Implementations
Call Hierarchy
Type Hierarchy
Dependency Diagram
Structure View

مثلاً اگر نمی‌دانی یک Interface چه نقشی دارد:

Interface → Find Implementations → Find Usages → Call Hierarchy

خیلی سریع‌تر از این است که ۸۰۰ فایل را مثل یک رمان روسی ورق بزنی.

هفته‌ای یک Shortcut یا Feature جدید.

حتی یک فایل شخصی به نام مثلاً:

IDE Tricks

بساز و Shortcutها و قابلیت‌های جدید را داخلش یادداشت کن و مرتب مرورشان کن تا تبدیل به muscle memory شوند.


Testها را قبل از Implementation بخوان

این یکی از مهم‌ترین تکنیک‌های فصل است.

Code می‌گوید سیستم چگونه کار می‌کند.
Test می‌گوید سیستم قرار است چگونه کار کند.

Test خوب مثل Living Documentation است.

اصل پیشنهادی کتاب:

You are the pilot, not the passenger. Trust but verify.

یعنی AI سرعتت را بالا ببرد، نه اینکه فهم سیستم را به آن outsource کنی.

این نکته با Agentic Coding حتی مهم‌تر می‌شود. چون اگر AI بتواند ۲۰ هزار خط کد بسازد، کسی هنوز باید بفهمد آن ۲۰ هزار خط دقیقاً چه بلایی سر سیستم آورده است.


چک‌لیست عملی Code Reading

وقتی فردا یک Task یا Codebase ناآشنا گرفتی، این ترتیب خیلی خوب است:

1. Business Problem را بفهم.

2. README / Docs / ADR را بخوان.

3. Project Structure را بررسی کن.

4. Domain و Module مرتبط را پیدا کن.

5. برنامه را Run کن.

6. یک User Flow مشخص انتخاب کن.

7. Testهای مربوط به آن Flow را بخوان.

8. از Find Usages / Go to Definition استفاده کن.

9. Call Hierarchy را دنبال کن.

10. Breakpoint بگذار و Flow را Trace کن.

11. Git History / Blame را بررسی کن.

12. Edge Caseها را از Testها پیدا کن.

13. چیزهایی را که فهمیدی یادداشت یا Document کن.

14. بعد تغییر را انجام بده.

این دقیقاً همان مهارتی است که نویسنده توصیه می‌کند با خواندن پروژه‌های Open Source تمرین شود. هیچ shortcut خاصی هم ندارد، باید مرتب کد دیگران را بخوانی.

خلاصه‌ی فصل در یک جمله

Junior کد را باز می‌کند تا جایی را که باید تغییر دهد پیدا کند؛ Engineer کد را می‌خواند تا بفهمد سیستم چرا این‌گونه رفتار می‌کند، بعد تصمیم می‌گیرد کجا و چگونه باید تغییرش دهد.

و اگر بخواهم دو فصل اول را به هم وصل کنم:

فصل ۱: قبل از Solution، Problem را بفهم.
فصل ۲: قبل از تغییر Code، System را بفهم.

این دو عادت به ظاهر ساده بخش بزرگی از فاصله‌ی «کسی که خوب Kotlin می‌نویسد» با «Software Engineer قوی» را تشکیل می‌دهند.




......................
## 3 Writing Code

کد خوب باید:

Readable + Concise + Organized + Maintainable

مهم‌تر از همه Intent خودش را واضح منتقل کند.

یکی از معیارها Cyclomatic Complexity است:

هرچه تعداد مسیرهای مختلف اجرای کد بیشتر شود، فهم و تست آن سخت‌تر می‌شود و احتمالاً Refactoring لازم است.

ابزارهای Static Analysis هم می‌توانند کمک کنند مشکلات را زود پیدا کنیم.


یکی از اصول اساسی فصل:

هرچه کد کمتری داشته باشی، کد کمتری هم باید بفهمی و نگهداری کنی.

هدف Software Design این است که سیستم بزرگ را به قسمت‌هایی تقسیم کنیم که مغز انسان بتواند آن‌ها را بفهمد.


High Cohesion

چیزهایی که به هم مربوط‌اند کنار هم باشند.

Low Coupling

قسمت‌های مختلف سیستم تا جای ممکن وابستگی کمی به یکدیگر داشته باشند.

Composition را به Inheritance ترجیح بده

اصل معروف:

Favor Composition over Inheritance

Inheritance گاهی درست است، اما نباید صرفاً برای Code Reuse استفاده شود.

Inheritance when relationship truly is-a
Composition when behavior/capability should be assembled

Reuse باید نتیجه‌ی Design خوب باشد، نه دلیل اصلی Inheritance.

اگر نمی‌توانی اسم مناسبی برای Method پیدا کنی، احتمال دارد مسئولیتش واضح نباشد.

Comment باید WHY را توضیح دهد، نه WHAT را.



در Review بهتر است تمرکز روی این‌ها باشد:

Naming
Readability
Duplication
Logging / Tracing / Metrics
Interfaces
Error-prone code
Domain Model
Abstractions

Formatting و Style تا جای ممکن باید توسط ابزارها automate شوند. وقت انسان گران‌تر از آن است که سر فاصله و newline بجنگد، هرچند صنعت نرم‌افزار سال‌هاست با شجاعت این واقعیت را نادیده گرفته.

تیم‌ها Bugها و مشکلات جالب هفته را با هم بررسی کنند تا دانشی که یک نفر به دست آورده بین بقیه پخش شود.


چک‌لیست عملی فصل برای هر Task

قبل از Coding:

1. آیا واقعاً باید Code جدید بنویسم؟
2. آیا چیزی مشابه در Codebase داریم؟
3. Language/Framework راه‌حل دارد؟
4. Library مناسب وجود دارد؟

هنگام Coding:

5. ساده‌ترین Solution چیست؟
6. Cohesion بالا و Coupling پایین است؟
7. آیا Class/Method زیادی بزرگ شده؟
8. آیا Composition بهتر از Inheritance است؟
9. Naming بدون خواندن body Intent را منتقل می‌کند؟
10. Accidental Complexity ایجاد کرده‌ام؟
11. Test دارم؟

قبل از PR:

12. Dead/Duplicate code را حذف کن.
13. Static Analysis را اجرا کن.
14. Commentهای WHAT را حذف یا با کد واضح جایگزین کن.
15. Commentهای ضروری WHY را نگه دار.
16. PR را کوچک و قابل Review نگه دار.

و برای تمرین، فصل پیشنهاد می‌کند Code Kata انجام دهی، یک مسئله را با چند روش یا چند زبان حل کنی، کد قدیمی خودت را دوباره Review کنی و گاهی Open Source بخوانی یا در آن مشارکت کنی. همچنین باید هر چند وقت یک بار تغییرات زبان و Framework اصلی خودت را مرور کنی.

سه فصل اول در سه جمله

فصل ۱، Programmer → Engineer:
قبل از اینکه Solution بدهی، Problem را بفهم.

فصل ۲، Reading Code:
قبل از اینکه Code را تغییر دهی، System را بفهم.

فصل ۳، Writing Code:
وقتی Code می‌نویسی، آن را برای Developer بعدی بنویس، نه صرفاً برای Compiler.

اگر این سه عادت جا بیفتند، تغییر اصلی دیگر «Kotlin بیشتری بلد بودن» نیست. داری کم‌کم از کسی که Task را implement می‌کند به کسی تبدیل می‌شوی که می‌تواند درباره‌ی کیفیت راه‌حل قضاوت کند. و خب، متأسفانه همین قسمت سخت‌تر و ارزشمندتر مهندسی نرم‌افزار است.


.......

## 4 Modeling


اگر سه فصل قبل می‌گفتند «مسئله را بفهم، سیستم را بخوان، بعد کد خوب بنویس»، این فصل یک ابزار مهم دیگر به جعبه‌ابزار مهندس نرم‌افزار اضافه می‌کند:

قبل از اینکه همه‌چیز را با کد توضیح بدهی، گاهی یک دیاگرام ساده می‌تواند مسئله و طراحی را بسیار واضح‌تر کند.

Software Model یک نمایش ساده‌شده و انتزاعی از سیستم است که کمک می‌کند:

Structure سیستم را بفهمیم،
Behavior آن را بررسی کنیم،
Interactions را ببینیم،
و مهم‌تر از همه، Design را با دیگران Communicate کنیم.

مثلاً به جای اینکه برای همکارت توضیح بدهی:

Screen به ViewModel وصل است، ViewModel UseCase را صدا می‌زند، UseCase Repository را...

می‌توانی بکشی:

UI
 ↓
ViewModel
 ↓
UseCase
 ↓
Repository
 ↓
API



برای کار واقعی، کدام Diagram را انتخاب کنم؟

این Mental Model را حفظ کن:

سؤال	Diagram
سیستم با چه چیزهایی ارتباط دارد؟	Context
اجزای اصلی سیستم چیست؟	Component
Classها چه رابطه‌ای دارند؟	Class
این Flow چطور اجرا می‌شود؟	Sequence
سیستم کجا Deploy شده؟	Deployment
Security Boundaryها کجاست؟	Security
Dataها چه رابطه‌ای دارند؟	Data Model

..........................
## 5 Automated Testing

Quality is not an act, it is a habit

یعنی کیفیت نرم‌افزار با یک بار Refactor یا نوشتن چند Test قبل از Release ساخته نمی‌شود. کیفیت نتیجه‌ی عادت دائمی به نوشتن، Review کردن و Test کردن درست کد است.

اما Test فقط برای پیدا کردن Bug نیست. فصل چهار فایده‌ی اصلی برایش مطرح می‌کند:

Documentation
Maintainability
Confidence
Consistency & Repeatability


بنابراین Test خوب می‌تواند به تو بگوید:

سیستم چه کاری می‌کند؟
چه Behaviorهایی انتظار می‌روند؟
Edge Caseها چیست؟
کدام قسمت Code مربوط است؟



یک Rule کاربردی:

اگر Unit Test نوشتن برای یک Class به شکل غیرعادی سخت است، Design آن Class را هم بررسی کن.

Behavior خودت را Test کن، نه اینکه ثابت کنی Kotlin و Android Framework هنوز سر کار آمده‌اند.


SUT = System Under Test

یعنی چیزی که در حال Test کردنش هستی.

در Unit Test تمرکز باید روی SUT باشد، نه Dependencies اطرافش.


