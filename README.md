<p align="center">
  <img src="assets/readme/cover.svg" alt="routing. — каталог маршрутизации для Happ" width="1200">
</p>

<p align="center">
  <strong>routing.</strong> · открытый каталог доменов и IP-сетей для Happ и совместимых клиентов.<br>
  Выберите готовый маршрут или составьте свой из тех же категорий.
</p>

<p align="center">
  <a href="https://xaikq.github.io/routing/install/"><img src="assets/readme/connect.svg" alt="Подключить Happ" width="246" height="50"></a>
</p>

<p align="center">
  <a href="#готовые-профили">🚀 Профили</a> · <a href="#карта-категорий">🧭 Категории</a> · <a href="#свой-json-профиль">🧩 Свой JSON</a> · <a href="#добавить-домен">✍️ Дополнить</a> · <a href="https://github.com/xAikq/routing/releases/latest">Релиз</a>
</p>

<a id="готовые-профили"></a>

## 🚀 Готовые профили

<p align="center">
  <a href="https://xaikq.github.io/routing/install/"><img src="assets/readme/default.svg" alt="DEFAULT — готовые правила" width="390"></a>
  <a href="https://xaikq.github.io/routing/"><img src="assets/readme/jsonsub.svg" alt="JSONSUB — основа для своих правил" width="390"></a>
</p>

| Профиль | Что делает | Посмотреть |
| --- | --- | --- |
| **DEFAULT** | `ads` и `tracking` → BLOCK; `ru` и `geoip:private` → DIRECT; остальное → PROXY | [JSON](happ/generated/DEFAULT.JSON) · [диплинк](happ/generated/DEFAULT.DEEPLINK) |
| **JSONSUB** | Подключает базы и DNS; списки DIRECT/PROXY/BLOCK пустые, по умолчанию действует PROXY | [JSON](happ/generated/JSONSUB.JSON) · [диплинк](happ/generated/JSONSUB.DEEPLINK) |

Диплинк — ссылка `happ://routing/add/…` с настройками профиля внутри. JSON показывает те же настройки в читаемом виде. Файлы выше находятся в Git; установка открывается кнопкой. Нужен Happ с настроенным VPN-подключением. После импорта дождитесь загрузки баз и переподключитесь.

