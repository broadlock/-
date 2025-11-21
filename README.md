# - код по курсовой

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>
#include <float.h>


double f(double x) {
    if (x < -1.0) {
        
        return x * x * x - 2 * x + 5;
    }
    else if (x < 1.0) {
        
        if (x == 0.0) return 1.0; 
        return (exp(x) - exp(-x)) / (2 * x);
    }
    else {
        
        double term1 = 1.0;
        double term2 = -x * x / 2.0;
        double term3 = x * x * x * x / 24.0;
        double term4 = -x * x * x * x * x * x / 720.0;
        double term5 = x * x * x * x * x * x * x * x / 40320.0;
        return term1 + term2 + term3 + term4 + term5;
    }
}


double derivative(double x, double h) {
    return (f(x + h) - f(x - h)) / (2 * h);
}


void print_table(double start, double end, double step) {
    printf("x\t\tf(x)\n");
    printf("----------------------------\n");
    for (double x = start; x <= end; x += step) {
        printf("%.6f\t%.6f\n", x, f(x));
    }
}


void find_min_max(double a, double b, double step) {
    double min_val = DBL_MAX;
    double max_val = -DBL_MAX;
    double min_x = a;
    double max_x = a;

    for (double x = a; x <= b; x += step) {
        double val = f(x);
        if (val < min_val) {
            min_val = val;
            min_x = x;
        }
        if (val > max_val) {
            max_val = val;
            max_x = x;
        }
    }

    printf("Минимум: f(%.6f) = %.6f\n", min_x, min_val);
    printf("Максимум: f(%.6f) = %.6f\n", max_x, max_val);
}


void check_monotonicity(double a, double b, double step) {
    int increasing = 1;
    int decreasing = 1;
    double prev = f(a);

    for (double x = a + step; x <= b; x += step) {
        double current = f(x);
        if (current < prev) increasing = 0;
        if (current > prev) decreasing = 0;
        prev = current;
    }

    if (increasing) {
        printf("Функция монотонно возрастает на [%.2f, %.2f]\n", a, b);
    }
    else if (decreasing) {
        printf("Функция монотонно убывает на [%.2f, %.2f]\n", a, b);
    }
    else {
        printf("Функция не монотонна на [%.2f, %.2f]\n", a, b);
    }
}

int main() {
    int choice;
    double x, a, b, step, h;

    printf("Кусочно-заданная функция:\n");
    printf("1. x^3 - 2x + 5\t\t\t(x < -1)\n");
    printf("2. (e^x - e^-x)/(2x)\t\t(-1 <= x < 1, x≠0)\n");
    printf("3. Ряд Тейлора cos(x)\t\t(x >= 1)\n\n");

    do {
        printf("\nМеню:\n");
        printf("1. Значение функции в точке\n");
        printf("2. Таблица значений на интервале\n");
        printf("3. Поиск минимума/максимума\n");
        printf("4. Проверка монотонности\n");
        printf("5. Производная в точке\n");
        printf("6. Выход\n");
        printf("Выберите операцию: ");
        scanf("%d", &choice);

        switch (choice) {
        case 1:
            printf("Введите x: ");
            scanf("%lf", &x);
            printf("f(%.6f) = %.6f\n", x, f(x));
            break;

        case 2:
            printf("Введите начало интервала: ");
            scanf("%lf", &a);
            printf("Введите конец интервала: ");
            scanf("%lf", &b);
            printf("Введите шаг: ");
            scanf("%lf", &step);
            print_table(a, b, step);
            break;

        case 3:
            printf("Введите начало отрезка: ");
            scanf("%lf", &a);
            printf("Введите конец отрезка: ");
            scanf("%lf", &b);
            printf("Введите шаг поиска: ");
            scanf("%lf", &step);
            find_min_max(a, b, step);
            break;

        case 4:
            printf("Введите начало интервала: ");
            scanf("%lf", &a);
            printf("Введите конец интервала: ");
            scanf("%lf", &b);
            printf("Введите шаг проверки: ");
            scanf("%lf", &step);
            check_monotonicity(a, b, step);
            break;

        case 5:
            printf("Введите точку x: ");
            scanf("%lf", &x);
            printf("Введите шаг h (рекомендуется 0.0001): ");
            scanf("%lf", &h);
            printf("f'(%.6f) = %.6f\n", x, derivative(x, h));
            break;

        case 6:
            printf("Выход...\n");
            break;

        default:
            printf("Неверный выбор!\n");
        }
    } while (choice != 6);

    return 0;
}
```







Tima:
```c
#include <stdio.h>
#include <math.h>
#include <stdlib.h>
#include <float.h>

#define M_PI 3.14159265358979323846

// Функция вычисления f(x)
double f(double x) {
    if (x < 0) {
        // f(x) = (cos x - 1)/x для x < 0
        return (cos(x) - 1) / x;
    }
    else if (x >= 0 && x < 1) {
        // f(x) = ln(x) / ∛x для 0 ≤ x < 1
        if (x == 0) {
            return NAN; // Неопределено при x=0
        }
        return log(x) / pow(x, 1.0/3.0);
    }
    else {
        // f(x) = Σ(x^n / arctan(n+1)) для n=0..8 при x ≥ 1
        double sum = 0.0;
        for (int n = 0; n <= 8; n++) {
            sum += pow(x, n) / atan(n + 1);
        }
        return sum;
    }
}

