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
