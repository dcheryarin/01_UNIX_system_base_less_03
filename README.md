# Урок 3. Права и пользователи в UNIX

## 1. 
> * Создать файл file1 и наполнить его произвольным содержимым. 
> * Скопировать его в file2. 
> * Создать символическую ссылку file3 на file1. 
> * Создать жесткую ссылку file4 на file1. 
> * Посмотреть, какие айноды у файлов. 
> * Удалить file1. 
> * Что стало с остальными созданными файлами? 
> * Попробовать вывести их на экран.

echo строка1 > file1</br>
echo строка2 > file1</br>
echo строка3 > file1</br>
echo строка4 > file1

cp file1 file2

ln -s file1 file3   -символическая ссылка

ln file1 file4 - жесткая ссылка

390301 -rw-rw-r--  2 dmitry dmitry   70 апр 12 18:36 file1
390375 -rw-rw-r--  1 dmitry dmitry   70 апр 12 18:39 file2
390451 lrwxrwxrwx  1 dmitry dmitry    5 апр 12 18:40 file3 -> file1
390301 -rw-rw-r--  2 dmitry dmitry   70 апр 12 18:36 file4

rm file1

cat file*
при обращении к file3 - ошибка, т.к. отсутствует файл, на который ссылается симлинк

cat: file3: Нет такого файла или каталога

## 2. 
> * Дать созданным файлам другие, произвольные имена. 
> * Создать новую символическую ссылку. 
> * Переместить ссылки в другую директорию.

mv file2 new_file2</br>
mv file3 new_file3</br>
mv file4 new_file4</br>

ln -s new_file2 new_file5

mv new_file3 new_file5 lesson_03/</br>
после перемещения новая символическая ссылка также перестает работать ( очевидно, в связи с тем, что она ссылается на файл-оригинал в текущей директории ).

## 3. 
> * Создать два произвольных файла. 
> * Первому присвоить права на чтение, запись для владельца и группы, только на чтение — для всех. 
> * Второму присвоить права на чтение, запись — только для владельца. 
> * Сделать это в численном и символьном виде.

touch exercise3_1</br>
touch exercise3_2

chmod 664 exercise3_1</br>
chmod 600 exercise3_2

chmod ug=rw,o=r exercise3_1</br>
chmod u+rw,go-rwx exercise3_2

## 4. Создать пользователя, обладающего возможностью выполнять действия от имени суперпользователя.
sudo adduser test</br>
4.1 sudo visudo</br>
4.2 test localhost=ALL

## 5. 
> * Создать группу developer и несколько пользователей, входящих в нее. 
> * Создать директорию для совместной работы. 
> * Сделать так, чтобы созданные одними пользователями файлы могли изменять другие пользователи этой группы.

sudo groupadd developer</br>
sudo useradd -m -G developer -s /bin/bash dev_1</br>
sudo useradd -m -G developer -s /bin/bash dev_2

mkdir developer

sudo chgrp developer ~/lesson_03/developer/</br>
chmod 775 ~/lesson_03/developer/</br>
sudo chmod g+s ~/lesson_03/developer/

dmitry@geekbrains:~/lesson_03$ ls -la
drwxrwsr-x 3 dmitry developer 4096 апр 12 19:47 developer

## 6. Создать в директории для совместной работы поддиректорию для обмена файлами, но чтобы удалять файлы могли только их создатели.

mkdir ~/lesson_03/developer/exchange/</br>
chmod 1777 ~/lesson_03/developer/exchange/

## 7. 
> * Создать директорию, в которой есть несколько файлов. 
> * Сделать так, чтобы открыть файлы можно было, только зная имя файла, а через ls список файлов посмотреть было нельзя.

mkdir exercise3_7</br>
touch ./exercise3_7/file1</br>
touch ./exercise3_7/file2</br>
touch ./exercise3_7/file3

chmod 331 ./exercise3_7/ </br>
 Если у директории нет права на чтение, то нельзя посмотреть ее оглавление.

ls -la ./exercise3_7/</br>
ls: невозможно открыть каталог './exercise3_7/': Отказано в доступе

cat ./exercise3_7/file1</br>
Содержимое файла file1
