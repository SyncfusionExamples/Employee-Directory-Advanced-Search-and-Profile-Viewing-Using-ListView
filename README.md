# Employee-Directory-Advanced-Search-and-Profile-Viewing-Using-ListView

Employee Directory-Advanced Search and Profile Viewing Using .NET MAUI ListView (SfListView).

## Sample

```xaml
<popup:SfPopup
            Grid.Row="2"
            AutoSizeMode="Width"
            HeightRequest="60"
            IsOpen="{Binding IsOpenForSearch}"
            RelativeView="{x:Reference Search}">
    <popup:SfPopup.ContentTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </popup:SfPopup.ContentTemplate>
</popup:SfPopup>

<!--  Employee List  -->
<listView:SfListView
    x:Name="listView"
    Grid.Row="1"
    ItemSize="120"
    ItemSpacing="8,8,8,8"
    ItemsSource="{Binding EmployeeDirectories}"
    SelectionMode="Single"
    TapCommand="{Binding ViewProfileCommand}">

    <listView:SfListView.ItemTemplate>
        <DataTemplate>
            <Grid
                Padding="16"
                BackgroundColor="White"
                RowDefinitions="Auto,Auto,Auto,Auto">

                <!--  Employee Header  -->
                <Grid Grid.Row="0" ColumnDefinitions="100,*,Auto">

                    <!--  Profile Image  -->
                    <Border
                        Grid.Column="0"
                        HeightRequest="100"
                        Stroke="Gray"
                        StrokeShape="RoundRectangle 50"
                        StrokeThickness="1"
                        WidthRequest="100">
                        <Image Aspect="AspectFill" Source="{Binding ImageUrl}" />
                    </Border>

                    <!--  Name and Position  -->
                    <StackLayout
                        Grid.Column="1"
                        Margin="12,0,0,0"
                        VerticalOptions="Center">
                        <Label
                            FontAttributes="Bold"
                            FontSize="18"
                            Text="{Binding FullName}"
                            TextColor="Black" />
                        <Label
                            FontSize="14"
                            Text="{Binding Position}"
                            TextColor="Gray" />

                        <HorizontalStackLayout Spacing="5">
                            <Label
                                FontSize="14"
                                Text="Reports to "
                                TextColor="Gray" />
                            <Label
                                FontSize="14"
                                Text="{Binding Manager}"
                                TextColor="Gray" />
                        </HorizontalStackLayout>

                        <Label
                            FontSize="14"
                            Text="{Binding Location}"
                            TextColor="Gray" />

                        <Label
                            FontSize="12"
                            Text="{Binding Department}"
                            TextColor="{StaticResource Primary}" />
                    </StackLayout>

                    <!--  Contact Information  -->
                    <Grid
                        Grid.Column="2"
                        Margin="0,8,0,0"
                        RowDefinitions="*,*,*,*">
                        <Label
                            Grid.Row="1"
                            FontSize="12"
                            Text="{Binding Email}"
                            TextColor="Gray" />
                        <Label
                            Grid.Row="2"
                            FontSize="12"
                            HorizontalTextAlignment="End"
                            Text="{Binding PhoneNumber}"
                            TextColor="Gray" />
                    </Grid>
                </Grid>

                <!--  Skills  -->
                <Label
                    Grid.Row="2"
                    Margin="0,4,0,0"
                    FontSize="11"
                    LineBreakMode="TailTruncation"
                    MaxLines="2"
                    Text="{Binding SkillsText}"
                    TextColor="{StaticResource Secondary}" />

                <!--  Hire Date  -->
                <Label
                    Grid.Row="3"
                    Margin="0,4,0,0"
                    FontSize="10"
                    Text="{Binding HireDate, StringFormat='Hired: {0:MMM yyyy}'}"
                    TextColor="Gray" />
            </Grid>
        </DataTemplate>
    </listView:SfListView.ItemTemplate>
</listView:SfListView>
```

```c#
public class SfListViewFilteringBehavior : Behavior<ContentPage>
{
    private Syncfusion.Maui.ListView.SfListView ListView;
    private EmployeeDirectoryViewModel viewModel;

    protected override void OnAttachedTo(ContentPage bindable)
    {
        ListView = ListView = bindable.FindByName<Syncfusion.Maui.ListView.SfListView>("listView");   
        viewModel = bindable.BindingContext as EmployeeDirectoryViewModel;
        viewModel.PropertyChanged += OnPropertyChanged;
        base.OnAttachedTo(bindable);
    }

    private void OnPropertyChanged(object sender, PropertyChangedEventArgs e)
    {
        if (ListView.DataSource != null)
        {
            switch (e.PropertyName)
            {
                case nameof(EmployeeDirectoryViewModel.SearchText):
                case nameof(EmployeeDirectoryViewModel.SelectedDepartment):
                case nameof(EmployeeDirectoryViewModel.SelectedSkill):
                    ListView.DataSource.Filter = viewModel.OnPerformSearch;
                    break;
            }
            ListView.DataSource.RefreshFilter();
            ListView.RefreshView();
        }
    }
}

```

## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.

