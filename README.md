Коды и полезные функции

Капча WinForms

'''

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
В событие Click кнопки button2 добавим метод выполняющий проверку введенного текста и сгенерированного:

private void button2_Click(object sender, EventArgs e)
{
if (textBox1.Text == this.text)
MessageBox.Show("Верно!");
else
MessageBox.Show("Ошибка!");
}

В событие Click кнопки button1 добавим метод для генерации новой капчи:
'''
private void button1_Click(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}
'''

В событие Click кнопки button2 добавим метод выполняющий проверку введенного текста и сгенерированного:

'''
private void button2_Click(object sender, EventArgs e)
{
if (textBox1.Text == this.text)
MessageBox.Show("Верно!");
else
MessageBox.Show("Ошибка!");
}

'''

В событие Form1_Load, запуск которого происходит при загрузке проекта, так же добавим генерацию капчи.

'''

private void Form1_Load(object sender, EventArgs e)
{
pictureBox1.Image = this.CreateImage(pictureBox1.Width, pictureBox1.Height);
}

'''


Капча WPF

'''

'''

