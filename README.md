# <span style="color:red">THIS BRANCH IS NOT FUNCTIONAL; WAIT UNTIL MERGED</span>

<p align="center">
  <img src="https://slonik.0chan.ru/system/media_attachments/files/109/679/620/696/823/059/small/9a129bef20a8733d.png" alt="Instant 0chan"/>
</p>

# Instant 0chan

Набор для быстрого развертывания нульчана на движке, использовавшемся на Nullch.org (форк Kusaba X).
## Функции
* Пользовательские доски (2.0чан) с максимальной свободой действий:
  * Полноценная модерация
  * Кастомные стили для досок
  * Раздельные спамлисты для каждой из досок
  * Автозамена
  * Баннеры
* ~~Мгновенное обновление тредов~~ Временно не работает
* Исправлены баги Kusaba X
* Добавлены собственные баги
* Главная страница на HTML5 с возможностью кастомизации
* ~~Полная~~ совместимость с куклоскриптом
* Тач-интерфейс (с превью по клику)
* Подсветка кода
* LaTEX
* Эмбеды Youtube, Vimeo и Coub, не нагружающие страницу
* Баннеры
* Кириллическая протухающая капча с генератором произносимых слов
* Улучшенный парсинг
* Фильтры флуда и спама, детектирующие замену букв
* Дайсы, useragent, катриболы
* Множество улучшений в клиентском и серверном коде
* Полный набор нульчановских досок в комплекте
* Поддержка неограниченного количества вложений в постах

### Выпилено
* Watched threads (через эту фичу можно было досить Kusaba X)
* Типы досок "Upload", "Оэкаки" и "Рисовач". **Полностью и безвозвратно.**

## Требования

### Для базового функционала

