## الوصول إلى ميزات المنصة

في الجزء الرابع، سنستخدم .NET MAUI للعثور على أقرب قرد إلينا وسنفتح أيضًا خريطة بها موقع القرود.

تتوفر هذه الوحدة أيضًا باللغتين [الصينية (المبسطة)](README.zh-cn.md) و[الصينية (التقليدية)](README.zh-tw.md).

### التحقق من الإنترنت

يمكننا بسهولة التحقق لمعرفة ما إذا كان المستخدم متصلاً بالإنترنت باستخدام `IConnectivity` المضمن في .NET MAUI

1. أولاً، دعنا نحصل على إمكانية الوصول إلى `IConnectivity` الموجودة داخل .NET MAUI. دعنا نحقن `IConnectivity` في منشئ `MonkeysViewModel` الخاص بنا:

    ```csharp
    IConnectivity connectivity;
    public MonkeysViewModel(MonkeyService monkeyService, IConnectivity connectivity)
    {
        Title = "Monkey Finder";
        this.monkeyService = monkeyService;
        this.connectivity = connectivity;
    }
    ```

1. قم بتسجيل `Connectivity.Current` في `MauiProgram.cs` الخاص بنا.


1. بينما نحن هنا، دعنا نضيف كلًا من `IGeolocation` و`IMap`، ونضيف الكود:

    ```csharp
    builder.Services.AddSingleton<IConnectivity>(Connectivity.Current);
    builder.Services.AddSingleton<IGeolocation>(Geolocation.Default);
    builder.Services.AddSingleton<IMap>(Map.Default);
    ```

1. الآن، دعنا نتحقق من وجود الإنترنت داخل طريقة `GetMonkeysAsync` ونعرض تنبيهًا في حالة عدم الاتصال بالإنترنت.

    ```csharp
    if (connectivity.NetworkAccess != NetworkAccess.Internet)
    {
        await Shell.Current.DisplayAlert("No connectivity!",
            $"Please check internet and try again.", "OK");
        return;
    }
    ```

  قم بتشغيل التطبيق على المحاكي الخاص بك وقم بتشغيل وإيقاف وضع الطيران للتحقق من تنفيذك.

### البحث عن أقرب قرد!

يمكننا إضافة المزيد من الوظائف إلى هذه الصفحة باستخدام نظام تحديد المواقع العالمي (GPS) الخاص بالجهاز نظرًا لأن كل قرد له خط عرض وخط طول مرتبط به.

1. أولاً، دعنا نحصل على إمكانية الوصول إلى `IGeolocator` الموجود داخل .NET MAUI. دعنا نحقن `IGeolocator` في منشئ `MonkeysViewModel` الخاص بنا:

    ```csharp
    IConnectivity connectivity;
    IGeolocation geolocation;
    public MonkeysViewModel(MonkeyService monkeyService, IConnectivity connectivity, IGeolocation geolocation)
    {
        Title = "Monkey Finder";
        this.monkeyService = monkeyService;
        this.connectivity = connectivity;
        this.geolocation = geolocation;
    }
    ```


1. في ملف `MonkeysViewModel.cs` الخاص بنا، دعنا ننشئ طريقة أخرى تسمى `GetClosestMonkey`:

    ```csharp
    [RelayCommand]
    async Task GetClosestMonkey()
    {

    }
    ```

1. يمكننا بعد ذلك ملؤه باستخدام .NET MAUI للاستعلام عن موقعنا والمساعدين الذين يجدون أقرب قرد إلينا:

    ```csharp
    [RelayCommand]
    async Task GetClosestMonkey()
    {
        if (IsBusy || Monkeys.Count == 0)
            return;

        try
        {
            // Get cached location, else get real location.
            var location = await geolocation.GetLastKnownLocationAsync();
            if (location == null)
            {
                location = await geolocation.GetLocationAsync(new GeolocationRequest
                {
                    DesiredAccuracy = GeolocationAccuracy.Medium,
                    Timeout = TimeSpan.FromSeconds(30)
                });
            }

            // Find closest monkey to us
            var first = Monkeys.OrderBy(m => location.CalculateDistance(
                new Location(m.Latitude, m.Longitude), DistanceUnits.Miles))
                .FirstOrDefault();

            await Shell.Current.DisplayAlert("", first.Name + " " +
                first.Location, "OK");

        }
        catch (Exception ex)
        {
            Debug.WriteLine($"Unable to query location: {ex.Message}");
            await Shell.Current.DisplayAlert("Error!", ex.Message, "OK");
        }
    }
    ```


1. في ملف `MainPage.xaml` الخاص بنا، يمكننا إضافة `Button` آخر لاستدعاء هذه الطريقة الجديدة:

أضف XAML التالي أسفل زر البحث.

    ```xml
    <Button Text="Find Closest" 
        Command="{Binding GetClosestMonkeyCommand}"
        IsEnabled="{Binding IsNotBusy}"
        Grid.Row="1"
        Grid.Column="1"
        Style="{StaticResource ButtonOutline}"
        Margin="8"/>
    ```

أعد تشغيل التطبيق لمشاهدة الموقع الجغرافي أثناء العمل بعد تحميل القرود!

تم تكوين هذا المشروع مسبقًا بجميع الأذونات والميزات المطلوبة لتحديد الموقع الجغرافي. يمكنك قراءة الوثائق لمعرفة المزيد حول الإعداد، ولكن إليك نظرة عامة سريعة.

1. تم تكوين .NET MAUI مسبقًا في جميع تطبيقات .NET MAUI بما في ذلك التعامل مع الأذونات.
1. تم تكوين معلومات بيان Android مسبقًا في **MonkeyFinder -> Platforms -> Android -> AssemblyInfo.cs**
1. تم تكوين معلومات بيان iOS/macOS في ملف **info.plist** لكل منصة
1. تم تكوين معلومات بيان Windows في **Package.appxmanifest**

### فتح الخرائط

يوفر .NET MAUI أكثر من 60 ميزة للمنصة من واجهة برمجة تطبيقات واحدة ويتم تضمين فتح تطبيق الخريطة الافتراضي!

1. قم بحقن `IMap` في `MonkeyDetailsViewModel` الخاص بنا:

    ```csharp
    IMap map;
    public MonkeyDetailsViewModel(IMap map)
    {
        this.map = map;
    }
    ```

1. افتح الملف `MonkeyDetailsViewModel.cs` وأضف طريقة تسمى `OpenMap` والتي تستدعي واجهة برمجة التطبيقات `Map` وتمرر إليها موقع القرد:

    ```csharp
    [RelayCommand]
    async Task OpenMap()
    {
        try
        {
            await map.OpenAsync(Monkey.Latitude, Monkey.Longitude, new MapLaunchOptions
            {
                Name = Monkey.Name,
                NavigationMode = NavigationMode.None
            });
        }
        catch (Exception ex)
        {
            Debug.WriteLine($"Unable to launch maps: {ex.Message}");
            await Shell.Current.DisplayAlert("Error, no Maps app!", ex.Message, "OK");
        }
    }

    ```

### تحديث تفاصيل صفحة واجهة المستخدم xaml

فوق اسم القرد، دعنا نضيف زرًا يستدعي الأمر `OpenMapCommand`.

```xml
<Button Text="Show on Map" 
        Command="{Binding OpenMapCommand}"
        HorizontalOptions="Center" 
        WidthRequest="200" 
        Margin="8"
        Style="{StaticResource ButtonOutline}"/>
                
<Label Style="{StaticResource MediumLabel}" Text="{Binding Monkey.Details}" />
```

قم بتشغيل التطبيق، وانتقل إلى قرد، ثم اضغط على "إظهار على الخريطة" لتشغيل تطبيق الخريطة على المنصة المحددة.

## تخطيطات منطقة الأمان في iOS

بالإضافة إلى الوصول إلى واجهات برمجة تطبيقات الأجهزة متعددة المنصات، يتضمن .NET MAUI أيضًا تكاملات خاصة بالمنصة. إذا كنت تقوم بتشغيل تطبيق Monkey Finder على جهاز iOS مزود بشق، فقد تكون لاحظت أن الأزرار الموجودة في الأسفل تتداخل مع الشريط الموجود في أسفل الجهاز. يحتوي نظام iOS على مفهوم "المناطق الآمنة" ويجب عليك ضبط ذلك بشكل برمجي. ومع ذلك، بفضل تفاصيل المنصة، يمكنك ضبطها مباشرة في XAML.

1. افتح `MainPage.xaml` وأضف مساحة اسم جديدة لتفاصيل iOS:

    ```xml
    xmlns:ios="clr-namespace:Microsoft.Maui.Controls.PlatformConfiguration.iOSSpecific;assembly=Microsoft.Maui.Controls"
    ```

1. في العقدة `ContentPage`، يمكنك الآن تعيين الخاصية التالية:

```xml
ios:Page.UseSafeArea="True"
```

أعد تشغيل التطبيق على محاكي iOS أو جهاز ولاحظ أن الأزرار تم تحريكها تلقائيًا لأعلى.

لننتقل إلى الوحدة التالية ونتعرف على عرض المجموعة في [الجزء 5](../Part%205%20-%20CollectionView/README.ar-sa.md)