**Базы отдельно от профиля:** [geosite.dat](https://github.com/xAikq/routing/releases/latest/download/geosite.dat) · [geoip.dat](https://github.com/xAikq/routing/releases/latest/download/geoip.dat) · [categories.json](https://github.com/xAikq/routing/releases/latest/download/categories.json) · [SHA256SUMS](https://github.com/xAikq/routing/releases/latest/download/SHA256SUMS). Они пригодятся, если профиль вы пишете сами. `categories.json` показывает уже раскрытые `include:` и фактические правила **опубликованного** релиза.

> [!IMPORTANT]
> Содержимое `main` и последнего релиза может различаться. Ссылки `/releases/latest/download/` ведут к последним **опубликованным** базам. Новые правила из Git начнут работать по этим адресам после следующего релиза. `geoip:ru` сейчас содержит только два IP DNS Яндекса, а не все российские сети.

<a id="карта-категорий"></a>

## 🧭 Карта категорий

Имя папки после `geosite/` — имя категории. `geosite:NAME` означает **весь список** из этой папки: все её `.txt` и рекурсивно включённые через `include:` категории. Например, `geosite:streaming-ru` содержит Okko, Rutube и VK Видео, а [geosite:ru](geosite/ru/) — все российские группы и зоны. [geosite:global](geosite/global/) объединяет зарубежные группы.

```text
geosite:ru                        geosite:global
├─ банки, сервисы, магазины        ├─ ИИ, разработка, экосистемы
├─ операторы, хостинги             ├─ игры, мессенджеры
├─ MAX, соцсети, видео             └─ зарубежные сервисы
├─ ru-zones (.ru/.рф/.su и др.)
└─ ru-other (нераспределённый остаток)

geosite:ads · geosite:tracking     отдельные списки для BLOCK
geoip:private · geoip:ru           IP-категории
```

<details open>
<summary><strong>🇷🇺 Российские сервисы · geosite:ru</strong></summary>

| Группа | Что внутри |
| --- | --- |
| [banks](geosite/banks/) | Все банки в **одной папке**; отдельного `geosite:sber` или `geosite:tbank` нет |
| [ecosystems-ru](geosite/ecosystems-ru/) | [Yandex](geosite/yandex/), [Mail.ru](geosite/mailru/) |
| [shopping-ru](geosite/shopping-ru/) | [Авто.ру](geosite/autoru/), [Авито](geosite/avito/), [Ozon](geosite/ozon/), [Wildberries](geosite/wildberries/) |
| [services-ru](geosite/services-ru/) | [2ГИС](geosite/2gis/), [СДЭК](geosite/cdek/), [HeadHunter](geosite/headhunter/) |
| [messengers-ru](geosite/messengers-ru/) | [MAX](geosite/max/) — сайт, веб-клиент и сервисные домены через `max.ru` и `oneme.ru` |
| [operators-ru](geosite/operators-ru/) | [билайн](geosite/beeline/), [МегаФон](geosite/megafon/), [МТС](geosite/mts/), [Ростелеком](geosite/rostelecom/), [T2](geosite/t2/) |
| [hosting-ru](geosite/hosting-ru/) | [Beget](geosite/beget/), [DSN-Host](geosite/dsn-host/), [NIC.RU](geosite/nic-ru/), [REG.RU](geosite/regru/), [Selectel](geosite/selectel/), [Timeweb](geosite/timeweb/) |
| [social-ru](geosite/social-ru/) | [Одноклассники](geosite/ok/), [VK](geosite/vk/) |
| [streaming-ru](geosite/streaming-ru/) | [Okko](geosite/okko/), [Rutube](geosite/rutube/), [VK Видео и VK Play Live](geosite/vkvideo/) |
| [ru-zones](geosite/ru-zones/) · [ru-other](geosite/ru-other/) | Российские доменные зоны и остаток импортированного RU-каталога; обычно выбирайте `geosite:ru` |

</details>

<details>
<summary><strong>🌍 Зарубежные сервисы · geosite:global</strong></summary>

| Группа | Что внутри |
| --- | --- |
| [ai-global](geosite/ai-global/) | [Anthropic/Claude](geosite/anthropic/), [Gemini](geosite/gemini/), [OpenAI](geosite/openai/), [Perplexity](geosite/perplexity/) |
| [dev-global](geosite/dev-global/) | [Docker](geosite/docker/), [GitHub](geosite/github/), [GitLab](geosite/gitlab/), [JetBrains](geosite/jetbrains/), [npm](geosite/npmjs/) |
| [ecosystems-global](geosite/ecosystems-global/) | [Apple](geosite/apple/), [Google](geosite/google/), [Microsoft](geosite/microsoft/) |
| [games-global](geosite/games-global/) | [Activision](geosite/activision/), [Blizzard](geosite/blizzard/), [EA](geosite/ea/), [Epic Games](geosite/epicgames/), [FACEIT](geosite/faceit/), [GOG](geosite/gog/), [Origin](geosite/origin/), [Riot](geosite/riot/), [Roblox](geosite/roblox/), [Rockstar](geosite/rockstar/), [Steam](geosite/steam/), [Ubisoft](geosite/ubisoft/), [Wargaming](geosite/wargaming/) |
| [messengers-global](geosite/messengers-global/) | [Discord](geosite/discord/), [Signal](geosite/signal/), [Telegram](geosite/telegram/), [Viber](geosite/viber/), [WhatsApp](geosite/whatsapp/), [Zoom](geosite/zoom/) |
| [social-global](geosite/social-global/) | [Facebook](geosite/facebook/), [Instagram](geosite/instagram/), [Reddit](geosite/reddit/), [X](geosite/x/) |
| [streaming-global](geosite/streaming-global/) | [Netflix](geosite/netflix/), [Spotify](geosite/spotify/), [TikTok](geosite/tiktok/), [Twitch](geosite/twitch/), [YouTube](geosite/youtube/) |

[geosite:games](geosite/games/) — совместимое имя для `geosite:games-global`.

</details>

<details>
<summary><strong>🛡️ Блокировка и IP-сети</strong></summary>

| Категория | Содержимое |
| --- | --- |
| [geosite:ads](geosite/ads/) | Рекламные домены |
| [geosite:tracking](geosite/tracking/) | Аналитика и телеметрия |
| [geoip:private](geoip/private/) | Локальные и частные IPv4/IPv6 сети |
| [geoip:ru](geoip/ru/) | Пока только `77.88.8.8/32` и `77.88.8.1/32`; **не используйте как базу всех IP РФ** |

</details>

Российская или зарубежная группа описывает принадлежность сервиса, а не географию каждого IP. Домены разных категорий могут пересекаться. В DEFAULT приоритет **BLOCK → DIRECT → PROXY**: реклама российского сервиса может блокироваться, а не идти напрямую.

<a id="свой-json-профиль"></a>

## 🧩 Свой JSON профиль

Возьмите [JSONSUB.JSON](happ/generated/JSONSUB.JSON) как основу. В его полях `Geositeurl` и `Geoipurl` уже указаны базы последнего релиза. В Happ списки заполняются именами категорий, а не путями к папкам:

```json
{
  "DirectSites": ["geosite:ru"],
  "DirectIp": ["geoip:private"],
  "ProxySites": ["geosite:telegram", "geosite:youtube"],
  "ProxyIp": [],
  "BlockSites": ["geosite:ads", "geosite:tracking"],
  "BlockIp": []
}
```

Это **фрагмент полей**, вставляемый в полный JSON профиль; остальные поля возьмите из JSONSUB. `geosite:max` берёт только MAX, `geosite:messengers-ru` — российские мессенджеры, `geosite:telegram` — только Telegram, `geosite:messengers-global` — шесть зарубежных мессенджеров. Если у вас `GlobalProxy: "true"`, отдельное правило `geosite:global` для обычного проксирования не требуется: трафик без совпадения уже идёт через PROXY. `geosite:ru` и `geosite:global` могут пересечься на отдельных доменах; при явных правилах проверьте порядок маршрутизации.

Если используете `.dat` в собственном Xray-конфиге, положите файлы в каталог ресурсов Xray и указывайте `geosite:NAME` / `geoip:NAME` в его правилах. Само имя категории не загружает файл из GitHub: клиенту нужны соответствующие `.dat`. Для Happ адреса загрузки задаются полями `Geositeurl` и `Geoipurl`. Для сервера Remnawave эти файлы нужно установить отдельно на стороне узла.

<a id="добавить-домен"></a>

## ✍️ Добавить домен

1. Найдите сервис в [карте категорий](#карта-категорий) и откройте его папку.
2. Добавьте правило в `custom.txt`. Файл `imported.txt` обновляется из внешнего источника и перезапишет ваши правки при следующем импорте.
3. Если создаёте новую категорию `geosite/new-service/`, включите её строкой `include:new-service` в нужную группу. Без `include:` она доступна как `geosite:new-service`, но не входит в `geosite:ru` или `geosite:global`.

```text
# Домен и все поддомены
domain:example.com
# Только точное имя
full:login.example.net
# Имена по шаблону
regexp:^cdn[0-9]+\.example\.org$
```

Комментарий пишите **отдельной строкой**: текст после правила не поддерживается. Все `.txt` одной папки объединяются; `include:` раскрывается при сборке. Имя папки пишите строчными латинскими буквами, цифрами, `_` или `-`; вложенные папки категорий не поддерживаются. Для IP-сетей используйте CIDR в `geoip/NAME/*.txt` и `geoip:NAME` в профиле.

Правила готового DEFAULT редактируются в [happ/profiles.json](happ/profiles.json). JSON и диплинки в `happ/generated/` — результат сборки; вручную их не правьте. Для собственного профиля редактируйте скачанный JSON, сохранив отдельное имя профиля: повторный импорт готового профиля с тем же именем обновит его настройки.

---

Каталог развивается. Пополнение доменов не заменяет проверку приложений и маршрутов на вашем устройстве. Лицензия — [MIT](LICENSE).
