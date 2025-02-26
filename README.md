Kréta
ViewModels és a Views mappa létrehozása, ha nincs

ViewModels
	-ControlPanelViewModel.cs
	-MainWindowViewModel.cs		--csak ebbe fogunk dolgozni
	-SchoolClassViewModel.cs
	-StudentViewModel.cs

Views
	-ControlPanelView.xaml
	-SchoolClassView.xaml		--több dolgunk ezekkel nincs
	-StudentView.xaml


MainWindow.xaml

Hozzá adjuk a View és a ViewModels mappát

xmlns:viewmodel="clr-namespace:KretaProject.ViewModels"
xmlns:view="clr-namespace:KretaProject.Views"

<Grid>-et kicseréljük <StackPanel>-re

<StackPanel>
    <DockPanel>
        <Menu>
            <MenuItem Header="Főmenüpont">
                <MenuItem Header="Vezérlőpult"></MenuItem>
                <MenuItem Header="Diákok"></MenuItem>
                <MenuItem Header="Osztályok"></MenuItem>
            </MenuItem>
        </Menu>
    </DockPanel>
</StackPanel>

Toolsba hozzá adjuk a CommunityToolkit.Mvvm-et


MainWindowViewModel.cs

public partial class MainWindowViewModel : ObservableObject
{
    private readonly StudentViewModel _studentViewModel= new StudentViewModel();
    private readonly SchoolClassViewModel _schoolClassViewModel = new SchoolClassViewModel();
    private readonly ControlPanelViewModel _controlPanelViewModel = new ControlPanelViewModel();

    [ObservableProperty]
    public object _currentView = new object();

    public MainWindowViewModel()
    {
        _currentView = _controlPanelViewModel;
    }

    [RelayCommand]
    private void ShowControlPanalView()
    {
        CurrentView = _controlPanelViewModel;
    }

    [RelayCommand]
    private void ShowStudentView()
    {
        CurrentView = _studentViewModel;
    }

    [RelayCommand]	
    private void ShowSchoolClassView()
    {
        CurrentView = _schoolClassViewModel;
    }

}

Vissza megyünk a MainWindow.xaml-re

Hozzá adjuk a Binding részt a Headerekhez és hozzá adjuk a CurrentView-t

<StackPanel>
    <DockPanel>
        <Menu>
            <MenuItem Header="Főmenüpont">
                <MenuItem Header="Vezérlőpult" Command="{Binding ShowControlPanalViewCommand}"></MenuItem>
                <MenuItem Header="Diákok" Command="{Binding ShowStudentViewCommand}" ></MenuItem>
                <MenuItem Header="Osztályok" Command="{Binding ShowSchoolClassViewCommand}"></MenuItem>
            </MenuItem>
        </Menu>
    </DockPanel>
    <ContentControl Content="{Binding CurrentView}" />
</StackPanel>

Még hozzá kell adni a DataContextet és a Window.Resources-t és akkor a teljes kódunk így fog kinézni

<Window.DataContext>
    <viewmodel:MainWindowViewModel />
</Window.DataContext>
<Window.Resources>
    <DataTemplate DataType="{x:Type viewmodel:ControlPanelViewModel}">
        <view:ControlPanelView />
    </DataTemplate>
    <DataTemplate DataType="{x:Type viewmodel:StudentViewModel}">
        <view:StudentView />
    </DataTemplate>
    <DataTemplate DataType="{x:Type viewmodel:SchoolClassViewModel}">
        <view:SchoolClassView />
    </DataTemplate>
</Window.Resources>
<StackPanel>
    <DockPanel>
        <Menu>
            <MenuItem Header="Főmenüpont">
                <MenuItem Header="Vezérlőpult" Command="{Binding ShowControlPanalViewCommand}"></MenuItem>
                <MenuItem Header="Diákok" Command="{Binding ShowStudentViewCommand}" ></MenuItem>
                <MenuItem Header="Osztályok" Command="{Binding ShowSchoolClassViewCommand}"></MenuItem>
            </MenuItem>
        </Menu>
    </DockPanel>
    <ContentControl Content="{Binding CurrentView}" />
</StackPanel>




Készítsen alkalmazást OrderProject néven. 
Legyen az alkalmazásban egy "Áru", "Felhasználók" és "Megrendelés" menüpont. 
A "Felhasználók" menüpontból nyíljon egy "Kezelés" és "Módosítás" almenüpont. 
A menüpontok, almenüpontok kiválasztásakor jelenjen meg egy form, amelyen egy felirat jelezze, hogy melyik menüpont lett kiválasztva. 
A program indulásakor az "Árú" menüpont jelenjen meg.

MainWindowModel.cs

using CommunityToolkit.Mvvm.ComponentModel;
using CommunityToolkit.Mvvm.Input;

namespace OrderProject.ViewModels
{
    public partial class MainWindowViewModel : ObservableObject
    {
        private readonly ProductViewModel _productViewModel = new ProductViewModel();
        private readonly OrderViewModel _orderViewModel = new OrderViewModel();
        private readonly UserManageViewModel _userManageViewModel = new UserManageViewModel();
        private readonly UserModifyViewModel _userModifyViewModel = new UserModifyViewModel();

        [ObservableProperty]
        private object _currentView;

        public MainWindowViewModel()
        {
            CurrentView = _productViewModel; // Alapértelmezett nézet: Áru
        }

        [RelayCommand]
        private void ShowProductView()
        {
            CurrentView = _productViewModel;
        }

        [RelayCommand]
        private void ShowOrderView()
        {
            CurrentView = _orderViewModel;
        }

        [RelayCommand]
        private void ShowUserManageView()
        {
            CurrentView = _userManageViewModel;
        }

        [RelayCommand]
        private void ShowUserModifyView()
        {
            CurrentView = _userModifyViewModel;
        }
    }
}



MainWindow.xaml

<Window x:Class="OrderProject.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:viewmodel="clr-namespace:OrderProject.ViewModels"
        xmlns:view="clr-namespace:OrderProject.Views"
        Title="Order Project" Height="350" Width="525">
    
    <Window.DataContext>
        <viewmodel:MainWindowViewModel />
    </Window.DataContext>

    <Window.Resources>
        <DataTemplate DataType="{x:Type viewmodel:ProductViewModel}">
            <view:ProductView />
        </DataTemplate>
        <DataTemplate DataType="{x:Type viewmodel:OrderViewModel}">
            <view:OrderView />
        </DataTemplate>
        <DataTemplate DataType="{x:Type viewmodel:UserManageViewModel}">
            <view:UserManageView />
        </DataTemplate>
        <DataTemplate DataType="{x:Type viewmodel:UserModifyViewModel}">
            <view:UserModifyView />
        </DataTemplate>
    </Window.Resources>

    <StackPanel>
        <DockPanel>
            <Menu>
                <MenuItem Header="Főmenüpont">
                    <MenuItem Header="Áru" Command="{Binding ShowProductViewCommand}"/>
                    <MenuItem Header="Megrendelés" Command="{Binding ShowOrderViewCommand}"/>
                    <MenuItem Header="Felhasználók">
                        <MenuItem Header="Kezelés" Command="{Binding ShowUserManageViewCommand}"/>
                        <MenuItem Header="Módosítás" Command="{Binding ShowUserModifyViewCommand}"/>
                    </MenuItem>
                </MenuItem>
            </Menu>
        </DockPanel>
        <ContentControl Content="{Binding CurrentView}" />
    </StackPanel>
</Window>




