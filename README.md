Коды и полезные функции

##  MaterialDesign

```xml
<Grid>
        <Grid>
            <Border MinWidth="100"
                    Margin="15"
                    Background="AliceBlue"
                    VerticalAlignment="Center"
                    Padding="40"
                    MaxHeight="400"
                    CornerRadius="30">
                
                <Border.Effect>
                    <DropShadowEffect BlurRadius="30"
                                      Color="LightGray"
                                      ShadowDepth="0"/>
                </Border.Effect>

                <StackPanel>
                    <TextBlock Text="База данных магазина компьютерной техники"
                               FontSize="30"
                               FontWeight="Bold"
                               Margin="0 0 0 20"/>
                    <TextBox Name="loginField"
                             materialDesign:HintAssist.Hint="Введите логин"
                             Style="{StaticResource MaterialDesignFloatingHintTextBox}"/>
                    <PasswordBox Name="passwordField"
                                 materialDesign:HintAssist.Hint="Введите пароль"
                                 Style="{StaticResource MaterialDesignFloatingHintPasswordBox}"/>

                    <TextBox Name="emailField"
                             materialDesign:HintAssist.Hint="Введите email"
                             Style="{StaticResource MaterialDesignFloatingHintTextBox}"/>
                    <Button Name="createButton"
                            Content="Создать"
                            Margin="0 20"
                            />
                </StackPanel>
            </Border>

        </Grid>
    </Grid>

```


## Page  WPF

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Controls; // for Frame

namespace WpfApp1
{
    public class ManagerPages
    {
        public static Frame Mainframe { get; set; }
    }
}
```

## Обновление объектов базы данных в DataGrid на странице

Событие IsVisibleChanged="Page1_InVisibleChanged"

```Csharp
private void Page1_InVisibleChanged(object sender, DependencyPropertyChangedEventArgs e)
        {
            if (Visibility == Visibility.Visible)
            {
                AppContext db = new AppContext();
                db.ChangeTracker.Entries().ToList().ForEach(p => p.Reload());
                ProductGrid.ItemsSource = db.Users.ToList();
            }
        }
```

## Вызов контекста на кнопке Редактировать

```Csharp
    private void EditButton_Click(object sender, RoutedEventArgs e)
    {
        ManagerPages.Mainframe.Navigate(new Page2((sender as Button).DataContext as User));
    }
```

## Кнопка удаления с диалогом

```Csharp

private void DeleteButton_Click(object sender, RoutedEventArgs e)
        {
            var selectedUsers = ProductGrid.SelectedItems.Cast<User>().ToList();

            if (MessageBox.Show($"Вы точно хотите удалить {selectedUsers.Count()} пользователей", "Внимание!",
                 MessageBoxButton.YesNo, MessageBoxImage.Question) == MessageBoxResult.Yes)
            {
                try
                {
                    AppContext db = new AppContext();
                    db.Users.RemoveRange(selectedUsers);
                    db.SaveChanges();
                    ProductGrid.ItemsSource = db.Users.ToList();
                    MessageBox.Show("Пользователи удалены!");
                }
                catch (Exception ex)
                {
                    MessageBox.Show($"{ex.Message}");
                }
            }
        }
```


## Передача параметров в конструктор для текущего пользователя

```Csharp
private User _currentUser = new User();

        public Page2(User selectedUser)
        {
            InitializeComponent();

            if(selectedUser != null)
            {
                _currentUser = selectedUser;
            }

            DataContext = _currentUser;
        }
```

## SQL Server create foreign key

```SQL
CONSTRAINT [FK_Abiturients_Specialty] FOREIGN KEY ([specialty_id]) REFERENCES [dbo].[Specialty] ([Id])
```

## Автоинкремент в SQL Server 

```SQL
[Id] int Identity(1,1)
```


## Применение глобального шрифта к страницам Page или Window
							    
```Csharp
Style = (Style)FindResource(typeof(Page));
```

## Количество элементов
```Csharp
CountAbiturients.Text = $"Количество: {db.Abiturients.Take(10).ToList().Count} из {db.Abiturients.ToList().Count}";
```

## Include relation

```Csharp
productGrid.ItemsSource = db.Abiturients.Include(p => p.Specialty).ToList();
//Без использования метода Include мы бы не могли бы получить связанную команду и ее свойства: p.Specialty.Name
```

## Переход на страницу и передача объекта
```Csharp
ProxyFrame.Mainframe.Navigate(new AddAbiturientPage(productGrid.SelectedItem as Abiturient));
```

## Переход на страницу Вперед

```Csharp
private void ForwardButton_Click(object sender, RoutedEventArgs e)
    {
        productGrid.ItemsSource = db.Abiturients.Skip(step).Take(10).ToList();
        if (step + 10 < db.Abiturients.Count())
            step += 10;

        CountAbiturients.Text = $"Количество: {db.Abiturients.Skip(step).Take(10).ToList().Count} из {db.Abiturients.ToList().Count}";
    }	
