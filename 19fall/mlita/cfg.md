# ИДЗ 2, LL(1)-грамматики

В ИДЗ 2 вы должны составить грамматику для языка, который получили в
условии, и написать программу, которая проверяет, принадлежит ли слово
языку.

Обратите внимание, что это **не** задание по программированию, и от вас
требуется только превратить вашу грамматику в код в соответствии с 
формальными правилами преобразования. Если полученная программа не
заработает, проверьте, правильно ли вы придумали
грамматику, и правильно ли преобразовали ее в программу. Не нужно
самостоятельно сочинять код, чтобы починить неработающую программу. 

## Условие задачи

Зададим язык: «арифметические выражения из переменных
"a", сложений и скобок». Примеры правильных слов:
  
    a
    a + a
    a + (a + a)
    (a + (a + a) + a)
  
примеры неправильных слов:
    
    a + 
    a + a)
    (a + a + (a)
    b
    ()
    
## Грамматика

Будем записывать нетерминальные символы в угловых
скобках:

    1) <expr> ::= <item><ending>

    2) <ending> ::=
    3) <ending> ::= + <expr>

    4) <item> ::= a
    5) <item> ::= ( <expr> )
    
## Пример вывода

Пример вывода слова `a + (a + a)`. Пробелы расставлены
для красоты, они не влияют на процесс. Приведен левый вывод, т.е. на
каждом шаге заменяется самый первый (левый) нетерминал:

    <expr> -> <item> <ending> -> a <ending> -> a + <expr> ->
    -> a + <item><ending> -> a + ( <expr> ) <ending> ->
    -> a + ( <item> <ending> ) <ending> -> a + ( a <ending> ) <ending> ->
    -> a + ( a + <expr> ) <ending> -> a + ( a + <item> <ending> ) <ending> ->
    -> a + (a + a <ending> ) <ending> -> a + (a + a) <ending> ->
    -> a + (a + a)
    
## Таблица `FIRST` и `FOLLOW`:

|Правило| `FIRST` | `FOLLOW`
|---------|:--------:|:-----:
| `<expr> ::= <item><ending>` | `a` `(` |   |
| `<ending> ::=` | \(\\Lambda\) | `$` `)` |
| `<ending> ::= + <expr>` | `+` |
| `<item> ::= a` | `a` |
| `<item> ::= ( <expr> )` | `(`

В таблице нас интересует множество `FOLLOW` только в том месте, где
`FIRST` содержит \(\\Lambda\).

Заметим, что разные строки для правил одного нетерминала содержат
разные символы. Это проверка того, что грамматика является LL(1)
грамматикой.

## Код программы
Ниже приведен пример программы на C++. Если будете писать
свою версию тоже на C++, можете оставить всю программу, заменив только
то, что написано между "НАЧАЛО" и "КОНЕЦ" функций разбора.

```c++
#include <iostream>
#include <string>

using namespace std;

//строка, введенная пользователем
string input;
//текущая позиция чтения символа
int pos;

//Возвращает текущий символ. Символ конца строки заменяется на $
char read() {
    char ch = input[pos];
    if (! ch)
        return '$';
    else
        return ch;
}

//Перемещает указатель чтения символа вперед
void next() {
    pos++;
}

//Сообщает об ошибке
void error() {
    throw exception();
}

//Читает терминальный символ, проверяет, что прочитала именно тот, который нужно
void read_terminal(char ch) {
    if (read() != ch)
        error();
    else
        next();
}

// НАЧАЛО ФУНКЦИЙ РАЗБОРА

// Грамматике соответствуют только следующие функции. Все остальное в этом файле нужно только, чтобы программу можно было запустить и проверить.

/*
Перечисление заголовков функций. Необходимо, чтобы можно было вызывать функцию до того, как
описан ее код. Без этого получается, что read_expr() вызывает read_item(), read_item() вызывает
read_expr() и невозможно выбрать, какую функцию описывать раньше.
PASCAL программисты тоже должны описывать функции заранее, для этого используется следующий синтаксис:
  procedure read_expr; forward;
Тело функции не описывается, оно будет описано позже.
*/
void read_expr();
void read_ending();
void read_item();

void read_expr() {
    switch (read()) {
        case 'a' :       //два case подряд означают, что прочитанный символ либо a, либо (
        case '(' :
            //правило 1
            //читаем <item>
            read_item();
            //читаем <ending>
            read_ending();
            break;
        default :       //во всех остальных случаях - error
            error();
    }
}

void read_ending() {
    switch (read()) {
        case '$':
        case ')':
            //правило 2
            break;
        case '+' :
            //правило 3
            //читаем символ +
            read_terminal('+');
            //читаем <expr>
            read_expr();
            break;
        default :
            error();
    }
}

void read_item() {
    switch (read()) {
        case 'a':
            //правило 4
            //читаем символ a
            read_terminal('a');
            break;
        case '(':
            //правило 5
            //читаем символ (
            read_terminal('(');
            //читаем выражение
            read_expr();
            //читаем символ )
            read_terminal(')');
            break;
        default:
            error();
    }
}

// КОНЕЦ ФУНКЦИЙ РАЗБОРА

int main() {
    cout << "Введите строку:\n";
    //читаем строку
    getline(cin, input);
    //устанавливаем текущий символ в начало строки
    pos = 0;

    try {
        //читаем выражение
        read_expr();

        //проверяем, что чтение дошло до конца
        if (read() != '$')
            error();

        //если не произошло ошибок, сообщаем, что строка корректна
        cout << "строка корректна";
    } catch (exception& ignored) {
        //если функцией error() было брошено исключение, сообщаем об ошибке и позиции.
        cout << "ошибка в позиции " << (pos + 1); // + 1, чтобы считать позиции с 1, а не с 0
    }
    
    cout << "\n";

    return 0;
}
```