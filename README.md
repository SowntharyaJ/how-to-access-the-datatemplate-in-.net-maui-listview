# how-to-access-the-datatemplate-in-.net-maui-listview

This repository contains a sample demonstrating how to access the datatemplate in .NET MAUI ListView (SfListView).

## Sample

```xaml
<popup:SfPopup
            x:Name="popup"
            AcceptCommand="{Binding PopupAcceptCommand}"
            HeightRequest="350"
            ShowFooter="True"
            ShowHeader="False"
            WidthRequest="350">
    <popup:SfPopup.ContentTemplate>
        <DataTemplate>
            <Grid>
                <Grid.Behaviors>
                    <local:GridBehavior />
                </Grid.Behaviors>
                <Grid.RowDefinitions>
                    <RowDefinition Height="50" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Button
                    x:Name="listviewButton"
                    Grid.Row="0"
                    Text="Find ListView" />
                <sfListView:SfListView
                    x:Name="listView"
                    Grid.Row="1"
                    HeightRequest="110"
                    ItemSpacing="5"
                    ItemsSource="{Binding Items}"
                    SelectionMode="Single">
                    <sfListView:SfListView.ItemTemplate>
                        <DataTemplate>
                            <Grid Padding="0,10,0,0">
                                <Grid.RowDefinitions>
                                    <RowDefinition Height="50" />
                                    <RowDefinition Height="50" />
                                </Grid.RowDefinitions>
                                <Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="*" />
                                    <ColumnDefinition Width="*" />
                                </Grid.ColumnDefinitions>
                                <Label
                                    Grid.Row="0"
                                    Grid.Column="0"
                                    FontAttributes="Bold"
                                    Text="ContactName" />
                                <Entry
                                    Grid.Row="0"
                                    Grid.Column="1"
                                    Text="{Binding ContactName, Mode=TwoWay}" />
                                <Label
                                    Grid.Row="1"
                                    Grid.Column="0"
                                    FontAttributes="Bold"
                                    Text="ContactNumber" />
                                <Entry
                                    Grid.Row="1"
                                    Grid.Column="1"
                                    Text="{Binding ContactNumber, Mode=TwoWay}" />
                            </Grid>
                        </DataTemplate>
                    </sfListView:SfListView.ItemTemplate>
                </sfListView:SfListView>
            </Grid>
        </DataTemplate>
    </popup:SfPopup.ContentTemplate>
</popup:SfPopup>
```

```c#
public class GridBehavior : Behavior<Grid>
{
    Grid grid;
    SfListView listView;
    Button button;
    protected override void OnAttachedTo(BindableObject bindable)
    {
        grid = bindable as Grid;
        grid.ChildAdded += Grid_ChildAdded;
    }
    
    //Method 1 : Get SfListView reference using Grid.ChildAdded Event
    private void Grid_ChildAdded(object sender, ElementEventArgs e)
    {
        if (e.Element is SfListView)
        {
            listView = e.Element as SfListView;
            
        }
        if (e.Element is Button)
        {
            button = e.Element as Button;
            button.Clicked += Button_Clicked;
        }
    }

    //Method 2 : Get SfListView reference using FindByName
    private void Button_Clicked(object sender, EventArgs e)
    {
        listView = grid.FindByName<SfListView>("listView");
        var container = listView.GetVisualContainer();
        App.Current.MainPage.DisplayAlert("Information", "ListView instance obtained", "Ok");
        listView.ItemTapped += ListView_ItemTapped;
    }

    private void ListView_ItemTapped(object sender, Syncfusion.Maui.ListView.ItemTappedEventArgs e)
    {
        App.Current.MainPage.DisplayAlert("Information", "ListView ItemTapped", "Ok");
    }

    protected override void OnDetachingFrom(BindableObject bindable)
    {
        button.Clicked -= Button_Clicked;
        grid.ChildAdded -= Grid_ChildAdded;
        listView.ItemTapped -= ListView_ItemTapped;
        listView = null;
        button = null;
        grid = null;
        base.OnDetachingFrom(bindable);
    }
}

```

## Requirements to run the demo

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) or [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/)
* Xamarin add-ons for Visual Studio (available via the Visual Studio installer).

## Troubleshooting

### Path too long exception

If you are facing path too long exception when building this example project, close Visual Studio and rename the repository to short and build the project.
