# How to add a jump list with xamarin.forms listview

You can add jump list using Xamarin.Forms ListView by tap on the indexed letter (group key) and scroll to the respective group [GroupHeaderItem](https://help.syncfusion.com/cr/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.GroupHeaderItem.html) by passing group key index in [ScrollToIndex](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.LayoutBase~ScrollToRowIndex.html) method like contact list. You can refer following article explains you how to create jump list with group key and scroll to tapped item.

https://www.syncfusion.com/kb/11021/how-to-add-a-jump-list-with-xamarin-forms-listview 

Creates panel to shows the group key as indexed vertically.
```
<ContentPage xmlns:syncfusion="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms" >
    <ContentPage.Content>
       <Grid >
          <syncfusion:SfListView x:Name="listView" Grid.Row="0">
                <syncfusion:SfListView.ItemTemplate>
                          <DataTemplate>
                                     <ViewCell>
                                             <Grid>
                                                  <Image Source="{Binding ContactImage}" Grid.Column="0"/>
                                                        <StackLayout Orientation="Vertical" Grid.Column="1">
                                                                     <Label Text="{Binding ContactName}"/>
                                                                     <Label Text="{Binding ContactNumber}"/>
                                                        </StackLayout>
                                               </Grid>
                                             <StackLayout HeightRequest="1" BackgroundColor="LightGray"/>
                                     </ViewCell>
                          </DataTemplate>
                </syncfusion:SfListView.ItemTemplate>                
            </syncfusion:SfListView>
            <Grid x:Name="IndexPanelGrid" Grid.Row="0"  VerticalOptions="CenterAndExpand" HorizontalOptions="End" />
         </Grid>
    </ContentPage.Content>
</ContentPage>
```
Creates and populate the index panel with index labeled from the Group key.

```
ListView.Loaded += ListView_Loaded;

private void ListView_Loaded(object sender, ListViewLoadedEventArgs e)
{
     var groupcount = this.ListView.DataSource.Groups.Count;
     for (int i = 0; i < groupcount; i++)
     {
        label = new Label();  
        GroupResult group = ListView.DataSource.Groups[i];
        label.Text = group.Key.ToString();
        indexPanelGrid.Children.Add(label, 0, i);
        var labelTapped = new TapGestureRecognizer() { Command = new Command<object>(OnTapped), CommandParameter = label };
        label.GestureRecognizers.Add(labelTapped);
     }
}
```
On tapping the group key value loaded in the index panel, you can scroll the listview to the respective group by comparing all GroupHeader’s Key value like below.

```
private void OnTapped(object obj)
{
    if (previousLabel != null)
    {
      previousLabel.TextColor = Color.DimGray;
    }
    var label = obj as Label;
    var text = label.Text;
    label.TextColor = Color.Red;
    for (int i = 0; i < ListView.DataSource.Groups.Count; i++)
    {
        var group = ListView.DataSource.Groups[i];

        if (group.Key == null)
          App.Current.MainPage.DisplayAlert("Message", "Group not available", "ok");
        if ((group.Key != null && group.Key.Equals(text)))
        {
                  ListView.LayoutManager.ScrollToRowIndex (ListView.DataSource.DisplayItems.IndexOf(group), true);
         }
     }
     previousLabel = label;
}
```
**OutPut**

![JumpList](https://github.com/SyncfusionExamples/how-to-add-a-jump-list-with-xamarin.forms-listview/blob/master/Screenshots/JumpList.png)
