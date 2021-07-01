# DevExpress Data Grid for .NET MAUI

The DevExpress Data Grid for .NET MAUI Preview 5 supports the following column types: 
- **TemplateColumn** - allows you to define a custom template for column cells.
- **ImageColumn** - displays images. 

This example allows you to get started with the DataGridView component - bind it to a data source and configure its columns.

Open the solution in Visual Studio 16.11 Preview 2 and restore NuGet packages to run the application:

1. [Obtain your NuGet feed URL](http://docs.devexpress.com/GeneralInformation/116042/installation/install-devexpress-controls-using-nuget-packages/obtain-your-nuget-feed-url).
2. Register the DevExpress NuGet feed as a package source.
3. Restore all NuGet packages for the solution.

Run the application on an Android device or emulator.

<img src="./img/devexpress-maui-data-grid.png"/>

The following step-by-step instructions describe how to create the same application.

## Create a New MAUI Application and Add a Data Grid

Create a new .NET MAUI solution in Visual Studio 16.11 Preview 2.  
Refer to the following Microsoft topic for more information on how to get started with .NET MAUI: [.NET Multi-platform App UI documentation](https://docs.microsoft.com/en-gb/dotnet/maui/).

Add the DevExpress Data Grid component to your solution as follows: 
1. [Obtain your NuGet feed URL](http://docs.devexpress.com/GeneralInformation/116042/installation/install-devexpress-controls-using-nuget-packages/obtain-your-nuget-feed-url).
2. Register the DevExpress NuGet feed as a package source. 
3. Install the **DevExpress.MAUI.DataGrid** package from the DevExpress NuGet feed.

In the *Startup.cs* file, register a handler for the DevExpress DataGridView:

```cs
using Microsoft.Maui;
using Microsoft.Maui.Hosting;
using Microsoft.Maui.Controls.Hosting;
using DevExpress.Maui.DataGrid;

namespace DataGridExample {
	public class Startup : IStartup {
		public void Configure(IAppHostBuilder appBuilder) {
			appBuilder
				.ConfigureMauiHandlers((_, handlers) => 
                                        handlers.AddHandler<DataGridView, DataGridViewHandler>())
				.UseMauiApp<App>()
				.ConfigureFonts(fonts => {
					fonts.AddFont("OpenSans-Regular.ttf", "OpenSansRegular");
				});
		}
	}
}
```

In the *MainPage.xaml* file, use the *dxg* prefix to declare the **DevExpress.Maui.DataGrid** namespace and add a **DataGridView** instance to the ContentPage:

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataGridExample.MainPage"
             xmlns:dxg="clr-namespace:DevExpress.Maui.DataGrid;assembly=DevExpress.Maui.DataGrid">
    <dxg:DataGridView>
    </dxg:DataGridView>
</ContentPage>
```

## Create a Data Source
In this example, the grid is bound to a collection of *Employee* objects - *EmployeeData*. Create a *Model.cs* file with the following classes:

```cs
public enum AccessLevel {
    Admin,
    User
}

public class Employee {
    string name;
    string resourceName;

    public string Name {
        get { return name; }
        set {
            name = value;
            if (Photo == null) {
                resourceName = "DataGridExample.Images." + value.Replace(" ", "_") + ".jpg";
                if (!String.IsNullOrEmpty(resourceName))
                    Photo = ImageSource.FromResource(resourceName);
            }
        }
    }

    public Employee(string name) {
        this.Name = name;
    }

    public ImageSource Photo { get; set; }
    public DateTime BirthDate { get; set; }
    public DateTime HireDate { get; set; }
    public string Position { get; set; }
    public string Address { get; set; }
    public string Phone { get; set; }
    public AccessLevel Access { get; set; }
    public bool OnVacation { get; set; }
}

public class EmployeeData {
    void GenerateEmployees() {
        ObservableCollection<Employee> result = new ObservableCollection<Employee>();
        result.Add(
            new Employee("Nancy Davolio") {
                BirthDate = new DateTime(1978, 12, 8),
                HireDate = new DateTime(2005, 5, 1),
                Position = "Sales Representative",
                Address = "98122, 507 - 20th Ave. E. Apt. 2A, Seattle WA, USA",
                Phone = "(206) 555-9857",
                Access = AccessLevel.User,
                OnVacation = false
            }
        );
        result.Add(
            new Employee("Andrew Fuller") {
                BirthDate = new DateTime(1965, 2, 19),
                HireDate = new DateTime(1992, 8, 14),
                Position = "Vice President, Sales",
                Address = "98401, 908 W. Capital Way, Tacoma WA, USA",
                Phone = "(206) 555-9482",
                Access = AccessLevel.Admin,
                OnVacation = false
            }
        );
        result.Add(
            new Employee("Janet Leverling") {
                BirthDate = new DateTime(1985, 8, 30),
                HireDate = new DateTime(2002, 4, 1),
                Position = "Sales Representative",
                Address = "98033, 722 Moss Bay Blvd., Kirkland WA, USA",
                Phone = "(206) 555-3412",
                Access = AccessLevel.User,
                OnVacation = false
            }
        );
        result.Add(
            new Employee("Margaret Peacock") {
                BirthDate = new DateTime(1973, 9, 19),
                HireDate = new DateTime(1993, 5, 3),
                Position = "Sales Representative",
                Address = "98052, 4110 Old Redmond Rd., Redmond WA, USA",
                Phone = "(206) 555-8122",
                Access = AccessLevel.User,
                OnVacation = false
            }
        );
        result.Add(
            new Employee("Steven Buchanan") {
                BirthDate = new DateTime(1955, 3, 4),
                HireDate = new DateTime(1993, 10, 17),
                Position = "Sales Manager",
                Address = "SW1 8JR, 14 Garrett Hill, London, UK",
                Phone = "(71) 555-4848",
                Access = AccessLevel.User,
                OnVacation = true
            }
        );
        result.Add(
            new Employee("Michael Suyama") {
                BirthDate = new DateTime(1981, 7, 2),
                HireDate = new DateTime(1999, 10, 17),
                Position = "Sales Representative",
                Address = "EC2 7JR, Coventry House Miner Rd., London, UK",
                Phone = "(71) 555-7773",
                Access = AccessLevel.User,
                OnVacation = false
            }
        );
        result.Add(
            new Employee("Robert King") {
                BirthDate = new DateTime(1960, 5, 29),
                HireDate = new DateTime(1994, 1, 2),
                Position = "Sales Representative",
                Address = "RG1 9SP, Edgeham Hollow Winchester Way, London, UK",
                Phone = "(71) 555-5598",
                Access = AccessLevel.User,
                OnVacation = false
            }
        );
        result.Add(
            new Employee("Laura Callahan") {
                BirthDate = new DateTime(1985, 1, 9),
                HireDate = new DateTime(2004, 3, 5),
                Position = "Inside Sales Coordinator",
                Address = "98105, 4726 - 11th Ave. N.E., Seattle WA, USA",
                Phone = "(206) 555-1189",
                Access = AccessLevel.User,
                OnVacation = true
            }
        );
        result.Add(
            new Employee("Anne Dodsworth") {
                BirthDate = new DateTime(1980, 1, 27),
                HireDate = new DateTime(2004, 11, 15),
                Position = "Sales Representative",
                Address = "WG2 7LT, 7 Houndstooth Rd., London, UK",
                Phone = "(71) 555-4444",
                Access = AccessLevel.User,
                OnVacation = false
            }
        );
        Employees = result;
    }

    public ObservableCollection<Employee> Employees { get; private set; }

    public EmployeeData() {
        GenerateEmployees();
    }
}
```

Create a *ViewModel.cs* file and add a view model class: 

```cs
using System.Collections.Generic;
using System.ComponentModel;

namespace DataGridExample {
    public class EmployeeDataViewModel : INotifyPropertyChanged {
        readonly EmployeeData data;

        public event PropertyChangedEventHandler PropertyChanged;
        public IReadOnlyList<Employee> Employees { get => data.Employees; }

        public EmployeeDataViewModel() {
            data = new EmployeeData();
        }

        protected void RaisePropertyChanged(string name) {
            if (PropertyChanged != null)
                PropertyChanged(this, new PropertyChangedEventArgs(name));
        }
    }
}
```

## Bind the Grid to Data
In the *MainPage.xaml* file:
1. Assign an **EmployeeDataViewModel** object to the **ContentPage.BindingContext** property.
2. Bind the **DataGridView.ItemsSource** property to the employee collection object that the **EmployeeDataViewModel.Employees** property returns.

```xaml
<ContentPage xmlns="http://schemas.microsoft.com/dotnet/2021/maui"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataGridExample.MainPage"
             xmlns:dxg="clr-namespace:DevExpress.Maui.DataGrid;assembly=DevExpress.Maui.DataGrid"
             xmlns:local="clr-namespace:DataGridExample">
    <ContentPage.BindingContext>
        <local:EmployeeDataViewModel/>
    </ContentPage.BindingContext>
    <dxg:DataGridView ItemsSource="{Binding Employees}">
    </dxg:DataGridView>
</ContentPage>
```

## Specify Grid Columns

Do the following to specify a collection of grid columns:
1. Create column objects and use the **FieldName** property to bind each column to a data source field.
2. Add columns to the **DataGridView.Columns** collection in the order you want them to be displayed in the grid.

In this example, the grid contains the following columns:

- Photo (**ImageColumn**) - displays photos of employees. Add images to a project as embedded resources.

- Employee (**TemplateColumn**) - displays names, positions, and hire dates of employees. 

    Assign a template to the **TemplateColumn.DisplayTemplate** property to define the appearance of column cells. Each cell contains a *Microsoft.Maui.Controls.Grid* with three *Microsoft.Maui.Controls.Label* elements bound to the *Name*, *Position*, and *HireDate* properties of the *Employee* class.
    
    The **CellData** object specifies a binding context for a cell template. Its **CellData.Value** property returns a value of a data field assigned to the column’s **FieldName** property. In this example, a column cell displays not only this field value but also the values of two more fields. Use the **CellData.Item** property to access the whole data row object (*Employee*) and bind its properties to properties of labels defined in the template.

- Access Level (**TemplateColumn**) - displays employee access level.  

    Set the **TemplateColumn.DisplayTemplate** property to a data template with a *Microsoft.Maui.Controls.Label*. Use the **Value** property of the template's binding context to bind the label to the *Employee.Access* property assigned to the column's **FieldName**.
    
```xaml
<dxg:DataGridView ItemsSource="{Binding Employees}">

    <dxg:DataGridView.Columns>
        <dxg:ImageColumn FieldName="Photo"
                            Width="100"/>
        <dxg:TemplateColumn FieldName="Name" Caption="Employee" MinWidth="200">
            <dxg:TemplateColumn.DisplayTemplate>
                <DataTemplate>
                    <Grid VerticalOptions="Center" Padding="15, 0, 0, 0" RowDefinitions="Auto, Auto, Auto">
                        <Label Text="{Binding Item.Name}" FontSize="18" FontAttributes="Bold"
                               TextColor="{DynamicResource GridCellFontColor}" Grid.Row="0" />
                        <Label Text="{Binding Item.Position, StringFormat = 'Job Title: {0}'}"
                               FontSize="Small" TextColor="{DynamicResource GridCellFontColor}" 
                               Grid.Row="1"/>
                        <Label Text="{Binding Item.HireDate, StringFormat = 'Hire Date: {0:d}'}"
                               FontSize="Small" TextColor="{DynamicResource GridCellFontColor}" 
                               Grid.Row="2" />
                    </Grid>
                </DataTemplate>
            </dxg:TemplateColumn.DisplayTemplate>
        </dxg:TemplateColumn>

        <dxg:TemplateColumn FieldName="Access" Caption="Access Level" Width="90">
            <dxg:TemplateColumn.DisplayTemplate>
                <DataTemplate>
                    <Label Text="{Binding Value}" TextColor="{DynamicResource GridCellFontColor}"
                            Grid.Row="0" Padding="14, 21, 14, 21" VerticalOptions="Center"/>
                </DataTemplate>
            </dxg:TemplateColumn.DisplayTemplate>
        </dxg:TemplateColumn>
    </dxg:DataGridView.Columns>
</dxg:DataGridView>
```

## Enable Drag-and-Drop
The DataGridView supports drag-and-drop operations and allows users to reorder rows. Users should touch and hold a data row and then drag and drop the row to another position.

To enable drag-and-drop operations, set the **AllowDragDropRows** property to **True**.
```xaml
<dxg:DataGridView ItemsSource="{Binding Employees}" AllowDragDropRows="True"/>
```
