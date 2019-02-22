# Основы программирования, Java

## Задачи
[Начальные](tasks-intro.md)

## Bat файл для запуска программ
Рекомендую использовать следующий bat файл для запуска программ. В нем настроены кодировки так, чтобы в ваших программах
корректно работал русский язык. Убедитесь только, что вы пишете свою программу в кодировке utf-8.

    del *.class
    set PATH=путь до каталога установки java/bin;%PATH%
    javac.exe -encoding utf8 YourClass.java
    java.exe -Dfile.encoding=CP866 YourClass
    pause

Еще одна версия bat файла, в ней не нужно указывать имя класса. Файл `HelloWorld.bat` будет запускать класс `HelloWorld`,
а если его переименовать в `Abc.bat`, он будет запускать класс `Abc`.

    del *.class
    set PATH=путь до каталога установки java/bin;%PATH%
    javac.exe -encoding utf8 %~n0.java
    java.exe -Dfile.encoding=CP866 %~n0
    pause