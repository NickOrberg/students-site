# Редактор генераторов многовариантных заданий

Многовариантное задание - это задание, которое имеет несколько вариантов условия. Есть
много способов описать набор условий, в этом проекте предлагается дать
возможность запрограммировать генератор, чтобы он при запуске создавал
случайные условия.

Программировать генератор можно на любом языке. Хорошо подойдут JavaScript и Python как
популярные скриптовые языки.

Результатом работы генератора должны быть тексты. Причем, это должны быть тексты с
форматированием и, желательно, формулами. Здесь тоже есть выбор языка для
форматированного текста. Варианты: HTML, LaTeX, Markdown.

Еще один выбор - это язык шаблонизатора, он позволяет удобно подставлять генерируемые
значения в текст, например: "у Тани было $x яблок". Здесь тоже много вариантов.
Например, jinja шаблоны "у Тани было \{\{ x \}\} яблок" или twirl шаблоны "У Тани было
@x яблок". Еще шаблоны могут быть самодельные.

Для задач по математике нужны системы компьютерной алгебры, в них уже запрограммированы
алгоритмы, часто требующиеся при генерации заданий по математике. Например, это
раскрытие скобок, упрощение выражений, вывод выражений. Нужно выбрать систему
компьютерной алгебры. Но с этим пока повременим.

В этом проекте предлагается остановиться на вариантах
 1. Код генератора пишется на JavaScript
 2. Язык вывода результата Markdown+LaTeX для формул
 3. Самодельный шаблонизатор
 

Этот вариант будет наиболее удобен для того, чтобы запускать генерацию в браузере без
использования сервера.

## Интерфейс

Базовый интерфейс - это окно, разделенное пополам. Слева пишется код и находится
кнопка запуска. Справа отображаются несколько вариантов условий и ответов.

Пример кода

```
statement = "Чему равна сумма @x и @y?"
answer = "@x + @y = @z"

x = random(1, 10)
y = random(1, 10)
z = x + y
```

В этом коде используются специализированные функции, помогающие писать генератор,
например, функция random() генерации случайного числа на отрезке. Такие функции
неявно определяются перед запуском генератора.
И считается, что после выполнения кода нужно взять значения переменных
`statement` и `answer` и подставить в них значения переменных, оставшиеся после
выполнения скрипта.

Вообще, для переменных statement и answer можно ввести отдельные поля ввода.
Чтобы не сне смешивать их с кодом. Только тогда нужно заранее предусмотреть, что таких
полей может быть много, и пользователь может вводить разные куски текста.

## Технологии

Библиотека пользовательского интерфейса Vue.js. Хотя она не очень нужна из-за
простоты интерфейса - несколько полей ввода и кнопка запуска.
Серверная часть - отсутствует. Все технологии выбраны так, что программе не нужен
сервер, она все может сама с помощью Браузера.

Понадобятся еще библиотеки для превращения Markdown в html, понадобится MathJax для
отображения формул.

Сложный вопрос с песочницей - как запускать код безопасно, т.е. чтобы он не завис 
и не использовал всю память. Пока избежим этот вопрос.