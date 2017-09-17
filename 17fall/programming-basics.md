# Основы программирования, первый семестр

## Сайт codingbat.com
При решении задач мы будем иногда использовать сайт [codingbat.com](http://codingbat.com). Зарегистрируйтесь на этом сайте.
После регистрации откройте ссылку `prefs` справа сверху, введите свое имя и внизу в раздел `Share To`
введите мой email: [iposov@gmail.com](mailto:iposov@gmail.com).
При любом использовании сайта проверяйте, что вы на нем залогинены.

Некоторые задачи могут быть проверены на сайте автоматически, у таких задачи после условия указана ссылка на автоматическую проверку.
Все автоматически проверяемые задачи собраны на одной странице: http://codingbat.com/home/iposov@gmail.com/1st-semester.


## Bat файл для запуска программ
Рекомендую использовать следующий bat файл для запуска программ. В нем настроены кодировки так, чтобы в ваших программах
корректно работал русский язык. Убедитесь только, что вы пишете свою программу в кодировке utf-8.

    del *.class
    "путь до javac.exe" -encoding utf8 YourClass.java
    "путь до java.exe" -Dfile.encoding=CP866 YourClass
    pause

Еще одна версия bat файла, в ней не нужно указывать имя класса. Файл `HelloWorld.bat` будет запускать класс `HelloWorld`,
а если его переименовать в `Abc.bat`, он будет запускать класс `Abc`.

    del *.class
    "путь до javac.exe" -encoding utf8 %~n0.java
    "путь до java.exe" -Dfile.encoding=CP866 %~n0
    pause

## Задания
### Типы и условные операторы
1. Дан год, определить, високосный или нет.
1. Дано целое число. Выведите после него существительное "кот" (или любое другое) в правильной форме: 1 кот, 2 кота, 5 котов
1. Даны коэффициенты квадратного уравнения, целые числа a, b, c: \\(ax^2+bx+c = 0\\) Решите квадратное уравнение.
Разберите все случаи, включая \\(a = 0\\). Ответ должен быть одним из следующих: «решений нет», «одно решение x = ...»,
«два решения x1 = ..., x2 = ...» или «решений бесконечно много».
1. Даны три целых числа a, b, с, выведите многочлен ax^2+bx+с. Получите результат в виде строки,
последней операцией в программе напечатайте эту строку на экран. http://codingbat.com/prob/p299433
    * Перерешайте эту задачу еще раз, напишите короткое и понятное решение с помощью двух функций.
    Функция `coef()` должна приписывать коэффициент к одночлену, например, `coef(2, "x") -> "2x"`,
    `coef(-1, "x^2") -> "-x^2"`,
    `coef(0, "x") -> "0"`,
    `coef(2, "") -> "2"`,
    `coef(-1, "") -> -1` (здесь вместо свободного члена x^0 пишем пустую строку "").
    А функция `sum()` должна складывать два слагаемых, например,
    `sum("x^2", "2x") -> "x^2+2x"`, `sum("x^2", "-2x") -> "x^2-2x"`, `sum("x", "-3") -> "x-3"`, `sum("x^2", "0") -> "x^2"`.
1. Дано целое число от 1 до 999, вывести его в виде текста. Убедитесь, что вы не выводите двух пробелов подряд,
и пробелов в конце текста. Получите результат в виде строки, последней операцией в программе напечатайте эту строку на экран.
http://codingbat.com/prob/p297521 (усложненный вариант http://codingbat.com/prob/p220544)