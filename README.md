# walking-robo

[![Greenkeeper badge](https://badges.greenkeeper.io/urbica/walking-robo.svg)](https://greenkeeper.io/)

##Как всё устроено
1. Работает всё от пользователя robosm
 * папка с данными /home/robosm/data (абсолютный путь - значит пусть на сервере)
 * на базы должны быть owner права
2. этот репозиторий ставиться в папку /home/robosm/data/walking-robo
3. Скрипт импрорта
 * запускается каждую ночь
 * выполняет все таски, которые заданы в файле import/testc.js
4. Таск - загрузка первичного файла, обрезание osmosis'ом, заливка в базу
 * результат выполнения тасков находится в папке /home/robosm/data/build
 * название таска значимо
 * с таким же именем создаётся подпапка в /home/robosm/data/build
 * заливка идёт в базу с таким-же названием
5. Заливка в базу
 * стиль заливки (набор колонок) определяется стилем по умолчанию + пользовательскими настройками
 * пользовательские настройки хранятся в файле /home/robosm/data/userStyles.json
6. Веб интерфейс запускается через forever web/server-http на порту 8080, веб отображает содержимое билд директории
 
##Как настроить пользовательские поля
1. открыть файл /home/robosm/data/userStyles.json
2. по ключу addFields в строчке через пробел указать нужные поля
3. по ключу removeFields в строчке через пробел указать поля, которые не нужно завиливать в sql

##Как запустить скрипт вручную
1. Зайти под robosm
2. cd ~/data/walking-robo
3. iojs import/import [--task taskName --step download|osmosis|sql]    
```
iojs import/import
```   
запустится выполнение всех тасков  
```
iojs import/import --task azore
```   
запустится выполнение только таска azore   
```
iojs import/import --task gis --step sql
```   
запустится только заливка в базу из уже скаченного и обрезанного файла

##Как запустить веб сервер
1. Зайти под robosm
2. cd ~/data/walking-robo
3. forever start web/server-http.js    

##Дополнительно
1. Базы должны быть созданны заранее с включённым PostGIS'ом:
```
CREATE DATABASE azore;
\connect azore
CREATE EXTENSION postgis;
```
