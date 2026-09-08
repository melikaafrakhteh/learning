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




