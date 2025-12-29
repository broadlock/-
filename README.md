```
/******************************************************
 * Факультет: ФИТКБ
 * Группа: бИЦ - 251
 * ФИО: Гаркин Алексей Андреевич
 * Курсовая работа: Конструирование программы анализа функции
 ******************************************************/

#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>
#include <locale.h>

#ifdef _WIN32
#include <windows.h>
#endif

 /*
  * Функция: Вычислить значение функции
  * Описание: Возвращает значение f(x) для кусочно-заданной функции
  * Параметры:
  *   x - значение аргумента
  *   status - указатель на флаг ошибки (0 - ошибка)
  * Возвращает: значение f(x)
  */
double calculateFunctionValue(double x, int* status)
{
    if (x < -1)
    {
        // Кубический многочлен
        return x * x * x - 2 * x + 5;
    }
    else if (x >= -1 && x < 1)
    {
        // Особый случай: функция не определена при x = 0
        if (x == 0)
        {
            *status = 0; // Флаг недопустимого значения
            return 0;
        }

        // Выражение с экспонентами
        return (exp(x) - exp(-x)) / (2 * x);
    }
    else
    {
        // Приближение рядом Тейлора
        double result = 1.0;
        result -= (x * x) / 2.0;
        result += pow(x, 4) / 24.0;
        result -= pow(x, 6) / 720.0;
        result += pow(x, 8) / 40320.0;
        return result;
    }
}

/*
 * Функция: Выполнить тестирование функции
 * Описание: Вычисляет значения функции для набора тестов
 *           и выводит результаты в консоль
 */
void runFunctionTests()
{
    // Тестовые значения аргумента
    double testArguments[] =
    { -3.0, -2.5, -2.0, -1.5, -1.0, -0.8, -0.5, -0.2,
      0.2, 0.5, 0.8, 1.0, 1.5, 2.0, 2.5, 3.0 };

    int testCount = sizeof(testArguments) / sizeof(testArguments[0]);

    printf("Результаты тестирования функции:\n");
    printf("%-15s | %-15s\n", "Аргумент x", "f(x)");
    printf("-----------------|-----------------\n");

    for (int i = 0; i < testCount; i++)
    {
        int status = 1; // Инициализация статуса
        double result = calculateFunctionValue(testArguments[i], &status);

        if (status == 0)
        {
            printf("%-15.2lf | %s\n", testArguments[i], "Не определено");
        }
        else
        {
            printf("%-15.2lf | %-15.5lf\n", testArguments[i], result);
        }
    }
}

/*
 * Функция: Инициализировать консоль для русского текста
 * Описание: Настраивает кодировку консоли для корректного
 *           отображения русских символов
 */
void initializeConsole()
{
    // Настройка консоли в зависимости от ОС
#ifdef _WIN32
    // Конфигурация для Windows
    SetConsoleOutputCP(1251);
    SetConsoleCP(1251);
    setlocale(LC_ALL, "Russian");
#else
    // Конфигурация для Linux/macOS
    setlocale(LC_ALL, "ru_RU.UTF-8");
#endif
}

/*
 * Главная функция программы
 * Описание: Организует ввод данных, вычисления и вывод результата
 */
int main()
{
    // Инициализация консоли для русского текста
    initializeConsole();

    // Вывод шапки программы
    printf("******************************************************\n");
    printf("* Факультет: ФИТКБ                                  *\n");
    printf("* Группа: бИЦ - 251                                 *\n");
    printf("* ФИО: Гаркин Алексей Андреевич                     *\n");
    printf("* Курсовая работа: Конструирование программы        *\n");
    printf("*         анализа функции                           *\n");
    printf("******************************************************\n\n");

    double argumentValue;
    int calculationStatus = 1; // Статус вычислений (1 - норма, 0 - ошибка)

    printf("Введите значение x: ");
    if (scanf("%lf", &argumentValue) != 1)
    {
        printf("Ошибка ввода. Пожалуйста, введите корректное числовое значение.\n");
        return 1;
    }

    double functionValue = calculateFunctionValue(argumentValue, &calculationStatus);

    if (calculationStatus == 0)
    {
        printf("Функция не определена для x = 0\n");
    }
    else
    {
        printf("f(%.5lf) = %.5lf\n", argumentValue, functionValue);
    }

    printf("\nПроверка работы программы на наборе тестовых значений:\n");
    runFunctionTests();

    return 0;
}
```