```
						   
## Переход на страницу Назад

```Csharp
private void BackButton_Click(object sender, RoutedEventArgs e)
    {
        if (step > 0)
            step -= 10;
        productGrid.ItemsSource = db.Abiturients.Skip(step).Take(10).ToList();

        CountAbiturients.Text = $"Количество: {db.Abiturients.Skip(step).Take(10).ToList().Count} из {db.Abiturients.ToList().Count}";
    }
```

## Очистка параметров сортировки, фильтрации и поиска

```Csharp
private void Clear_ButtonClick(object sender, RoutedEventArgs e)
    {
        SortCombobox.Text = "Сортировка";
        FilterComboBox.Text = "Все типы";
        SearchBox.Text = "";
    }
```

## Атрибуты для DataGrid

```xml
    Grid.Row="1"
            Margin="5"
            AutoGenerateColumns="False"
            x:Name="productGrid"
            
            MouseDoubleClick="Edit_MouseDoubleClick"
            IsReadOnly="True"
            GridLinesVisibility="None"
            SelectionMode="Extended"
            SelectionUnit="FullRow"
            
            ColumnWidth="Auto"
            HorizontalAlignment="Stretch" 
            VerticalAlignment="Stretch" 
            HorizontalContentAlignment="Stretch"
            EnableRowVirtualization="false"
            EnableColumnVirtualization="false"
            CanUserAddRows="False"
            CanUserReorderColumns="False"
            CanUserResizeColumns="True" IsSynchronizedWithCurrentItem="False"
```

## Триггеры в DataGrid на Row

```xml
    <DataGrid.RowStyle>
        <Style TargetType="DataGridRow">
            <Style.Triggers>
                <DataTrigger Binding="{Binding Ball}" Value="5" >
                    <Setter Property="Background" Value="Orange"/>
                </DataTrigger>
            </Style.Triggers>
        </Style>
    </DataGrid.RowStyle>
```

## Вinding даты через Stringformat
	
```xml
    Binding="{Binding BirthDay, StringFormat={}{0:dd.MM.yyyy}}"
```
	
## Атрибуты окна Window
```xml
 	Title="Главное меню"
        Height="700"
        Width="1100"
        Background="White"
        WindowStartupLocation="CenterScreen"
        MinHeight="500"
        MinWidth="900"
        Icon="emblscc.ico"
```
	
## Авторизация

```Csharp
using(ColledgeStoreContext db = new ColledgeStoreContext())
    {
        var currentUser = db.Users.Where(user => user.Login == LoginBox.Text && user.Password == PasswordBox.Password).FirstOrDefault();
        if (currentUser != null)
        {
            ProxyFrame.CurrentUser = currentUser;
            MainWindow main = new MainWindow();
            main.Show();
            this.Close();
        }
        else
        {
            MessageBox.Show("Неправильный логин или пароль");
        }
    }
```

	
## Валидация

```Csharp
    
    StringBuilder errors = new StringBuilder();
    if (string.IsNullOrWhiteSpace(full_name.Text))
        errors.AppendLine("Укажите имя");
    if (string.IsNullOrWhiteSpace(specialty_id.Text))
        errors.AppendLine("Укажите специальность");
    if (string.IsNullOrWhiteSpace(birth_day.Text))
        errors.AppendLine("Укажите дату рождения");
    if (string.IsNullOrWhiteSpace(date_certificate.Text))
        errors.AppendLine("Укажите дату выдачи аттестата");
    if (string.IsNullOrWhiteSpace(passport_issued.Text))
        errors.AppendLine("Укажите дату выдачи паспорта");

    //
    if (errors.Length > 0)
    {
        MessageBox.Show(errors.ToString());
        return;
    }
```

## ComboBox

```xml
<ComboBox SelectedValue="{Binding Specialty}"
          Text="{Binding Specialty.Name}"
          Name="specialty_id"
          Margin="1"
          Height="30"
          Width="150" 
          IsEditable="True" />
```
	
```csharp
    specialty_id.ItemsSource = db.Specialties.ToList(); // загрузка в комбобокс объектов специальностей
    specialty_id.DisplayMemberPath = "Name"; // отображение в списке объектов конкретные свойства, а не весь объект
```

## Изображение по абсолютному пути

```Csharp
    BitmapImage image = new BitmapImage(new Uri(System.IO.Path.Combine(Environment.CurrentDirectory, $"{_currentAbiturient.Image}"), UriKind.Absolute));
    ImagePicture.Source = image;
```

## Captcha WPF

```Csharp
    public static class CaptchaBuild
        {
            public static string Refresh()
            {

                string captcha = "A1fd";

                Random rand = new Random();

                for (int i = 0; i < 4; i++)
                {
                    captcha += (char)rand.Next('A', 'Z' + 1);
                }

                return captcha;
            }
        }								  
```
				  
```csharp
	CaptchaBox.Visibility = Visibility.Visible;
	CaptchaText.Visibility = Visibility.Visible;
	Captcha.Visibility = Visibility.Visible;
	LoginButtonName.IsEnabled = false;

	MessageBox.Show(CaptchaBuild.Refresh());
	Captcha.Text = CaptchaBuild.Refresh();
	Captcha.IsReadOnly = true;				  
