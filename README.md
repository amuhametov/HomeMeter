![main](https://cloud.githubusercontent.com/assets/147685/16461469/69b19d82-3e35-11e6-8ff6-68b285bcc05e.jpg)

# HomeMeter
Шуточный Node.js проект для Raspberry Pi, способный на полном серьёзе регистрировать показания домашних счётчиков воды и загружать собираемые значения на портал государственных онлайн услуг (pgu.mos.ru).

## Возможности
* Настраиваемая конфигурация подключения счётчиков к GPIO-портам Raspberry PI
* Встроенный крохотный веб-сервер для отображения и управления показаниями
* Модуль регистрации показаний счётчиков учёта воды на портале http://pgu.mos.ru
* Журналирование

## Установка
Для установки проекта необходимо выполнить следующие шаги (подразумевается, что у вас установлен Node.js):
* Если на вашем Raspberry Pi не установлен Node.js, установите его (https://nodejs.org/)

* Клонируйте репозиторий  
    _git clone git@github.com:juks/HomeMeter.git_

* Установите дополнительные модули  
    _cd HomeMeter_  
    _npm install_

  Готово!
  
## Пример подключения счётчика
Рядовой счётчик воды с импульсным выходом можно подключить к контактам GPIO следующим образом:
![scheme](https://cloud.githubusercontent.com/assets/147685/16464262/081680d0-3e42-11e6-9a63-66933f000032.png)

Используются штырёк выхода 3.3 В и один из GPIO-интерфесов.

Принцип работы импульсного выхода большинства домашних счётчиков воды заключается в замыкании входного и выходного контактов средствами внутреннего геркона. Временное замыкание осуществляется при прохождении внутренним вращающимся механизмом установленной отметки объёма (в большинстве случаев — 10 дм<sup>3</sup> или, другими словами, каждые 10 литров, прошедшие через счётчик, временно замыкают, затем размыкают линию).
  
## Запуск
Для ознакомление с описанием доступных параметров, выполните следующую команду в директории проекта:

    $ node homeMeter.js --help

**Важно!** Перед запуском необхоимо создать файл настройки подключения счётчиков _wiring.js_ (_cp wiring.js.dist wiring.js_)

Чтобы запустить приложение и веб-сервер на порту 3000, выполните следующую команду в строке консоли:

    $ sudo node homeMeter.js --localServerPort=3000
    
Для работы с единым файлом конфигурации можно использовать параметр _--с_:

    $ sudo node homeMeter.js --c=config.json

![console](https://cloud.githubusercontent.com/assets/147685/16465198/282f3ea8-3e46-11e6-86c8-86bbb7439d54.png)

**Важно!** По-умолчанию в системе Raspbian доступ к GPIO-портам доступен только для суперпользователя, поэтому запуск приложения необходимо осуществлять через команду sudo.

После запуска web-инерфейс приложения будет доступен средствами браузера на порту 3000, по адресу, присвоенному устройству.

![web](https://cloud.githubusercontent.com/assets/147685/16462344/80a180d0-3e39-11e6-9301-f0a8ed1470c1.png)

## Загрузка показаний в pgu.mos.ru
Так как публичные API для загрузки показаний счётчиков автору неизвестны, загрузка осуществляется самим приложением через авторизацию на сайте pgu.mos.ru и последующую передачу параметров к известным (на момент разработки) URL.

Для работы функционала загрузки показаний необходимо задать настроку _subPguMos_ в параметрах конфиругции, указав параметры действительной учётной записи проекта pgu.mos.ru, идентификатор плательщика и номер квартиры например:
```json
"subPguMos":    {
                    "username": "ivanov",
                    "password": "ivanovRocks",
                    "payerId": 1234567890,
                    "flatNumber": 100
                  }
```

## Freeware
Данное приложение является свободно распространяемым без ограничений на оизменение его исходного кода. Автор не несёт ответственности за возможные последствия использования приложения и работоспособность оборудования.
