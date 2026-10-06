# How-to-add-columns-in-viewmodel-using-Prism-in-WPF Data Grid

This sample demonstrates how to add and bind columns for the [WPF DataGrid](https://www.syncfusion.com/wpf-controls/datagrid) (SfDataGrid) from a ViewModel by using Prism.

In this example, the grid columns are created in the ViewModel and assigned to the `SfDataGrid.Columns` property. A custom `DataTemplate` is also used to show a button in a template column that calls a command to copy the `OrderID` value to the clipboard.

## Features

- Define `Data Grid` columns in the ViewModel
- Bind the columns collection to the grid using Prism `BindableBase`
- Create custom template columns for interactive UI inside the grid
- Populate the Data Grid using an `ObservableCollection`

## Project structure

- `SfDataGridDemo` - WPF application
- `SfDataGridDemo/MainWindow.xaml` - hosts the `Data Grid` and binds it to the ViewModel
- `SfDataGridDemo/ViewModel/ViewModel.cs` - contains column creation logic and sample data generation
- `SfDataGridDemo/Model/OrderInfo.cs` - model used for data binding

## ViewModel implementation

```csharp
public class ViewModel : BindableBase
{
    private ObservableCollection<OrderInfo> _orders;
    private Columns sfGridColumns;

    public DelegateCommand<OrderInfo> CopyCommand { get; }

    public ObservableCollection<OrderInfo> Orders
    {
        get { return _orders; }
        set { SetProperty(ref _orders, value); }
    }

    public Columns SfGridColumns
    {
        get { return sfGridColumns; }
        set { SetProperty(ref sfGridColumns, value); }
    }

    public ViewModel()
    {
        CopyCommand = new DelegateCommand<OrderInfo>(CopyAccountNo);

        SetSfGridColumns();

        _orders = new ObservableCollection<OrderInfo>();
        GenerateOrders();
    }

    private void GenerateOrders()
    {
        _orders.Add(new OrderInfo(1001, "Maria Anders", "Germany", "ALFKI", "Berlin"));
        _orders.Add(new OrderInfo(1002, "Ana Trujilo", "Mexico", "ANATR", "Mexico D.F."));
        _orders.Add(new OrderInfo(1003, "Antonio Moreno", "Mexico", "ANTON", "Mexico D.F."));
        _orders.Add(new OrderInfo(1004, "Thomas Hardy", "UK", "AROUT", "London"));
        _orders.Add(new OrderInfo(1005, "Christina Berglund", "Sweden", "BERGS", "Lula"));
        _orders.Add(new OrderInfo(1006, "Hanna Moos", "Germany", "BLAUS", "Mannheim"));
        _orders.Add(new OrderInfo(1007, "Frederique Citeaux", "France", "BLONP", "Strasbourg"));
        _orders.Add(new OrderInfo(1008, "Martin Sommer", "Spain", "BOLID", "Madrid"));
        _orders.Add(new OrderInfo(1009, "Laurence Lebihan", "France", "BONAP", "Marseille"));
        _orders.Add(new OrderInfo(1010, "Elizabeth Lincoln", "Canada", "BOTTM", "Tsawassen"));
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

    private void CopyAccountNo(OrderInfo orderID)
    {
        if (orderID == null || orderID.OrderID == null)
            return;

        var text = orderID.OrderID.ToString();
        Clipboard.SetText(text);
    }
}
```

## XAML binding

```xml
<Window.DataContext>
    <local:ViewModel/>
</Window.DataContext>

<Grid>
    <syncfusion:SfDataGrid x:Name="sfGrid"
            AutoGenerateColumns="False"
            Columns="{Binding SfGridColumns, Mode=TwoWay}"
            ItemsSource="{Binding Orders}">
    </syncfusion:SfDataGrid>
</Grid>
```

## How to run this sample

1. Open the `SfDataGridDemo/SfDataGridDemo.sln` file in Visual Studio.
2. Restore the NuGet packages.
3. Build and run the project.

## Output

![Add Columns In ViewModel Using Prism](Add_Columns_In_ViewModel_Using_Prism.png)

Take a moment to peruse the [WPF DataGrid - Columns](https://help.syncfusion.com/wpf/datagrid/columns) documentation to learn more about columns and examples.