```

```csharp
	private void CaptchaBox_TextChanged(object sender, TextChangedEventArgs e)
		{
		    if (CaptchaBox.Text == Captcha.Text)
		    {
			CaptchaBox.Visibility = Visibility.Collapsed;
			CaptchaText.Visibility = Visibility.Collapsed;
			Captcha.Visibility = Visibility.Collapsed;
			LoginButtonName.IsEnabled = true;
		    }
		    else
		    {
			Captcha.Text = CaptchaBuild.Refresh();
			disableButton();

		    }
		}
```

## Border

```xml
    <Border
        CornerRadius="3"
        BorderThickness="2"
        Width="800"
        Height="Auto"
        BorderBrush="{StaticResource ColorAccent}"

        <Border.Effect>
            <DropShadowEffect BlurRadius="30"
            Color="LightGray"
            ShadowDepth="0"/>
        </Border.Effect>
    </Border>
```

## Начальные значения для сортировки и фильтрации

```csharp
    SortComboBox.ItemsSource = new List<String>() { "Цена", "По убыванию", "По возрастанию" };   //.Select(p => p.Login);
    SortComboBox.SelectedIndex = 0;

    Suppliers.Insert(0, "Все производители");
    FilterComboBox.ItemsSource = Suppliers;
    FilterComboBox.SelectedIndex = 0;
   ```
    
    
   ## Капча WinForms
   

```

private Bitmap CreateImage(int Width, int Height)
{
Random rnd = new Random();

//Создадим изображение
Bitmap result = new Bitmap(Width, Height);

//Вычислим позицию текста
int Xpos = 10;
int Ypos = 10;

//Добавим различные цвета ддя текста
Brush[] colors = {
Brushes.Black,
Brushes.Red,
Brushes.RoyalBlue,
Brushes.Green,
Brushes.Yellow,
Brushes.White,
Brushes.Tomato,
Brushes.Sienna,
Brushes.Pink };

//Добавим различные цвета линий
Pen[] colorpens = {
Pens.Black,
Pens.Red,
Pens.RoyalBlue,
Pens.Green,
Pens.Yellow,
Pens.White,
Pens.Tomato,
Pens.Sienna,
Pens.Pink };

//Делаем случайный стиль текста
FontStyle[] fontstyle = {
FontStyle.Bold,
FontStyle.Italic,
FontStyle.Regular,
FontStyle.Strikeout,
FontStyle.Underline};

//Добавим различные углы поворота текста
Int16[] rotate = {1,-1,2,-2,3,-3,4,-4,5,-5,6,-6};

//Укажем где рисовать
Graphics g = Graphics.FromImage((Image)result);

//Пусть фон картинки будет серым
g.Clear(Color.Gray);

//Делаем случайный угол поворота текста
g.RotateTransform(rnd.Next(rotate.Length));

//Генерируем текст
text = String.Empty;
string ALF = "


7890QWERTYUIOPASDFGHJKLZXCVBNM";
for (int i = 0; i < 5; ++i)
text += ALF[rnd.Next(ALF.Length)];

//Нарисуем сгенирируемый текст
g.DrawString(text,
new Font("Arial", 25, fontstyle[rnd.Next(fontstyle.Length)]),
colors[rnd.Next(colors.Length)],
new PointF(Xpos, Ypos));

//Добавим немного помех
//Линии из углов
g.DrawLine(colorpens[rnd.Next(colorpens.Length)],
new Point(0, 0),
new Point(Width - 1, Height - 1));
g.DrawLine(colorpens[rnd.Next(colorpens.Length)],
new Point(0, Height - 1),
new Point(Width - 1, 0));

//Белые точки
for (int i = 0; i < Width; ++i)
for (int j = 0; j < Height; ++j)
if (rnd.Next() % 20 == 0)
result.SetPixel(i, j, Color.White);

return result;
}
В событие Click кнопки button1 добавим метод для генерации новой капчи:

private void button1_Click(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}
```

 ##В событие Click кнопки button2 добавим метод выполняющий проверку введенного текста и сгенерированного:

  ```

private void button2_Click(object sender, EventArgs e)
{
if (textBox1.Text == this.text)
MessageBox.Show("Верно!");
else
MessageBox.Show("Ошибка!");
}

 ```

  В событие Click кнопки button1 добавим метод для генерации новой капчи:

    ```

private void button1_Click(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}
    ```
    
  В событие Click кнопки button2 добавим метод выполняющий проверку введенного текста и сгенерированного:
    ```

private void button2_Click(object sender, EventArgs e)
{
if (textBox1.Text == this.text)
MessageBox.Show("Верно!");
else
MessageBox.Show("Ошибка!");
}

   ``` 

В событие Form1_Load, запуск которого происходит при загрузке проекта, так же добавим генерацию капчи.

```

private void Form1_Load(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}

```
