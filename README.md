# WPF
## Membuat Student Registration APP dengan WPF
### Langkah-langkah

Run code berikut
```
dotnet new console -n StudentRegistrationApp
cd StudentRegistrationApp
```
kemudian install package Avalonia UI
```
dotnet add package Avalonia --version 11.2.3
dotnet add package Avalonia.Desktop --version 11.2.3
dotnet add package Avalonia.Themes.Fluent --version 11.2.3
dotnet add package Avalonia.Fonts.Inter --version 11.2.3
dotnet add package Avalonia.Controls.DataGrid --version 11.2.3
```
Buka file ``` StudentRegistrationApp.csproj ``` lalu isi dengan code berikut
```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net8.0</TargetFramework>
    <Nullable>enable</Nullable>
    <BuiltInComInteropSupport>true</BuiltInComInteropSupport>
    <AvaloniaUseCompiledBindingsByDefault>false</AvaloniaUseCompiledBindingsByDefault>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Avalonia" Version="11.2.3" />
    <PackageReference Include="Avalonia.Desktop" Version="11.2.3" />
    <PackageReference Include="Avalonia.Themes.Fluent" Version="11.2.3" />
    <PackageReference Include="Avalonia.Fonts.Inter" Version="11.2.3" />
    <PackageReference Include="Avalonia.Controls.DataGrid" Version="11.2.3" />
  </ItemGroup>
</Project>
```
Buka file ```Program.cs``` lalu isi dengan code berikut
```
using Avalonia;
using System;

namespace StudentRegistrationApp;

class Program
{
    [STAThread]
    public static void Main(string[] args) => BuildAvaloniaApp()
        .StartWithClassicDesktopLifetime(args);

    public static AppBuilder BuildAvaloniaApp()
        => AppBuilder.Configure<App>()
            .UsePlatformDetect()
            .WithInterFont()
            .LogToTrace();
}
```
lalu buat file ```App.axaml``` dan isi dengan code berikut
```
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="StudentRegistrationApp.App"
             RequestedThemeVariant="Light">

    <Application.Styles>
        <FluentTheme />
        <StyleInclude Source="avares://Avalonia.Controls.DataGrid/Themes/Fluent.xaml"/>
    </Application.Styles>
</Application>
```
kemudian buat file ```App.axaml.cs``` dan isi dengan code berikut
```
using Avalonia;
using Avalonia.Controls.ApplicationLifetimes;
using Avalonia.Markup.Xaml;

namespace StudentRegistrationApp;

public partial class App : Application
{
    public override void Initialize()
    {
        AvaloniaXamlLoader.Load(this);
    }

    public override void OnFrameworkInitializationCompleted()
    {
        if (ApplicationLifetime is IClassicDesktopStyleApplicationLifetime desktop)
        {
            desktop.MainWindow = new MainWindow();
        }

        base.OnFrameworkInitializationCompleted();
    }
}
```
Buat file baru dengan nama ```Mahasiswa.cs ``` isi dengan code berikut
```
namespace StudentRegistrationApp;

public class Mahasiswa
{
    public string NRP { get; set; } = string.Empty;
    public string Nama { get; set; } = string.Empty;
    public string Prodi { get; set; } = string.Empty;
    public double IPK { get; set; }

    public string IPKDisplay => IPK.ToString("F2");
}
```
Buat file baru bernama ```MainWindow.axaml``` isi dengan code berikut
```
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="StudentRegistrationApp.MainWindow"
        Title="Aplikasi Pendataan Mahasiswa"
        Width="950" Height="650"
        WindowStartupLocation="CenterScreen">

    <Window.Styles>
        <Style Selector="Border.card">
            <Setter Property="Background" Value="#F8F9FA"/>
            <Setter Property="BorderBrush" Value="#E9ECEF"/>
            <Setter Property="BorderThickness" Value="1"/>
            <Setter Property="CornerRadius" Value="8"/>
            <Setter Property="Padding" Value="12"/>
        </Style>
    </Window.Styles>

    <Grid RowDefinitions="Auto, Auto, *, Auto" Margin="16">
        
        <!-- Header -->
        <TextBlock Grid.Row="0" Text="Sistem Manajemen Data Mahasiswa" 
                   FontSize="20" FontWeight="Bold" Margin="0,0,0,16"/>

        <!-- Stat Cards -->
        <Grid Grid.Row="1" ColumnDefinitions="*, *, *" Margin="0,0,0,16">
            <Border Grid.Column="0" Classes="card" Margin="0,0,8,0">
                <StackPanel>
                    <TextBlock Text="Total Mahasiswa" Foreground="Gray" FontSize="12"/>
                    <TextBlock x:Name="TxtTotal" Text="0" FontSize="22" FontWeight="Bold"/>
                </StackPanel>
            </Border>
            <Border Grid.Column="1" Classes="card" Margin="4,0,4,0">
                <StackPanel>
                    <TextBlock Text="Rata-Rata IPK" Foreground="Gray" FontSize="12"/>
                    <TextBlock x:Name="TxtRataIPK" Text="0.00" FontSize="22" FontWeight="Bold"/>
                </StackPanel>
            </Border>
            <Border Grid.Column="2" Classes="card" Margin="8,0,0,0">
                <StackPanel>
                    <TextBlock Text="IPK Tertinggi" Foreground="Gray" FontSize="12"/>
                    <TextBlock x:Name="TxtMaxIPK" Text="0.00" FontSize="22" FontWeight="Bold"/>
                </StackPanel>
            </Border>
        </Grid>

        <!-- Main Content -->
        <Grid Grid.Row="2" ColumnDefinitions="300, *">
            
            <!-- Left Panel: Form Input -->
            <Border Grid.Column="0" Classes="card" Margin="0,0,12,0">
                <StackPanel Spacing="10">
                    <TextBlock x:Name="TxtFormHeader" Text="Tambah Data Baru" FontWeight="Bold" FontSize="14"/>
                    
                    <StackPanel>
                        <TextBlock Text="NRP (10 Digit):" FontSize="12"/>
                        <TextBox x:Name="TxtNRP"/>
                    </StackPanel>

                    <StackPanel>
                        <TextBlock Text="Nama Lengkap:" FontSize="12"/>
                        <TextBox x:Name="TxtNama"/>
                    </StackPanel>

                    <StackPanel>
                        <TextBlock Text="Program Studi:" FontSize="12"/>
                        <TextBox x:Name="TxtProdi"/>
                    </StackPanel>

                    <StackPanel>
                        <TextBlock Text="IPK (0 - 4.00):" FontSize="12"/>
                        <TextBox x:Name="TxtIPK"/>
                    </StackPanel>

                    <StackPanel Orientation="Horizontal" HorizontalAlignment="Right" Spacing="8" Margin="0,10,0,0">
                        <Button x:Name="BtnBatal" Content="Batal" Click="BtnBatal_Click" IsVisible="False"/>
                        <Button x:Name="BtnSimpan" Content="Simpan" Click="BtnSimpan_Click"/>
                    </StackPanel>

                    <TextBlock x:Name="TxtError" Foreground="Red" TextWrapping="Wrap" FontSize="11" Margin="0,5,0,0"/>
                </StackPanel>
            </Border>

            <!-- Right Panel: Grid & Search -->
            <Grid Grid.Column="1" RowDefinitions="Auto, *">
                <TextBox Grid.Row="0" x:Name="TxtCari" Watermark="Cari berdasarkan NRP, Nama, atau Prodi..." 
                         KeyUp="TxtCari_KeyUp" Margin="0,0,0,10"/>

                <Grid Grid.Row="1" RowDefinitions="*, Auto">
                    <DataGrid Grid.Row="0" x:Name="GridData" AutoGenerateColumns="False" 
                              IsReadOnly="True" GridLinesVisibility="All"
                              SelectionChanged="GridData_SelectionChanged">
                        <DataGrid.Columns>
                            <DataGridTextColumn Header="NRP" Binding="{Binding NRP}" Width="120"/>
                            <DataGridTextColumn Header="Nama" Binding="{Binding Nama}" Width="*"/>
                            <DataGridTextColumn Header="Program Studi" Binding="{Binding Prodi}" Width="150"/>
                            <DataGridTextColumn Header="IPK" Binding="{Binding IPKDisplay}" SortMemberPath="IPK" Width="80"/>
                        </DataGrid.Columns>
                    </DataGrid>

                    <StackPanel Grid.Row="1" Orientation="Horizontal" HorizontalAlignment="Right" Spacing="8" Margin="0,10,0,0">
                        <Button x:Name="BtnEdit" Content="Edit" Click="BtnEdit_Click" IsEnabled="False"/>
                        <Button x:Name="BtnHapus" Content="Hapus" Click="BtnHapus_Click" IsEnabled="False"/>
                    </StackPanel>
                </Grid>
            </Grid>
        </Grid>

        <!-- Footer -->
        <TextBlock Grid.Row="3" x:Name="TxtStatus" Text="Siap." Foreground="Gray" FontSize="11" Margin="0,10,0,0"/>
    </Grid>
</Window>
```
Buat file baru bernama ```MainWindow.axaml.cs``` isi dengan code berikut
```
using Avalonia.Controls;
using Avalonia.Input;
using Avalonia.Interactivity;
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Globalization;
using System.Linq;
using System.Threading.Tasks;

namespace StudentRegistrationApp;

public partial class MainWindow : Window
{
    private enum ModeEditor { Tambah, Edit }
    
    private readonly ObservableCollection<Mahasiswa> _masterList = new();
    private ModeEditor _modeAktif = ModeEditor.Tambah;
    private Mahasiswa? _targetEdit = null;

    public MainWindow()
    {
        InitializeComponent();
        GridData.ItemsSource = _masterList;
        RefreshMetrics();
    }

    private void BtnSimpan_Click(object? sender, RoutedEventArgs e)
    {
        TxtError.Text = string.Empty;

        if (!ProsesValidasi(out string pesanPeringatan, out double ipkParsed))
        {
            TxtError.Text = pesanPeringatan;
            return;
        }

        if (_modeAktif == ModeEditor.Tambah)
        {
            var itemBaru = new Mahasiswa
            {
                NRP = TxtNRP.Text?.Trim() ?? "",
                Nama = TxtNama.Text?.Trim() ?? "",
                Prodi = TxtProdi.Text?.Trim() ?? "",
                IPK = ipkParsed
            };
            _masterList.Add(itemBaru);
            TxtStatus.Text = $"Data {itemBaru.NRP} berhasil ditambahkan.";
        }
        else if (_modeAktif == ModeEditor.Edit && _targetEdit != null)
        {
            _targetEdit.NRP = TxtNRP.Text?.Trim() ?? "";
            _targetEdit.Nama = TxtNama.Text?.Trim() ?? "";
            _targetEdit.Prodi = TxtProdi.Text?.Trim() ?? "";
            _targetEdit.IPK = ipkParsed;
            
            GridData.ItemsSource = null;
            ApplyFilter();
            TxtStatus.Text = $"Data {_targetEdit.NRP} berhasil diperbarui.";
        }

        ResetForm();
        RefreshMetrics();
    }

    private bool ProsesValidasi(out string pesan, out double ipk)
    {
        pesan = string.Empty;
        ipk = 0;

        string nrp = TxtNRP.Text?.Trim() ?? "";
        string nama = TxtNama.Text?.Trim() ?? "";
        string prodi = TxtProdi.Text?.Trim() ?? "";
        string rawIpk = TxtIPK.Text?.Trim().Replace(',', '.') ?? "";

        if (string.IsNullOrWhiteSpace(nrp) || nrp.Length != 10 || !nrp.All(char.IsDigit))
        {
            pesan = "NRP wajib berupa 10 digit angka.";
            return false;
        }

        bool isDuplicate = _masterList.Any(m => m.NRP.Equals(nrp, StringComparison.OrdinalIgnoreCase) && m != _targetEdit);
        if (isDuplicate)
        {
            pesan = "NRP sudah terdaftar dalam sistem.";
            return false;
        }

        if (string.IsNullOrWhiteSpace(nama))
        {
            pesan = "Nama tidak boleh kosong.";
            return false;
        }

        if (string.IsNullOrWhiteSpace(prodi))
        {
            pesan = "Program studi tidak boleh kosong.";
            return false;
        }

        if (!double.TryParse(rawIpk, NumberStyles.Any, CultureInfo.InvariantCulture, out ipk) || ipk < 0.0 || ipk > 4.0)
        {
            pesan = "IPK harus berupa angka valid antara 0.00 hingga 4.00.";
            return false;
        }

        return true;
    }

    private void GridData_SelectionChanged(object? sender, SelectionChangedEventArgs e)
    {
        bool adaPilihan = GridData.SelectedItem is Mahasiswa;
        BtnEdit.IsEnabled = adaPilihan;
        BtnHapus.IsEnabled = adaPilihan;
    }

    private void BtnEdit_Click(object? sender, RoutedEventArgs e)
    {
        if (GridData.SelectedItem is Mahasiswa m)
        {
            _targetEdit = m;
            _modeAktif = ModeEditor.Edit;

            TxtNRP.Text = m.NRP;
            TxtNama.Text = m.Nama;
            TxtProdi.Text = m.Prodi;
            TxtIPK.Text = m.IPK.ToString(CultureInfo.InvariantCulture);

            TxtFormHeader.Text = $"Edit Data: {m.NRP}";
            BtnBatal.IsVisible = true;
        }
    }

    private async void BtnHapus_Click(object? sender, RoutedEventArgs e)
    {
        if (GridData.SelectedItem is Mahasiswa m)
        {
            bool konfirmasi = await TampilkanDialogKonfirmasi($"Apakah Anda yakin ingin menghapus mahasiswa {m.Nama} ({m.NRP})?");
            if (konfirmasi)
            {
                _masterList.Remove(m);
                ApplyFilter();
                RefreshMetrics();
                TxtStatus.Text = $"Mahasiswa {m.NRP} telah dihapus.";
            }
        }
    }

    private void BtnBatal_Click(object? sender, RoutedEventArgs e)
    {
        ResetForm();
    }

    private void ResetForm()
    {
        TxtNRP.Text = string.Empty;
        TxtNama.Text = string.Empty;
        TxtProdi.Text = string.Empty;
        TxtIPK.Text = string.Empty;
        TxtError.Text = string.Empty;

        _targetEdit = null;
        _modeAktif = ModeEditor.Tambah;
        TxtFormHeader.Text = "Tambah Data Baru";
        BtnBatal.IsVisible = false;
        GridData.SelectedItem = null;
    }

    private void TxtCari_KeyUp(object? sender, KeyEventArgs e)
    {
        ApplyFilter();
    }

    private void ApplyFilter()
    {
        string query = TxtCari.Text?.Trim().ToLower() ?? "";
        if (string.IsNullOrEmpty(query))
        {
            GridData.ItemsSource = _masterList;
        }
        else
        {
            var filtered = _masterList.Where(m =>
                m.NRP.ToLower().Contains(query) ||
                m.Nama.ToLower().Contains(query) ||
                m.Prodi.ToLower().Contains(query)
            ).ToList();
            GridData.ItemsSource = filtered;
        }
    }

    private void RefreshMetrics()
    {
        int count = _masterList.Count;
        TxtTotal.Text = count.ToString();

        if (count > 0)
        {
            TxtRataIPK.Text = _masterList.Average(m => m.IPK).ToString("F2");
            TxtMaxIPK.Text = _masterList.Max(m => m.IPK).ToString("F2");
        }
        else
        {
            TxtRataIPK.Text = "0.00";
            TxtMaxIPK.Text = "0.00";
        }
    }

    private async Task<bool> TampilkanDialogKonfirmasi(string pesan)
    {
        var dialog = new Window
        {
            Title = "Konfirmasi Hapus",
            Width = 350,
            Height = 150,
            WindowStartupLocation = WindowStartupLocation.CenterOwner
        };

        var result = false;
        var btnYa = new Button { Content = "Ya, Hapus", Margin = new Avalonia.Thickness(5) };
        var btnTidak = new Button { Content = "Batal", Margin = new Avalonia.Thickness(5) };

        btnYa.Click += (_, _) => { result = true; dialog.Close(); };
        btnTidak.Click += (_, _) => { result = false; dialog.Close(); };

        dialog.Content = new StackPanel
        {
            Margin = new Avalonia.Thickness(15),
            Children =
            {
                new TextBlock { Text = pesan, TextWrapping = Avalonia.Media.TextWrapping.Wrap, Margin = new Avalonia.Thickness(0, 0, 0, 15) },
                new StackPanel
                {
                    Orientation = Avalonia.Layout.Orientation.Horizontal,
                    HorizontalAlignment = Avalonia.Layout.HorizontalAlignment.Right,
                    Children = { btnTidak, btnYa }
                }
            }
        };

        await dialog.ShowDialog(this);
        return result;
    }
}
```
lalu jalan kan kode berikut ini
``` dotnet restore ```
lalu rund dengan
``` dotnet run ```

## Hasil Run
<img width="710" height="506" alt="image" src="https://github.com/user-attachments/assets/6555a2ea-4886-4ef1-91d7-1d83811de145" />

### Menambahkan Data
<img width="713" height="509" alt="image" src="https://github.com/user-attachments/assets/27264a6d-8b1b-4d11-a4a1-1ba04446211c" />

### Mengedit Data
<img width="711" height="508" alt="image" src="https://github.com/user-attachments/assets/2cf78a28-5d45-4d2e-8fe4-f0d228ca191d" />
hasil
<img width="712" height="507" alt="image" src="https://github.com/user-attachments/assets/0c2cab4f-e900-43d7-9db4-27c1e05b53c9" />

### Menghapus Data
Warning
<img width="266" height="134" alt="image" src="https://github.com/user-attachments/assets/7846a371-6454-4255-861c-4451d299e3c4" />
Hasil
<img width="713" height="511" alt="image" src="https://github.com/user-attachments/assets/3c0d77c2-67d7-4f1a-a366-995bcb6b2520" />
