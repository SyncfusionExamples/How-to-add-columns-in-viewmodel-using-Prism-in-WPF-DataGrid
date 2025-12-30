# How to add columns in viewmodel using Prism in WPF DataGrid?
In [WPF DataGrid](https://www.syncfusion.com/wpf-controls/datagrid) (SfDataGrid), [columns ](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Grid.Columns.html) are defined within the ViewModel and bound to the grid using Prism, ensuring a clean MVVM architecture. The ViewModel creates a [columns ](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Grid.Columns.html) collection and dynamically adds [GridTextColumn](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Grid.GridTextColumn.html) and [GridTemplateColumn](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Grid.GridTemplateColumn.html). To achieve this behavior using Prism, you need to install the Prism.Core package.

A DataTemplate for the template column is created using XamlReader.Parse, which contains a button bound to a DelegateCommand that copies the OrderID to the clipboard. The grid binds its ItemsSource to an ObservableCollection and its [columns](https://help.syncfusion.com/cr/wpf/Syncfusion.UI.Xaml.Grid.Columns.html) property to the ViewModel’s SfGridColumns, ensuring no code-behind logic.

**XAML**
 ```xml
<syncfusion:SfDataGrid 
        x:Name="sfGrid"
        AutoGenerateColumns="False"
        Columns="{Binding SfGridColumns, Mode=TwoWay}"
        ItemsSource="{Binding Orders}">
 </syncfusion:SfDataGrid> 
 ```
 
**C#**
 ```csharp
public Columns SfGridColumns
{
     get { return sfGridColumns; }
     set { SetProperty(ref sfGridColumns, value); }
}

private void SetSfGridColumns()
{
    string cellTemplateXaml =
        @"<DataTemplate xmlns='http://schemas.microsoft.com/winfx/2006/xaml/presentation'
                    xmlns:x='http://schemas.microsoft.com/winfx/2006/xaml'
                    xmlns:syncfusion='http://schemas.syncfusion.com/wpf'>
        <StackPanel Orientation='Horizontal'>
            <TextBlock Text='{Binding OrderID}' Margin='0,0,10,0'/>
            <Button Content='Copy ID'
                    Command='{Binding DataContext.CopyCommand,ElementName=sfGrid}'
                    CommandParameter='{Binding}'/>
        </StackPanel>
    </DataTemplate>";

    var template = (DataTemplate)XamlReader.Parse(cellTemplateXaml);

    var cols = new Columns();

    cols.Add(new GridTemplateColumn
    {
        MappingName = "OrderID",
        HeaderText = "Order ID",
        CellTemplate = template,
        Width = 100
    });

    cols.Add(new GridTextColumn
    {
        MappingName = "CustomerID",
        HeaderText = "Customer ID"
    });

    cols.Add(new GridTextColumn
    {
        MappingName = "CustomerName",
        HeaderText = "Customer Name"
    });

    cols.Add(new GridTextColumn
    {
        MappingName = "Country",
        HeaderText = "Country"
    });

    cols.Add(new GridTextColumn
    {
        MappingName = "ShipCity",
        HeaderText = "Ship City"
    });

    sfGridColumns = cols;
} 
 ```
 
![Add Columns In ViewModel Using Prism](Add_Columns_In_ViewModel_Using_Prism.png)

Take a moment to peruse the [WPF DataGrid - Columns](https://help.syncfusion.com/wpf/datagrid/columns) documentation, to learn more about columns and its types with examples.