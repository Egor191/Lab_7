# Lab_7
#include<iostream>

#include <stdio.h>

#include <conio.h>

#include <locale.h>

#include <stdlib.h>

#define n 5

using namespace std;

void InitMatr (int a[n][n]) //Ввод элементов матрицы с клавиатуры

{

int i, j;

for (i = 0; i < n; i++)

for (j = 0; j < n; j++)

{

cout << "a[" << i + 1 << "][" << j + 1 << "]= ";

cin >> a[i][j];

}

cout << "\n";

}

void InitMatr_f (int a[n][n]) //Получение элементов матрицы из файла

{

int i, j;

FILE* fl;

fopen_s (&fl, "init.txt", "r");

if (fl)

{

for (i = 0; i < n; i++)

for (j = 0; j < n; j++)

fscanf_s (fl, "%d", &a[i][j]);

fclose (fl);

}

else cout << "Ошибка открытия файла. \n";

}

int* Mas_x (int(*a)[n], int* x, //Формирование массива X

void (*init)(int[n][n]))

{

int i, j, e;

init(a); //Вызов функции через указатель (для ввода элементов матрицы)

for (i = 0; i < n; i++)

{

e = j = 0;

while (j < n && e == 0)

{

if (a[i][j] < 0)

e = a[i][j];

j = j + 1;

}

x[i] = e;

}

return x;

}

int seek_y (int y, int x[], int i) //Поиск величины Y

{

if (i < n)

{

if (x[i] < 0) //Поиск отрицательного элемента массива X

{

y = i;

}

if (y == -1)

y = seek_y (y, x, i+1); //Рекурсивная функция

}

return y;

}

void output (const int a[][n], const int x[], const int y) //Вывод на экран и в файл

{

int i, j;

cout << "Матрица:\n";

for (i = 0; i < n; i++)

{

for (j = 0; j < n; j++)

cout << a[i][j] << " ";

cout << "\n";

}

cout << "\n";

cout << "Массив:\n";

for (i = 0; i < n; i++)

cout << x[i] << " ";

cout << "\n\n";

if (y != -1)

{

cout << "Y: " << y;

cout << "\n";

}

else cout << "Отрицательных элементов в массиве нет.\n ";

FILE* fl;

fopen_s(&fl, "output.txt", "w");

if (fl)

{

cout << "\n";

fprintf(fl, "Матрица:\n");

for (i = 0; i < n; i++)

{

for (j = 0; j < n; j++)

fprintf(fl, "%3d", a[i][j]);

fprintf(fl, "\n");

}

fprintf(fl, "Маccив:\n");

for (i = 0; i < n; i++)

fprintf(fl, "%3d", x[i]);

fprintf(fl, "\n");

fprintf(fl, "Y: ");

fprintf(fl, "%d", y);

fclose(fl);

}

else cout << "Ошибка открытия файла. \n";

}

int main()

{

setlocale(LC_CTYPE, "");

int a[n][n], x[n], y, choise;

void (*init)(int[n][n]);

y = -1;

do

{

cout << "Ввод матрицы: 0 - с клавиатуры, 1 - из файла\nВыбор варианта ввода: ";

cin >> choise;

}

while (choise != 0 && choise != 1);

if (choise == 0)

{

init = &InitMatr;

}

else init = &InitMatr_f;

output(a, Mas_x(a, x, init), seek_y(y, x, 0));

system("pause");

}