* Apache
  * [mod_cloudflare](https://www.cloudflare.com/resources-downloads)  (для корректной работы с Cloudflare и кантриболлов). **Без MC баны из-под Cloudflare будут работать некорректно!** (как это было на zerochan.in). [Установка mod_cloudflare на CPanel.](http://tltech.com/info/installing-mod_cloudflare-on-cpanel/) 
* PHP (Поддерживается в том числе PHP7 и скорее всего PHP 7.1)
* MySQL


Особых требований к версиям нет, должно встать на среднестатичтический виртуальный хостинг. Читайте ошибки, если что-то не заработает. Теоретически возможна работа с базами SQLite и PostgreSQL, но это не тестировалось, и дампы базы данных для них еще нужно написать. Вместо Apache можно использовать Nginx.

### Для расширенного функционала

* PHP 5.3+ (для 2.0 и мгновенных обновлений)
* Node.JS (для мгновенных обновлений)
* Cloudflare или GeoIP (для кантриболлов)
* FFMpeg (для WEBM)

## Установка

### Конфигурация

#### `config.php`

Файл `config.php` в корневой папке содержит основную конфигурацию, которую нужно выполнить в первую очередь.
Конфигурация же при этом выполняется в `instance-config.php`. Для редактирования настроек из `config.php` необходимо скопировать туда нужные строчки.
 
* **KU_DBTYPE** `(=mysqli)`: тип базы данных 
* **KU_DBHOST**, **KU_DBDATABASE**, **KU_DBUSERNAME**, **KU_DBPASSWORD**: данные для доступа к БД.
* **KU_NAME**: название сайта.
* **KU_SLOGAN**: слоган сайта.
* **KU_WEBFOLDER**: подпапка кусабы на вашем сайте, если она лежит не в корне. (Например, для http://example.com/board/b KU_WEBFOLDER = `"/board/"`).
* **KU_LIVEUPD_ENA** `(true | false)`: включает/выключает мгновенные обновления. Смотри "Настройка мгновенных обновлений" ниже.
* **KU_CSSVER**, **KU_CSSVER** `(любая строка)`: версии CSS и JS, отдаваемые клиенту. При внесении изменений в CSS или JS, рекомендуется инкрементировать версию и пересобрать все файлы.
* **KU_CAPTCHALIFE** `(число)`: время жизни капчи в секундах.
* **KU_20_BOARDSLIMIT** `(число)`: сколько досок может иметь пользователь 2.0-ча.
* **KU_20_CLOUDTIME** `(strtotime-совместимая строка со знаком минус)`: период времени, за который подсчитываются посты на 2.0 досках при сортировке.
* **KU_USERAGENT_ENABLED**: список досок, на которых работает `##useragent##`

#### Дополнительно

* При необходимости добавьте содержимое `UTIL/nginx.snippets.conf` в ваш файл конфигурации хоста nginx.

### Процесс установки

* Убедитесь, что база данных, указанная в конфигурации, создана.
* Убедитесь, что файл `instance-config.php.example` переименован в `instance-config.php` и отредактирован должным образом.
* Скопируйте `install.php`, `install-mysql.php` и `kusaba_freshinstall.mysql.php` из папки `UTIL` в корневую папку.
* Запустите `install.php`. После проверки доступности базы данных вам предложат импортировать данные в базу.
* После дампа базы данных введите логин и пароль администратора.
* Удалите файлы, скопированные из папки `UTIL`.

#### Треки с главной страницы

Треки не включены в репозиторий; скачать их можно [отсюда](https://dl.dropboxusercontent.com/u/43313258/loops.zip). Скопируйте их в `/pages/loops/`. 

### Установка мгновенных обновлений

**Данный функционал давно не обновлялся и скорее всего работать не будет. Это ненадолго.**

* Скопируйте содержимое `util/updates` в папку, в которой собираетесь запускать Node.js.
* Находясь в этой папке, выполните "`npm install`".
* Введите имя сайта (`sitename`) и соответствующую ему случайную строку (`srvtoken`). Присвойте это же значение `$cf['KU_LIVEUPD_SITENAME']`, `$cf['KU_LIVEUPD_SRVTOKEN']` в `config.php`.
* Задайте порт и IP (`node_port`, `node_ip`), на которых будет работать процесс `node`. Внесите эти значения в `config.php` в форме URL (`KU_LOCAL_LIVEUPD_API` и `KU_CLI_LIVEUPD_API`). Если сервис обновлений будет работать на субдомене, но вместо IP в качестве `KU_CLI_LIVEUPD_API` введите этот субдомен.
* Запустите процесс `node updates.js`.
  * Для стабильной работы `node.js` в продакшене нужно воспользоваться одним из способов по демонизации и мониторингу этого процесса. Наилучшим способом является `systemd`.
* Активируйте фичу (в `config.php`: `KU_LIVEUPD_ENA = true`).
* Пересоберите все файлы и папки.

### Кастомизация

* Логотипы сайта и 2.0-ча располагаются в папке `/images/`.
* Для редактирования паттерна на главной странице выполните команду `editmode();` из консоли браузера на главной странице. Нарисуйте нужный паттерн и нажмите "Получить паттерн". Скопируйте паттерн и размеры паттерна в `/pages/patterns/main.php` (или другой файл).
* Данные для доната располагаются в `/pages/contents/donate.php`. По умолчанию там указаны [кошельки ЕФГ](http://web.archive.org/web/20121028010659/http://0chan.ru/?donate).
* Чтобы добавить собственные лупы на главную, их нужно скопировать в `/pages/loops/`, после чего внести в список `loops` в `/pages/index.js`. Файлы должны быть в формате OGG.

## Управление сайтом

### manage.php
* Derp

### Управление версиями и кэшем

* Возможно обновление сайта с помощью `git`, если в движок не вносились никакие изменения. Просто запустите `git pull`.
* При обновении движка на новую версию бывает необходимо обновить БД. Смотрите лог коммитов и соответствующие SQL-файлы в папке `UTIL`.
* При изменении CSS или JS файлов рекомендуется инкрементировать `KU_CSSVER` и `KU_JSVER` в файле `config.php` после чего пересобирать все файлы.
* При изменении других файлов, если вы используете Cloudflare, необходимо [вручную удалить файл из кэша](http://blog.cloudflare.com/introducing-single-file-purge). Если вы не уверены, что пользователям отдается свежая версия ресурсов или если что-то пошло не так, очищайте кэш Cloudflare полностью.

## TODO

* Дописать README (лол)
* Удостовериться в правильности работы мгновенных обновлений
* Починить перенос тредов
* Починить или выпилить KU_FIRSTLAST
