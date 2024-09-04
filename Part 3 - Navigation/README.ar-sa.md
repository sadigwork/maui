## التنقل

في الجزء 3، سنضيف تنقلًا بسيطًا لدفع صفحة جديدة إلى المكدس لعرض تفاصيل حول القرد.

هذه الوحدة متاحة أيضًا باللغتين [الصينية (المبسطة)](README.zh-cn.md) و[الصينية (التقليدية)](README.zh-tw.md).

سنستخدم التنقل المدمج في Shell في .NET MAUI. يعتمد نظام التنقل القوي هذا على URIs. يمكنك تمرير معلومات إضافية أثناء التنقل عبر معلمة الاستعلام مثل سلسلة أو كائن كامل.

على سبيل المثال، لنفترض أننا أردنا الانتقال إلى صفحة تفاصيل وتمرير معرف.

```csharp
await Shell.Current.GoToAsync("DetailsPage?name=james");
```

ثم في صفحة التفاصيل أو نموذج العرض الخاص بنا، يجب أن نحدد هذه الخاصية:

```csharp
[QueryProperty(nameof(Name), "name")]
public partial class DetailsPage : ContentPage
{
string name;
public string Name
{
get => name;
set => name = value;
}
}
```

عند التنقل، سيتم تمرير الاسم "james" تلقائيًا. يمكننا أيضًا تمرير كائن كامل باستخدام نفس الآلية:

```csharp
var person = new Person { Name="James" };
await Shell.Current.GoToAsync("DetailsPage", new Dictionary<string, object>
{
{ "person", person }
});
```

ثم في نموذج الصفحة أو العرض الخاص بنا، سنقوم بإنشاء الخاصية.

```csharp
[QueryProperty(nameof(Person), "person")]
public partial class DetailsPage : ContentPage
{
Person person;
public Person Person
{
get => person;
set => person = value;
}
}
```

هنا، يتم تسلسل وإلغاء تسلسل `Person` تلقائيًا بالنسبة لنا عندما ننتقل.

الآن، دعنا نضيف معالج النقر إلى عرض المجموعة ونمرر القرد إلى صفحة التفاصيل.

### إضافة حدث محدد

الآن، دعنا نضيف التنقل إلى صفحة ثانية تعرض تفاصيل القرد!

1. في `MonkeysViewModel.cs`، قم بإنشاء طريقة `async Task GoToDetailsAsync(Monkey monkey)` مكشوفة كـ `[RelayCommand]`:

```csharp
[RelayCommand]
async Task GoToDetails(Monkey monkey)
{
if (monkey == null)
return;

await Shell.Current.GoToAsync(nameof(DetailsPage), true, new Dictionary<string, object>
{
{"Monkey", monkey }
});
}
```

- يتحقق هذا الكود لمعرفة ما إذا كان العنصر المحدد غير فارغ ثم يستخدم واجهة برمجة التطبيقات المضمنة في Shell `Navigation` لدفع صفحة جديدة مع القرد كمعلمة ثم إلغاء تحديد العنصر.

1. في `MainPage.xaml` يمكننا إضافة حدث `TapGestureRecognizer` إلى `Frame` الخاص بقردنا داخل `CollectionView.ItemTemplate`:

Before:

```xml
<CollectionView.ItemTemplate>
<DataTemplate x:DataType="model:Monkey">
<Grid Padding="10">
<Frame HeightRequest="125" Style="{StaticResource CardView}">
<Grid Padding="0" ColumnDefinitions="125,*">
<Image
Aspect="AspectFill"
HeightRequest="125"
Source="{Binding Image}"
WidthRequest="125" />
<VerticalStackLayout
Grid.Column="1"
VerticalOptions="Center"
Padding="10">
<Label Style="{StaticResource LargeLabel}" Text="{Binding Name}" />
<Label Style="{StaticResource MediumLabel}" Text="{Binding Location}" />
</VerticalStackLayout>
</Grid>
</Frame>
</Grid>
</DataTemplate>
</CollectionView.ItemTemplate>
```

بعد:
```xml
<CollectionView.ItemTemplate>
<DataTemplate x:DataType="model:Monkey">
<Grid Padding="10">
<Frame HeightRequest="125" Style="{StaticResource CardView}">
<!-- إضافة أداة التعرف على الإيماءات-->
<Frame.GestureRecognizers>
<TapGestureRecognizer
Command="{Binding Source={RelativeSource AncestorType={x:Type viewmodel:MonkeysViewModel}}, Path=GoToDetailsCommand}"
CommandParameter="{Binding .}"/>
</Frame.GestureRecognizers>
<Grid Padding="0" ColumnDefinitions="125,*">
<Image
Aspect="AspectFill"
HeightRequest="125"
Source="{Binding Image}"
WidthRequest="125" />
<VerticalStackLayout
Grid.Column="1"
VerticalOptions="Center"
Padding="10">
<Label Style="{StaticResource LargeLabel}" Text="{Binding Name}" />
<Label Style="{StaticResource MediumLabel}" Text="{Binding Location}" />
</VerticalStackLayout>
</Grid>
</Frame>