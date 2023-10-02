<p align=center><img src="https://alekslitvinenk.github.io/docker-openvpn/assets/img/logo-s.png"></p>
<br>
<br>

<p align="center">
<a href="https://github.com/alekslitvinenk/docker-openvpn/blob/master/README.md">[English]</a>
<a href="https://github.com/alekslitvinenk/docker-openvpn/blob/master/docs/README_RU.md">[Русский]</a>
<br>

![Build Version](http://cicd.dockovpn.io/version/dockovpn)
![Build Status](http://cicd.dockovpn.io/build/dockovpn)
[![Tests Status](http://cicd.dockovpn.io/tests/dockovpn)](https://cicd.dockovpn.io/reports/dockovpn)
![Build Time](http://cicd.dockovpn.io/built/dockovpn)
[![Docker Pulls](https://img.shields.io/docker/pulls/alekslitvinenk/openvpn.svg)](https://hub.docker.com/r/alekslitvinenk/openvpn/)
[![Gitter chat](https://img.shields.io/badge/chat-on_gitter-50b6bb.svg)](https://gitter.im/docker-openvpn/community)
![GitHub](https://img.shields.io/github/license/dockovpn/dockovpn)

**⚠️ Note:** Мы прикладываем большое количество усилий, чтобы поддерживать локализованную документацию в актуальном состоянии, однако это не всегда возможно ввиду ограниченности наших ресурсов. Поэтому мы приняли решение перейти к обновлению локализованных версий документации патчами. Мы приветствуем помощь волонтеров. В случае любых несоответствий, мы рекомендуем пользоваться <a href="https://github.com/alekslitvinenk/docker-openvpn/blob/master/README.md">[English]</a> версией документации.

# 🔐DockOvpn
Докеризированный VPN сервер, который работает прямо из коробки, не требует долгой настройки и постоянного места на жестком диске. Стартует за 2 секунды. Высокий уровень безопасности. Чтобы запустить, скопируйте и вставьте код ниже в окне терминала вашего сервера и следуйте инструкциям:
```bash
docker run -it --rm --cap-add=NET_ADMIN \
-p 1194:1194/udp -p 80:8080/tcp \
-e HOST_ADDR=$(curl -s https://api.ipify.org) \
--name dockovpn alekslitvinenk/openvpn
```
Чтобы получить более детальную информацию, перейдите к [Быстрый старт](#-быстрый-старт) или посмотрите [видео](https://youtu.be/A8zvrHsT9A0).

## Веб-сайт
https://dockovpn.io

## GitHub репозиторий:
https://github.com/dockovpn/dockovpn

## DockerHub репозиторий:
https://hub.docker.com/r/alekslitvinenk/openvpn

### Докер теги
| Тег    | Описание    | 
| :----: | :---------: |
| `latest` | Этот тег добавляется ко всем новым сборкам docker-openvpn: `v#.#.#` или `v#.#.#-regen-dh` |
| `v#.#.#` | Стандартная фиксированная релиз версия. Здесь {1} указывает на _мажорную версию_, {2} - _минорную_ и {3} - это a _патч_. Пример - `v1.1.0` |
| `v#.#.#-regen-dh` | Релизная версия с обновленным файлом Деффи Хеллмана. Чтобы поддерживать уровень безопасности контейнера на высоком уровне, новая сборка с постфиксом тега `regen-dh` генерируется каждый час. Пример - `v1.1.0-regen-dh` |
| `dev` | Рабочая сборка Dockovpn, которая содержит последние изменения из ветки где ведется активная разработка (master) |

### Переменные окружения
| Переменная | Описание | Значение по умолчанию |
| :------: | :---------: | :-----------: |
| NET_ADAPTER | Сетевой адаптер для использования на серверной машине | eth0 |
| HOST_ADDR | Адрес сервера, который будет использоваться в клиентском файле конфигурации | localhost |
| HOST_TUN_PORT | Порт на сервере для передачи  VPN данных | 1194 |
| HOST_CONF_PORT | HTTP порт на сервере для скачивания клиентского файла конфигурации | 80 |

**⚠️ Note:** В предоставленном фрагменте кода, мы используем конфигурацию, которая подойдет большинству пользователей. Мы не рекоммендуем менять NET_ADAPTER и HOST_ADDR на свои настройки если это абсолютно необходимо. HOST_ADDR определяется автоматически с помощью запуска подкомманды `$(curl -s https://api.ipify.org)`.
В отдельных случаях может потребоваться использовать порты отличные от тех, которые используются по умолчанию. Если это такой случай, используйте фрагмент кода ниже предварительно заменив `<custom port>` на нужные вам значения:

```shell
DOCKOVPN_CONFIG_PORT=<custom port>
DOCKOVPN_TUNNEL_PORT=<custom port>
docker run -it --rm --cap-add=NET_ADMIN \
-p $DOCKOVPN_TUNNEL_PORT:1194/udp -p $DOCKOVPN_CONFIG_PORT:8080/tcp \
-e HOST_ADDR=$(curl -s https://api.ipify.org) \
-e HOST_CONF_PORT="$DOCKOVPN_CONFIG_PORT" \
-e HOST_TUN_PORT="$DOCKOVPN_TUNNEL_PORT" \
--name dockovpn alekslitvinenk/openvpn
```

### Команды контейнера
После того как контейнер был запущен с помощью команды `docker run` становится возможным использование дополнительных команд контейнера через `docker exec`. Например, `docker exec <container id> ./version.sh`. Ниже приведен список всех поддерживаемых команд.

| Команда  | Описание | Параметры | Пример |
| :------: | :------: | :-------: | :----: |
| `./version.sh` | Выводит полную версию контейнера, например `Dockovpn v1.2.0` | | `docker exec dockovpn ./version.sh` |
| `./genclient.sh` | Генерирует новую конфигурацию для клиента | `z` — Необязательный. Помещает свежесгенерированный файл client.ovpn в zip архив.<br><br>`zp paswd` — Необязательный. Помещает свежесгенерированный файл client.ovpn в zip архив с защитой паролем `paswd` <br><br>`o` — Необязательный. Выводит клиентский файл конфигурации в консоль. <br><br>`oz` — Необязательный. Помещает свежесгенерированный файл client.ovpn в zip архив и выводит файл в консоль. Лучше всего использовать с переадресацией вывода в файл. <br><br>`ozp paswd` — Необязательный. Выводит zip архив с парольной защитой клиентского файла коифигурации в консоль. Лучше всего использовать с переадресацией вывода в файл. | `docker exec dockovpn ./genclient.sh`<br><br>`docker exec dockovpn ./genclient.sh z`<br><br>`docker exec dockovpn ./genclient.sh zp 123` <br><br>`docker exec dockovpn ./genclient.sh o > client.ovpn`<br><br>`docker exec dockovpn ./genclient.sh oz > client.zip` <br><br>`docker exec dockovpn ./genclient.sh ozp paswd > client.zip`|
 | `./rmclient.sh` | Удалаяет клиентский сертификат. Таким образом все последующие попытки пользователя установить соединение с данным сервером Dockovpn будут отвергаться. | Идентификатор клиента, пример `vFOoQ3Hngz4H790IpRo6JgKR6cMR3YAp`. | `docker exec dockovpn ./rmclient.sh vFOoQ3Hngz4H790IpRo6JgKR6cMR3YAp` |

## 📺 Видео руководство
<p align=center><a href="https://youtu.be/A8zvrHsT9A0"><img src="https://alekslitvinenk.github.io/docker-openvpn/assets/img/video-cover-play.png"></a></p><br>

## 🚀 Быстрый старт

### Предварительные реквизиты:
1. Сервер: физический или виртуальный. У вас должны быть права администратора на данной машине.
2. Докер.
3. Публичный IP адрес.

### 1. Запуск dockovpn
Скопируйте код ниже и вставьте его в консоли вашего сервера:<br>
```bash
docker run -it --rm --cap-add=NET_ADMIN \
-p 1194:1194/udp -p 80:8080/tcp \
-e HOST_ADDR=$(curl -s https://api.ipify.org) \
--name dockovpn alekslitvinenk/openvpn
```
**⚠️ Note:** Приведенный выше код запустит Dockovpn в присоединенном режиме и если вы закроете окно ssh сессии, то контейнер будет остановлен. Чтобы избежать этого, необходимо сначала отвязать контейнер от ssh сессии, для этого наберите `Ctrl+P Ctrl+Q`.

Если все предыдущие шаги были выполнены верно, то мы должны увидеть в консоли нечто похожее:
```
Sun Jun  9 08:56:11 2019 Initialization Sequence Completed
Sun Jun  9 08:56:12 2019 Client.ovpn file has been generated
Sun Jun  9 08:56:12 2019 Config server started, download your client.ovpn config at http://example.com/
Sun Jun  9 08:56:12 2019 NOTE: After you download you client config, http server will be shut down!
 ```

Сервис поднимет одноразовый http-сервер для того чтобы вы могли скачать файл с клиентскими настройками. После того как файл будет скачан, http-сервер будет остановлен.

### 2. Получите клиентский файл конфигурации
Теперь, когда сервер запущен, вы можете прейти на `<IP адрес вашего сервера>` в браузере и скачать клиентский файл конфигурации. Загрузка фала должна начаться немедленно.<br>
Как только вы загрузите файл, в консоли вашего сервера вы увидите сообщение о том что http сервер был остановлен.

```
Sun Jun  9 09:01:15 2019 Config http server has been shut down
```
Импортируйте `client.ovpn` в ваш любимый клиент OpenVPN. В большинстве случаев достаточно дважды кликнуть на файл, чтобы инициировать импорт настроек.

### 3. Подключитесь в вашему контейнеру docker-openvpn
Вы должны увидеть вашу новую конфигурацию в списке доступных конфигураций для подключения. Кликните ее и начнется установка соединения. Через пару секунд все будет готово.

Поздравляем, теперь вы можете безопасно путешествовать по Всемирной Сети!

## Сохранение конфигурации
Есть возможность сохранить сгенерированные файлы в volume. Запустите docker образ с:
```bash
-v openvpn_conf:/opt/Dockovpn_data
```

## Альтернативный способ. Запустить с помощью docker-compose
Иногда более удобно использовать [docker-compose](https://docs.docker.com/compose/).

Для того чтобы запустить docker-openvpn с помощью docker-compose, выполните:
```bash
echo HOST_ADDR=$(curl -s https://api.ipify.org) > .env && \
docker-compose up -d && \
docker-compose exec -d dockovpn wget -O /doc/Dockovpn/client.ovpn localhost:8080
```

После запуска данной команды вы сможете взять `client.ovpn` из `openvpn_conf` директории.

# Другие ресурсы
[Руководство контрибьютора (на английском)](https://github.com/alekslitvinenk/docker-openvpn/blob/master/CONTRIBUTING.md)<br>
[Кодекс поведения (на английском)](https://github.com/alekslitvinenk/docker-openvpn/blob/master/CODE_OF_CONDUCT.md)<br>
[Руководство по релизу (ENG)](https://github.com/alekslitvinenk/docker-openvpn/blob/master/docs/RELEASE_GUIDELINE.md)<br>
[Лицензионное соглашение (на английском)](https://github.com/alekslitvinenk/docker-openvpn/blob/master/LICENSE)
