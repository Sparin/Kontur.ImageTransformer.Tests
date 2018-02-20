# Kontur.ImageTransformer.Tests
Реализация тестов на тестовое задание первой волны на стажировку в СКБ Контур 2018.

## Дисклеймер
Автор репозитория не является сотрудником СКБ Контур и **тесты не обязаны и могут не покрывать полный перечень критериев тестового задания**.
Используя данный проект, вы соглашаетесь с его лицензией и используете на свой страх и риск.

## Яндекс.Танк
Многие просили конфиг к Яндекс.Танку, который я использовал, проверяя свое решение. load.yaml и ammo.txt лежат в папке Yandex.Tank

Если вы хотите поделится [результатами](https://docs.google.com/spreadsheets/d/1STmc6E6h0-DJtSc9guihpTu9Z4hBeXkDLwhmFu6rqAk/edit#gid=0) вашего решения, 
отправьте скриншот с результатами и параметры из таблицы [автору](http://t.me/sparin) таблицы. 

_Конфигурация танка в репозитории полностью соотвествует требованиям таблицы_

## Как использовать
Перед использованием в папке с проектом, необходимо выполнить

```bat
nuget restore && msbuild /p:Configuration=Release
```
### Visual Studio
1. Открываем Kontur.ImageTransformer.sln
2. СTRL-E, T или TEST -> Windows -> Test Explorer
3. Если тесты не появились, то F6 или BUILD -> Build solution
4. Запускаем сервер
5. Run All в Test Explorer

### Console
1. Win + R или открывам командную строку
2. Переходим в папку с проектом, используя команду cd
3. Запускаем сервер
3. packages\xunit.runner.console.2.3.1\tools\net452\xunit.console.exe Kontur.ImageTransformer.Tests\bin\Release\Kontur.ImageTransformer.Tests.dll -diagnostics -html report.html
4. Открываем появившийся в папке с проектом report.html или смотрим вывод программы

### Настройки
Всего две очевидных и одна не очень.
#### Очевидные
В Settings.json в имеются параметры RequestTimeout и BaseAddress. 
* RequestTimeout - отвечает за время ожидания ответа от сервера в секундах. По умолчанию: 30
* BaseAddress - URL адрес до вашего сервера. По умолчанию http://localhost:8080
#### Неочевидные
Проверка применения фильтра на изображение происходит следующим образом. В Controllers/ImageProcessingController.cs есть две константы 
```C#
private const int CONTROL_WIDTH_PIXELS = 10;
private const int CONTROL_HEIGHT_PIXELS = 10;
```
Ширина и высота каждого контрольного изображения (от 100х100 до 1000х1000) делится на эти две константы получая шаг прибавки. 
То есть, гарантируется, что на изображении будет проверены каждый десятый пиксель по ширине и высоте (при стандартных значениях). 
Однако, **в тестах нет проверки на нулевой шаг**, когда количество контрольных пикселей больше, чем один из параметров изображения. 
Эту проверку придется дописать вам.
