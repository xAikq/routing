# GeoSite / GeoIP и маршрутизация Happ

Собственные категории доменов и IP-диапазонов для Happ и Xray-core. В репозитории хранятся исходные списки и правила маршрутизации; готовые базы и профили доступны в [Releases](https://github.com/xAikq/routing/releases).

Списки пока минимальные и не охватывают все домены сервисов. **`geoip:ru` содержит только два адреса публичного DNS Яндекса, а не все российские IP.**

## Скачать

- [geosite.dat](https://github.com/xAikq/routing/releases/latest/download/geosite.dat)
- [geoip.dat](https://github.com/xAikq/routing/releases/latest/download/geoip.dat)
- [Готовый профиль Happ](https://github.com/xAikq/routing/releases/latest/download/happ-default.json)
- [Профиль для своих правил](https://github.com/xAikq/routing/releases/latest/download/happ-custom.json)
- [Ссылка импорта готового профиля](https://github.com/xAikq/routing/releases/latest/download/happ-default.txt)
- [Ссылка импорта профиля для своих правил](https://github.com/xAikq/routing/releases/latest/download/happ-custom.txt)
- [Страница с кнопками импорта](https://github.com/xAikq/routing/releases/latest/download/import.html)
- [Контрольные суммы SHA-256](https://github.com/xAikq/routing/releases/latest/download/SHA256SUMS)
- [Фактические категории и содержимое последней сборки](https://github.com/xAikq/routing/releases/latest/download/categories.json)

Ссылки заработают после первого стабильного релиза и сохранятся при обновлениях.

## Импорт в Happ

Скачайте `import.html` и откройте на устройстве с Happ: кнопки содержат готовые ссылки импорта. После включения GitHub Pages страница также доступна по адресу [xaikq.github.io/routing](https://xaikq.github.io/routing/).

Альтернативный способ: скопируйте содержимое `happ-default.txt` или `happ-custom.txt` и импортируйте ссылку из буфера в Happ. Это `happ://routing/add/{base64}`, содержащий JSON профиля, а не URL JSON-файла.

- **Готовый профиль:** правила берутся из [happ/profiles.json](happ/profiles.json).
- **Свои правила:** URL наших баз уже заданы, все массивы правил пустые. Добавьте нужные категории вручную.

Например:

```text
geosite:yandex → DIRECT
geosite:banks → DIRECT
geosite:youtube → PROXY
geoip:private → DIRECT
```

В обоих профилях остальной трафик идёт через прокси. Дождитесь скачивания баз, выберите профиль в нужной подписке и переподключите VPN. VPN-подключение настраивается отдельно; профили не содержат нод и ключей.

В JSON-подписках Happ ручной импорт и изменение маршрутизации ограничены: профиль должен поставляться вместе с подпиской провайдером. См. [официальную документацию Happ](https://github.com/HappDev/happ_su/blob/main/dev-docs/routing.md).

## Категории

| GeoSite | Содержимое |
| --- | --- |
| `yandex` | Минимальные домены Яндекса |
| `vk` | `vk.com` |
| `ozon` | `ozon.ru` |
| `wildberries` | `wildberries.ru` |
| `sber` | `sberbank.ru` |
| `banks` | Объединяет `sber` через include |
| `youtube` | Основные примеры доменов YouTube и Google Video |
| `riot` | `riotgames.com`; не полный список игровых серверов/CDN |
| `games` | Объединяет `riot` через include |

| GeoIP | Содержимое |
| --- | --- |
| `private` | Частные сети, loopback и link-local IPv4/IPv6; не весь зарезервированный адресный простор |
| `ru` | Только `77.88.8.8/32`, `77.88.8.1/32` — [публичный DNS Яндекса](https://dns.yandex.ru/) |

## Изменение списков

Редактируйте `.txt` в соответствующей папке. Один домен или CIDR на строку, комментарии начинаются с `#` отдельной строкой.

```text
domain:yandex.ru
full:passport.yandex.ru
regexp:^video[0-9]+\.yandex\.ru$
keyword:yandex
```

`domain:` охватывает домен и поддомены; `full:` задаёт точное имя, `regexp:` — выражение Go/RE2, `keyword:` — подстроку. Без префикса используется `domain:`. IDN указывайте в punycode. Атрибуты `@...` и комментарии после правил не поддерживаются. Пробел и `#` внутри regexp записывайте как `\x20` и `\x23`; в keyword эти символы запрещены.

Чтобы добавить категорию, создайте `geosite/my-service/domains.txt` с реальными доменами. Она станет `geosite:my-service`. Сейчас используется один уровень категорий; вложенные папки не поддерживаются. Имена папок: строчные ASCII-буквы, цифры, `_`, `-`.

Для агрегатора добавьте в его файл:

```text
include:sber
```

Добавление в агрегатор явное: новая папка сама не включается в `banks` или `games`. Циклы и неизвестные включения запрещены. Пустые категории не допускаются.

IP-списки находятся в `geoip/<категория>/*.txt`, IPv4 и IPv6 поддерживаются вместе. Используйте канонический CIDR, например `10.0.0.0/8`, а одиночный IP — с `/32` или `/128`. Ненулевые биты хоста и IPv4-mapped IPv6 отклоняются. `include:` также работает для IP.

## Правила готового профиля

В [happ/profiles.json](happ/profiles.json) назначайте категории действиям `DIRECT`, `PROXY`, `BLOCK`. Имена двух профилей задаются там же. Категории должны существовать; одну категорию нельзя направить в разные действия. Учитывайте пересечения агрегаторов и отдельных списков. Полный анализ пересечений произвольных regexp/keyword не выполняется.

Сами базы не содержат действий. Появление новой категории не добавляет её автоматически в готовую маршрутизацию.

## Обновления

Изменения исходников попадают в базы после выпуска нового стабильного релиза. Адреса `/releases/latest/download/...` остаются прежними. Обновление файлов на GitHub не гарантирует немедленную загрузку в Happ: клиент использует кеш и ограничения частоты обновлений.

При изменении правил повторно импортируйте свежую ссылку: старая Base64-ссылка содержит прежний снимок профиля. Импорт профиля с тем же именем обновляет его; собственные изменения сохраняйте отдельной копией. Поведение обновлений описано в [документации Happ](https://github.com/HappDev/happ_su/blob/main/dev-docs/routing.md).
