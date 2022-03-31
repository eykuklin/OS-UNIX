<img src="img/head.png" width="100%">

# Операционная система UNIX
### Домашние задания по курсу

## Предисловие

Готовые задачи принимаются в виде ссылок на репозиторий на GitHub.

Во всех программах разбор параметров делать через библиотеку getopt. Сборка программы должна осуществляться командой make. Для запуска и тестирования должен использоваться скрипт с именем runme.sh, который при необходимости может вызывать вспомогательные скрипты.

Каждый системный вызов должен проверяться на корректность выполнения. В случае критических ошибок программа должна завершаться с информативным сообщением.

Отчет о работе тестов должен сохраняться в файл result.txt и должен содержать краткое описание тестов с описанием ожидаемого результата и вывод фактических результатов тестов.

Язык программирования: C.

## Задача 1

Тема: работа с файлами - open, read, write, seek.

Sparse file — файл, в котором последовательности нулевых байтов заменены на информацию о них. Последовательность нулевых байт внутри файла (дыры) не записывается на диск, а информация о них (смещение от начала файла в байтах и количество байт) хранится в метаданных ФС. Сжатие архиваторами типа gzip происходит очень эффективно. При распаковке всё наоборот: gzip не заботится о создании дырок и забивает их нулями с риском выйти за пределы файловой системы.

Требуется написать программу создания sparse файлов. Программа считает нули и заменяет блоки, заполненные нулями, на seek для создания разреженного файла. Поскольку запись в файл идёт поблочно, то и входные данные надо обрабатывать поблочно (бессмысленно записать 1 байт, потом сделать seek на 315 байт и записать ещё один байт - все уйдет в один блок). По умолчанию сделать размер блока 4096 байт, но отдельный параметр должен задавать размер блока в байтах.

Если на вход программе подаётся один аргумент — имя файла, то читается stdin и пишется в указанный файл. Если два аргумента, то читается первый файл и пишется в последний.

Также написать вспомогательный скрипт, создающий тестовый файл A, длиной 4\*1024\*1024 + 1 байт, заполненный в основном нулями. По смещениям 0, 10000 и в конце файла должны быть записаны единицы.

Далее в runme.sh прописываются следующие действия:

1. Скопировать созданный файл A через нашу программу в файл B, сделав его разреженным:
```markdown
$ ./myprogram fileA fileB
```
2. Сжать A и B с помощью gzip.
3. Распаковать сжатый файл B в stdout и сохранить через программу в файл C:
```markdown
$ gzip -cd fileB.gz | ./myprogram fileC
```
4. Скопировать A через программу в файл D, указав нестандартный размер блока - 100 байт.
5. Программой stat вывести реальный размер файлов A, A.gz, B, B.gz, C, D

Ключевые функции: read(), write(), lseek(output, count, SEEK_CUR), ftruncate(output, size).


## Задача 2

You can use the [editor on GitHub](https://github.com/eykuklin/OS-UNIX/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.



### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/eykuklin/OS-UNIX/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
