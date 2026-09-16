# Как сделать свой onion-ресурс в сети Tor на Windows

Это гайд о том, как сделать свой onion-ресурс на Windows, чтобы больше людей заинтересовала эта тема.

## 📚 Содержание

- [🔧 Установка Tor Expert Bundle](#-установка-tor-expert-bundle-и-настройка)
- [🌐 Гайд чтобы Tor работал без мостов](https://github.com/niksus23/WARP-AmneziaWG-Guide)
- [🚨 Важно](#-Важно-ключи-сервиса)
- [🔧Приватный onion-ресурс через Client Authorization](#-Ваш-onion-ресурс-можно-сделать-приватным-через-`Client-Authorization`)
- [🧅 Примеры как работает Tor](#-Как-это-работает-пример-с-File-Browser)
- [⚙️ Параметры конфига](#-все-параметры-конфига-с-пояснениями)
- [⚠️ Дисклеймер](#-Дисклеймер)
- [💬 Обратная связь](#-Обратная-связь)

## 🔧 Установка Tor Expert Bundle и настройка

Ссылка: https://download.torproject.org/tor/ 

Выбираем `Windows (x86_64)` если у вас 32-х битная Windows то: `Windows (i686)`

1. Разархивируйте архив куда вам удобно

2. Для проверки запустите: `tor.exe` и дождитесь сообщения: `text[notice] Bootstrapped 100% (done): Done`

3. Откройте файл `torrc`: по пути `C:\Users\ваш_user\AppData\Roaming\tor\torrc`

если его нет то создайте текстовый файл `torrc` (без расширения)

<details>
<summary>🚫 Для тех у кого Tor не доступен</summary>

Туда Вставьте это:

```txt
UseBridges 1

ClientTransportPlugin obfs4,webtunnel exec Диск:\путь_к\lyrebird.exe

obfs4
  Bridge obfs4 ... ... mode=
  Bridge obfs4 ... ... mode=
```

и/или webtunnel:

```txt
WebTunnel
  Bridge  webtunnel [...] ... ver=
  Bridge  webtunnel [...] ... ver=
```
lyrebird.exe находится в папке `pluggable_transports`

мосты можно получить тут:

- Сайт: https://bridges.torproject.org/options/en

- TG bot: @GetBridgesBot

- Прямо из Tor Browser:

1. Откройте Tor Browser

2. Зайдите в `Settings → Connection`

3. В разделе `Мосты` нажмите `Request bridges` и пройдите капчу (Мосты добавятся автоматически)

4. Вставьте скопированные строки (каждую с новой строки) в torrc после `UseBridges 1`

Пример:
```config
ClientTransportPlugin obfs4,webtunnel exec C:\tor\pluggable_transports\lyrebird.exe
UseBridges 1
obfs4
  Bridge obfs4 109.202.219.164:47111 8EBD640CBC81B1AFB1E41921D376505C29E57A06 cert=nucr4/4B1W2UfTQK5bX/dqAKRcRD6UuEjvLNxlblk52owLbZNgSP0RTi869BdYPg5dzyLQ iat-mode=0
  Bridge obfs4 96.43.207.76:8877 6E2620AEDD466CC1542CBC8B85F45FE04AC126EC cert=O9j5SLu3FYxUGj0hvbC72FE5q0eXUOCmdhjwLr7lZvKTTQakPCgPjAVgoMK7y48g1T3yHw iat-mode=0

 WebTunnel
  Bridge  webtunnel [2001:db8:8d16:cb5b:d7f5:504b:8768:bdd3]:443 570FEB15763CC623146E8433ACA751948A6756D4 url=https://pl.808.re/SDglNGoixITem7ZHxQKXSscK ver=0.0.2
  Bridge  webtunnel [2001:db8:f3f8:1a33:dba0:17f6:35ce:24f3]:443 963668851C177DC162895A33F1473E32E1E4BE56 url=https://pod05.oneclickhost.eu/K5Fsvkz3SaWjVPm4i0vn5gIs ver=0.0.4
```
</details>

🌐 Гайд, чтобы Tor работал без мостов

Ссылка: https://github.com/niksus23/WARP-AmneziaWG-Guide

4. Если все работает: закрываем Tor и пишем базовые параметры для запуска скрытого сервиса
```config
DataDirectory Диск:\Ваш путь к папке
```
(куда вам удобно)

<details>
<summary>👁 Пример</summary>
DataDirectory C:\Tor
</details>

```config
HiddenServiceDir Диск:\Ваш путь к папке
```
(Директория сервиса) можно не создавать папку, Tor создаст её сам

<details>
<summary>👁 Пример</summary>
HiddenServiceDir C:\tor\service
</details>

```config
HiddenServicePort
```

<details>
<summary>👁 Пример</summary>
HiddenServicePort 80 127.0.0.1:9090
</details>

- Так выглядит минимальная конфигурация для onion-сервиса

1. С мостами
<details><summary>📎 Пример</summary>
<pre>
DataDirectory Диск :Ваш путь к папке
ClientTransportPlugin obfs4,webtunnel exec :Ваш путь к lyrebird.exe
UseBridges 1
obfs4
  Bridge obfs4 (мост)
  Bridge obfs4 (мост)
и/или
 WebTunnel
  Bridge  webtunnel (мост)
  Bridge  webtunnel (мост)
HiddenServiceDir :Ваш путь к папке
HiddenServicePort 80 127.0.0.1:Порт
</pre>
</details>
2. Без мостов
<details><summary>📎 Пример</summary>
<pre>
DataDirectory Диск :Ваш путь к папке
HiddenServiceDir :Ваш путь к папке
HiddenServicePort 80 127.0.0.1:Порт
</pre>
</details>

5. Запускаем `tor.exe` и проверяем

После этого в папке которую вы указали в `HiddenServiceDir` у вас будут файлы: `hostname` `public_key` `secret_key`

## 🚨 Важно: ключи сервиса

- `hostname` адрес вашего ресурса
- `public_key` публичная часть пары ключей
- `secret_key` главный секретный ключ

⚠️ `secret_key` Это главный секретный ключ вашего сервиса. Он используется для подтверждения того, что вы являетесь владельцем .onion-адреса, и для расшифровки входящих запросов. Этот файл критически важен для безопасности. Если он попадет в чужие руки, злоумышленник сможет выдать себя за ваш сервис. Именно этот ключ нужно беречь и делать его резервные копии.

## Ваш onion-ресурс можно сделать приватным через `Client Authorization`
<details><summary>🔧 Инструкция</summary>

1. Генерация ключей:

(в  PowerShell)

openssl genpkey -algorithm x25519 -out k.prv.pem

- Публичный ключ:

openssl pkey -in k.prv.pem -pubout | grep -v " PUBLIC KEY" | base64 -d | tail -c 32 | base32 | tr -d '=' > k.pub

- Приватный ключ:

cat k.prv.pem | grep -v " PRIVATE KEY" | base64 -d | tail -c 32 | base32 | tr -d '=' > k.prv

2. Настройка сервера

В HiddenServiceDir/authorized_clients/ создайте файл client1.auth:

- Если нет папки authorized_clients создайте

descriptor:x25519:СОДЕРЖИМОЕ_k.pub

Перезагрузите Tor:

С этого момента доступ только у тех, у кого есть ключ.

3. Клиент

Открывает ваш .onion в Tor Browser.

В появившемся окне вставляет содержимое k.prv.

⚠️ Важно

Чтобы отозвать доступ удалите .auth файл и перезагрузить Tor.
</details>

## 🧅 Как это работает: пример с File Browser

https://github.com/filebrowser/filebrowser

1. File Browser запускается на компьютере (например, на Windows) и начинает слушать локальный порт, скажем, 8080. Это значит, что если открыть в браузере http://127.0.0.1:8080, появится интерфейс File Browser.

2. Tor с настроенным скрытым сервисом получает запрос от посетителя, который переходит по адресу вида xxxxxxxx.onion. В файле конфигурации torrc есть строка:
```text
HiddenServicePort 80 127.0.0.1:8080
```
3. Это правило говорит Tor: «Всё, что приходит на 80-й порт этого onion-адреса, перенаправляй на локальный порт 8080»

4. Результат: посетитель открывает .onion-адрес в Tor Browser, Tor передаёт его запрос на локальный File Browser. Пользователь видит веб-интерфейс для загрузки, скачивания и управления файлами, но при этом соединение идёт внутри анонимной сети Tor .

Допустим, File Browser запущен в папке C:\Share и слушает порт 8080. В torrc указано:
```text
HiddenServiceDir C:\Users\<user>\AppData\Roaming\tor\hidden_service
HiddenServicePort 80 127.0.0.1:8080
```
После запуска Tor появится адрес .onion. Перейдя по нему в Tor Browser, можно увидеть интерфейс File Browser и управлять файлами из папки C:\Share — но доступ к этому интерфейсу будет только через сеть Tor.
<details>
<summary>👁  Если прям коротко</summary>

File Browser работает локально на порту 8080. Tor создаёт .onion-адрес и перенаправляет запросы с него на 127.0.0.1:8080. Посетитель открывает .onion в Tor Browser и видит интерфейс File Browser, но трафик идёт внутри сети Tor, а не напрямую.
</details>

<details>
<summary>👁  на примере сайта</summary>

Допустим, есть сайт myblog.onion.

Без Tor: сайт лежит на сервере с обычным IP, например 203.0.113.5. Любой может узнать этот IP и атаковать сервер.

С Tor:

На компьютере запущен веб-сервер, который отдаёт страницы блога. Он слушает 127.0.0.1:8080, то есть доступен только локально, извне его никто не видит.

В torrc прописано:
```text
    HiddenServiceDir C:\tor\hidden_service
    HiddenServicePort 80 127.0.0.1:8080
```
- Tor создаёт .onion-адрес и публикует его в сети Tor. Настоящий IP сервера нигде не фигурирует.

- Посетитель открывает myblog.onion в Tor Browser. Запрос идёт через цепочку узлов Tor, попадает на твой компьютер и перенаправляется на 127.0.0.1:8080.

- Веб-сервер отдаёт страницу. Посетитель видит блог, но не знает, где физически находится сервер. Сервер тоже не знает IP посетителя.

Итог: сайт доступен только через Tor, реальный IP скрыт с обеих сторон
</details>

## ⚙️ Все параметры конфига с пояснениями
- это список параметров которые я использую (и вам советую)

| Параметр | Значение | Описание |
|---|---|---|
| `DataDirectory` | путь | Папка, где Tor хранит состояние, ключи, кэш |
| `SocksPort` | 9050 | SOCKS5-прокси для приложений |
| `ControlPort` | 9051 | Порт управления Tor |
| `SafeSocks` | 1 | Блокирует соединения с самостоятельным DNS |
| `#UseBridges` | — | Выключатель мостов (закомментирован) |
| `BridgeRelay` | 0 | Не быть мостом-ретранслятором |
| `StrictNodes` | 1 | Жёстко соблюдать запреты стран |
| `#ExcludeNodes` | — | Запрет стран на любой позиции в цепи |
| `#ExcludeExitNodes` | — | Запрет стран только для выходных узлов |
| `CookieAuthentication` | 1 | Аутентификация на ControlPort через cookie |
| `ORPort` | 0 | Не принимать соединения от узлов сети |
| `ExitRelay` | 0 | Не быть выходным узлом |
| `EnforceDistinctSubnets` | 1 | Узлы цепи из разных подсетей /16 |
| `GeoIPFile` | путь | База геолокации IPv4 |
| `GeoIPv6File` | путь | База геолокации IPv6 |
| `NewCircuitPeriod` | 300 | Как часто строится новая цепь |
| `MaxCircuitDirtiness` | 600 | Максимальное время жизни цепи |
| `CircuitStreamTimeout` | 300 | Таймаут неактивного потока |
| `CircuitBuildTimeout` | 300 | Таймаут построения цепи |
| `KeepalivePeriod` | 300 | Период пустых ячеек, чтобы не рвался NAT |
| `#LearnCircuitBuildTimeout` | — | Автонастройка таймаута построения цепи |
| `LongLivedPorts` | список | Порты для более стабильных цепей |
| `HiddenServiceSingleHopMode` | 0 | Запрет однохопного режима |
| `HiddenServiceNonAnonymousMode` | 0 | Запрет неанонимного режима |
| `#ClientTransportPlugin` | — | Путь к плагину моста |
| `ClientUseIPv6` | 0 | Только IPv4 |
| `AddressDisableIPv6` | 1 | Отключить публикацию IPv6-адреса |
| `#Bridge obfs4` | — | Мост obfs4 |
| `#Bridge webtunnel` | — | Мост webtunnel |
| `HiddenServiceDir` | путь | Папка с ключами и hostname сервиса |
| `HiddenServicePort` | 80 127.0.0.1:9090 | Перенаправление портов |
| `HiddenServiceAllowUnknownPorts` | 0 | Запрет неописанных портов |
| `HiddenServiceVersion` | 3 | Версия onion-сервиса |
| `HiddenServiceNumIntroductionPoints` | 5 | Число точек ввода |
| `HiddenServiceMaxStreams` | 50 | Лимит одновременных потоков |
| `HiddenServiceMaxStreamsCloseCircuit` | 1 | Закрывать цепь при превышении лимита |
| `HiddenServiceEnableIntroDoSDefense` | 1 | Защита точек ввода от DoS |
| `HiddenServiceEnableIntroDoSRatePerSec` | 50 | Порог запросов в секунду |
| `HiddenServiceEnableIntroDoSBurstPerSec` | 200 | Допустимый всплеск запросов |
| `HiddenServicePoWDefensesEnabled` | 1 | Proof-of-Work защита |
| `HiddenServicePoWQueueRate` | 250 | Скорость приоритетной очереди |
| `HiddenServicePoWQueueBurst` | 2500 | Размер всплеска приоритетной очереди |

### 📄 Мой конфиг с пояснениями

<details>
<summary>⚙️ Конфиг</summary>
<pre>
#Директория Tor
DataDirectory C:\torchek\data\Tor            # Хранилище состояния, ключей, кэша. При полном сбросе — удалить

#настройка Tor
SocksPort 9050                               # SOCKS-прокси для клиентских приложений
ControlPort 9051                             # Порт управления. Команды: SIGNAL NEWNYM, GETINFO, и т.д.
SafeSocks 1                                  # Режет соединения, где приложение резолвило DNS само. Защита от утечки
#UseBridges 1                                # Глобальный тумблер мостов. Без этого мосты игнорируются
BridgeRelay 0                                # Явное указание: не работать как мост-ретранслятор
StrictNodes 1                                # Жёсткое соблюдение ExcludeNodes. Нет подходящих узлов — цепь не строится
#ExcludeNodes {ru},{cn},{ir},{kp},{sa},{tm},{er},{vn},{sg},{ae},{eg},{by}      # Запрет этих стран на любой позиции в цепи
#ExcludeExitNodes {ru},{cn},{ir},{kp},{sa},{tm},{er},{vn},{sg},{ae},{eg},{by}  # Запрет выходных узлов в этих странах
CookieAuthentication 1                       # Аутентификация на ControlPort через cookie-файл
ORPort 0                                     # Не принимать входящие соединения от других узлов
ExitRelay 0                                  # Запрет быть выходным релеем
EnforceDistinctSubnets 1                     # Узлы одной цепи из разных подсетей /16
GeoIPFile C:\torchek\data\geoip              # База геолокации IPv4
GeoIPv6File C:\torchek\data\geoip6           # База геолокации IPv6
NewCircuitPeriod 300                         # Интервал попыток построения новой цепи (сек)
MaxCircuitDirtiness 600                      # Максимальное время жизни цепи (сек)
CircuitStreamTimeout 300                     # Таймаут неактивного потока. Нужно для вебсокетов, SSH
CircuitBuildTimeout 300                      # Максимальное ожидание построения цепи
KeepalivePeriod 300                          # Период отправки пустых ячеек, чтобы не рвался NAT/файрвол
#LearnCircuitBuildTimeout 0                  # Закомментирован. По умолчанию Tor сам адаптирует CircuitBuildTimeout
LongLivedPorts 21,22,706,1863,5050,5190,5222,5223,6667,8300,8888,6767 # Порты, для которых Tor старается строить более стабильные цепи
HiddenServiceSingleHopMode 0                 # Запрет однохопного режима
HiddenServiceNonAnonymousMode 0              # Запрет неанонимного режима

#Мосты
#ClientTransportPlugin obfs4,webtunnel exec C:\torchek\tor\pluggable_transports\lyrebird.exe # Путь к исполняемому файлу подключаемых транспортов
ClientUseIPv6 0
AddressDisableIPv6 1

#obfs4
#Bridge obfs4 109.202.219.164:47111 8EBD640CBC81B1AFB1E41921D376505C29E57A06 cert=nucr4/4B1W2UfTQK5bX/dqAKRcRD6UuEjvLNxlblk52owLbZNgSP0RTi869BdYPg5dzyLQ iat-mode=0
#Bridge obfs4 96.43.207.76:8877 6E2620AEDD466CC1542CBC8B85F45FE04AC126EC cert=O9j5SLu3FYxUGj0hvbC72FE5q0eXUOCmdhjwLr7lZvKTTQakPCgPjAVgoMK7y48g1T3yHw iat-mode=0

#WebTunnel
#Bridge  webtunnel [2001:db8:8d16:cb5b:d7f5:504b:8768:bdd3]:443 570FEB15763CC623146E8433ACA751948A6756D4 url=https://pl.808.re/SDglNGoixITem7ZHxQKXSscK ver=0.0.2
#Bridge  webtunnel [2001:db8:f3f8:1a33:dba0:17f6:35ce:24f3]:443 963668851C177DC162895A33F1473E32E1E4BE56 url=https://pod05.oneclickhost.eu/K5Fsvkz3SaWjVPm4i0vn5gIs ver=0.0.4

#Скрытый сервис Tor
HiddenServiceDir C:\torchek\tor\ebali        # Директория с ключами и hostname сервиса
HiddenServicePort 80 127.0.0.1:9090          # Внешний порт 80 → Nginx на 9090 (не обязательно Nginx)
HiddenServiceAllowUnknownPorts 0             # Запрет на подключение к неописанным портам
HiddenServiceVersion 3                       # Версия скрытого сервиса (v3)
HiddenServiceNumIntroductionPoints 5         # Количество точек ввода
HiddenServiceMaxStreams 50                   # Максимальное количество одновременных потоков на сервис
HiddenServiceMaxStreamsCloseCircuit 1        # Закрывать цепь при превышении лимита потоков
HiddenServiceEnableIntroDoSDefense 1         # Защита точек ввода от DoS
HiddenServiceEnableIntroDoSRatePerSec 50     # Порог запросов в секунду к точке ввода
HiddenServiceEnableIntroDoSBurstPerSec 200   # Разрешённый всплеск запросов к точке ввода
HiddenServicePoWDefensesEnabled 1            # Включение Proof-of-Work защиты
HiddenServicePoWQueueRate 250                # Пропускная способность приоритетной очереди в секунду
HiddenServicePoWQueueBurst 2500              # Размер всплеска приоритетной очереди
</pre>

</details>

## 📚 Ресурсы для чтения
- https://www.torproject.org/ru/
- https://www.torproject.org/ru/about/history/
- https://community.torproject.org/ru/onion-services/
- https://onionservices.torproject.org/apps/web/onionspray/
- https://community.torproject.org/ru/onion-services/setup/

## ⚠️ Дисклеймер

Гайд носит образовательный характер. Автор не несёт
ответственности за использование информации в противоправных
целях. Соблюдайте законодательство своей страны.

## 💬 Обратная связь

Нашли ошибку или хотите дополнить гайд — Cоздайте Issue или Pull Request в этом репозитории.