// Вычисление значения функции в точке
void calculate_value() {
    double x;
    printf("Введите x: ");
    if (scanf("%lf", &x) != 1) {
        printf("Ошибка: некорректный ввод!\n");
        while (getchar() != '\n'); // Очистка буфера
        return;
    }
    
    double result = f(x);
    if (isnan(result)) {
        printf("f(%.2f) не определена\n", x);
    } else {
        printf("f(%.2f) = %.6f\n", x, result);
    }
}

// Вывод таблицы значений
void print_table() {
    double start, end, step;
    printf("Введите начало интервала: ");
    if (scanf("%lf", &start) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    printf("Введите конец интервала: ");
    if (scanf("%lf", &end) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    printf("Введите шаг: ");
    if (scanf("%lf", &step) != 1 || step <= 0) {
        printf("Ошибка: шаг должен быть положительным!\n");
        return;
    }
    
    printf("\n   x        f(x)\n");
    printf("-------------------\n");
    
    for (double x = start; x <= end; x += step) {
        double result = f(x);
        if (isnan(result)) {
            printf("%6.2f    не определена\n", x);
        } else {
            printf("%6.2f    %10.6f\n", x, result);
        }
    }
}

// Вычисление интеграла методом трапеций
void calculate_integral() {
    double a, b;
    int n;
    
    printf("Введите нижний предел a: ");
    if (scanf("%lf", &a) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    printf("Введите верхний предел b: ");
    if (scanf("%lf", &b) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    printf("Введите количество разбиений: ");
    if (scanf("%d", &n) != 1 || n <= 0) {
        printf("Ошибка: количество разбиений должно быть положительным!\n");
        return;
    }
    
    if (a >= b) {
        printf("Ошибка: a должно быть меньше b!\n");
        return;
    }
    
    double h = (b - a) / n;
    double sum = 0.0;
    int valid_points = 0;
    
    for (int i = 0; i <= n; i++) {
        double x = a + i * h;
        double fx = f(x);
        
        if (!isnan(fx)) {
            if (i == 0 || i == n) {
                sum += fx / 2.0;
            } else {
                sum += fx;
            }
            valid_points++;
        }
    }
    
    if (valid_points < 2) {
        printf("Недостаточно точек для вычисления интеграла!\n");
        return;
    }
    
    double integral = sum * h;
    printf("∫f(x)dx от %.2f до %.2f = %.6f\n", a, b, integral);
}

// Поиск x: f(x) ≥ Y
void find_x_greater_than_y() {
    double Y;
    printf("Введите Y: ");
    if (scanf("%lf", &Y) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    
    double start, end, step;
    printf("Введите начало поиска: ");
    if (scanf("%lf", &start) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    printf("Введите конец поиска: ");
    if (scanf("%lf", &end) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    printf("Введите шаг поиска: ");
    if (scanf("%lf", &step) != 1 || step <= 0) {
        printf("Ошибка: шаг должен быть положительным!\n");
        return;
    }
    
    printf("\nТочки, где f(x) ≥ %.2f:\n", Y);
    printf("   x        f(x)\n");
    printf("-------------------\n");
    
    int found = 0;
    for (double x = start; x <= end; x += step) {
        double result = f(x);

if (!isnan(result) && result >= Y) {
            printf("%6.2f    %10.6f\n", x, result);
            found = 1;
        }
    }
    
    if (!found) {
        printf("Точки не найдены в заданном диапазоне\n");
    }
}

// Вычисление производной в точке
void calculate_derivative() {
    double x;
    printf("Введите x: ");
    if (scanf("%lf", &x) != 1) {
        printf("Ошибка ввода!\n");
        return;
    }
    
    double h = 1e-8; // Малое приращение
    double f1 = f(x + h);
    double f2 = f(x - h);
    
    if (isnan(f1) || isnan(f2)) {
        printf("Производная в точке %.2f не может быть вычислена\n", x);
        return;
    }
    
    double derivative = (f1 - f2) / (2 * h);
    printf("f'(%.2f) = %.6f\n", x, derivative);
}

// Главное меню
void print_menu() {
    printf("\n=== АНАЛИЗ ФУНКЦИИ ===\n");
    printf("1. Значение функции в точке\n");
    printf("2. Таблица значений на интервале\n");
    printf("3. Вычисление определенного интеграла\n");
    printf("4. Поиск x: f(x) ≥ Y\n");
    printf("5. Производная в точке\n");
    printf("6. Выход\n");
    printf("Выберите операцию: ");
}

int main() {
    int choice;
    
    printf("Программа анализа функции:\n");
    printf("f(x) = (cos x - 1)/x, x < 0\n");
    printf("f(x) = ln(x)/∛x, 0 ≤ x < 1\n");
    printf("f(x) = Σ(x^n/arctan(n+1)), x ≥ 1\n");
    
    while (1) {
        print_menu();
        
        if (scanf("%d", &choice) != 1) {
            printf("Ошибка: введите число от 1 до 6!\n");
            while (getchar() != '\n'); // Очистка буфера
            continue;
        }
        
        switch (choice) {
            case 1:
                calculate_value();
                break;
            case 2:
                print_table();
                break;
            case 3:
                calculate_integral();
                break;
            case 4:
                find_x_greater_than_y();
                break;
            case 5:
                calculate_derivative();
                break;
            case 6:
                printf("Выход из программы...\n");
                return 0;
            default:
                printf("Ошибка: выберите число от 1 до 6!\n");
        }
    }
    
    return 0;
}
```