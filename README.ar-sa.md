# .NET MAUI - ورشة عمل

سنقوم اليوم ببناء تطبيق [.NET MAUI](https://docs.microsoft.com/dotnet/maui?WT.mc_id=friends-mauiworkshop-jamont) لعرض قائمة بالقرود من جميع أنحاء العالم. سنبدأ ببناء واجهة عمل منطقية تسحب البيانات المشفرة بتنسيق json من نقطة نهاية RESTful. ثم سنستفيد من [.NET MAUI](https://docs.microsoft.com/xamarin/essentials/index?WT.mc_id=friends-mauiworkshop-jamont) للعثور على أقرب قرد إلينا وإظهاره أيضًا على الخريطة. سنرى أيضًا كيفية عرض البيانات بطرق مختلفة عديدة ثم أخيرًا تخصيص سمات للتطبيق بالكامل.

## اللغات
هذه الورشة متاحة باللغات التالية:
* الإنجليزية - ملفات README الافتراضية
* [الصينية (المبسطة)](README.zh-cn.md) - ملفات README التي تنتهي بـ .zh-cn.md (مترجم بواسطة [Kinfey Lo](https://github.com/kinfey))
* [الصينية (التقليدية)](README.zh-tw.md) - ملف README الذي ينتهي بـ .zh-tw.md (مترجم بواسطة [James Tsai](https://github.com/JamestsaiTW))

## دليل الإعداد
مرحبًا! ستكون هذه الورشة ورشة عمل عملية وورشة عمل يمكنك فيها إحضار جهازك الخاص. يمكنك التطوير على جهاز كمبيوتر شخصي أو جهاز Mac وكل ما عليك فعله هو تثبيت Visual Studio 2022 أو Visual Studio for Mac 2022 مع عبء عمل .NET MAUI. تم إنشاؤه على .NET 8، مما يعني أنك ستحتاج إلى الإصدار 17.9 من Visual Studio 2022 أو أحدث. راجع [دليل التثبيت الكامل لـ .NET MAUI](https://learn.microsoft.com/dotnet/maui/get-started/installation?view=net-maui-8.0) لمزيد من المعلومات.

قبل بدء الورشة، أوصيك بالاطلاع على [دليل .NET MAUI](https://docs.microsoft.com/dotnet/maui/get-started/first-app?WT.mc_id=friends-mauiworkshop-jamont) السريع الذي سيرشدك خلال التثبيت ويضمن أيضًا تكوين كل شيء بشكل صحيح.

إذا كنت جديدًا في تطوير الأجهزة المحمولة، فنوصيك بالنشر على جهاز Android فعلي يمكن إعداده في بضع خطوات فقط. إذا لم يكن لديك جهاز، فلا تقلق حيث يمكنك إعداد [محاكي Android مع تسريع الأجهزة](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator?WT.mc_id=friends-mauiworkshop-jamont). إذا لم يكن لديك الوقت لإعداد هذا مسبقًا، فلا تقلق لأننا هنا للمساعدة أثناء ورشة العمل.

بعد ذلك، ستكون على استعداد للذهاب إلى ورشة العمل!

## جدول الأعمال
لقد قمت أيضًا بتجميع ملخص لما يمكنك توقعه من ورشة العمل التي ستستمر طوال اليوم:

* [الجزء 0](الجزء%200%20-%20نظرة عامة/README.md) - جلسة مدتها 30 دقيقة - مقدمة إلى جلسة .NET MAUI ومساعدة الإعداد
* [الجزء 1](الجزء%201%20-%20عرض%20البيانات/README.md) - قائمة صفحة واحدة للبيانات
* [الجزء 2](الجزء%202%20-%20MVVM/README.md) - MVVM وربط البيانات
* [الجزء 3](الجزء%203%20-%20التنقل/README.md) - التنقل
* [الجزء 4](الجزء%204%20-%20ميزات المنصة/README.md) - تنفيذ ميزات المنصة
* [الجزء 5](Part%205%20-%20CollectionView/README.md) - CollectionView وما بعده
* [الجزء 6](Part%206%20-%20AppThemes/README.md) - تخصيص موضوع للتطبيق

للبدء، افتح المجلد `الجزء 1 - عرض البيانات` وافتح `MonkeyFinder.sln`. يمكنك استخدام هذا طوال ورشة العمل. يحتوي كل **جزء** على ملف **README** يحتوي على تعليمات لهذا الجزء. إذا أتيت متأخرًا، يمكنك فتح أي من المجلدات وهناك مشروع بداية لهذا القسم.

## شرح فيديو
سجل جيمس [شرحًا كاملاً لمدة 4 ساعات](https://www.youtube.com/watch?v=DuNLR_NJv8U) من البداية إلى النهاية على [YouTube](https://youtube.com/jamesmontemagno)!

## المزيد من الروابط والموارد:
- [موقع ويب .NET MAUI](https://dot.net/maui)
- [.NET MAUI على Microsoft Learn](https://docs.microsoft.com/learn/paths/build-apps-with-dotnet-maui/)
- [وثائق .NET MAUI](https://docs.microsoft.com/dotnet/maui)
- [.NET MAUI على GitHub](https://github.com/dotnet/maui)
- [سلسلة مقاطع فيديو للمبتدئين في .NET](https://dot.net/videos)

إذا كانت لديك أي أسئلة، فيرجى التواصل معي على تويتر [@JamesMontemagno](https://twitter.com/jamesmontemagno).