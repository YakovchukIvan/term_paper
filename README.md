# Gulp - збірка

> використовується Gulp 4

19.01.2023
Курсова робота Яковчука Івана

## Початок роботи

Для роботи зі збіркою у новому проекті, клонуйте весь вміст репозиторію <br>
`git clone <this repo>`
Далі запустіть в терміналі команду `npm i`, яка встановить всі залежності, що знаходяться в package.json.
Після цього ви можете використовувати будь-яку із запропонованих команд збірки (підсумкові файли потрапляють до папки **app** кореневої директорії): <br>

`gulp` - базова команда, яка запускає складання для розробки, використовуючи browser-sync

`gulp build` - команда для продакшн-складання проекту. Усі ассети стиснуті та оптимізовані для викладення на хостинг.

`gulp cache` - команда, яку варто запускати після `gulp build`, якщо вам потрібно завантажити нові файли на хостинг без кешування.

`gulp backend` - команда для бекенд-складання проекту. Вона позбавлена ​​непотрібних речей з dev-складання, але не стиснута, для зручності бекендера.

`gulp zip` – команда збирає ваш готовий код у zip-архів.

## Структура тек і файлів

```
├── src/                          # Вихідні файли
│   ├── js                        # Скрипти
│   │   └── main.js               # Головний скрипт
│   │   ├── _vars.js              # файл зі змінними проекту
│   │   ├── _vendor.js            # файл із підключеннями бібліотек
│   │   ├── _functions.js         # файл із готовими функціями на js
│   │   ├── _components.js        # файл із підключеннями компонентів
│   │   ├── components            # js-компоненти
│   │   ├── vendor                # папка для загрузки локальных версий библиотек
│   ├── scss                      # Стилі сайту (препроцесор sass у scss-синтаксисі)
│   │   └── main.scss             # Головний файл стилів
│   │   └── vendor.scss           # Файл для підключення стилів бібліотек з папки vendor
│   │   └── _fonts.scss           # Файл для підключення шрифтів (можна використовувати міксин)
│   │   └── _mixins.scss          # Файл для підключення міксинів із папки mixins
│   │   └── _vars.scss            # Файл для написання css- або scss-змінних
│   │   └── _settings.scss        # Файл для написання глобальних стилів
│   │   ├── components            # scss-компоненти
│   │   ├── mixins                # папка для збереження готових scss-компонентів
│   │   ├── vendor                # папка для зберігання локальних css-стилів бібліотек
│   ├── partials                  # папка для зберігання html-частин сторінки
│   ├── img                       # папка для зберігання картинок
│   │   ├── svg                   # спеціальна папка для перетворення svg в спрайт
│   ├── resources                 # папка для зберігання інших ассетів - php, відеофайли, favicon і т.д.
│   │   ├── fonts                 # папка для зберігання шрифтів у форматі woff2
│   └── index.html                # Головний html-файл
└── gulpfile.js                   # файл с налаштуваннями Gulp
└── package.json                  # файл с налаштуваннями збірки та встановленими пакетами
└── .editorconfig                 # файл с налаштуваннями форматування коду
└── .ecrc                         # файл с налаштуваннями пакету editorconfig-checker (виключає непотрібні папки)
└── .stylelintrc                  # файл с налаштуваннями stylelint
└── README.md                     # документація збірки
```

## npm-скрипти

Ви можете викликати gulp-скрипти через npm.
Також у збірці є можливість перевіряти код на відповідність конфігу (editorconfig) та валідувати html.

`npm run html` - запускає валідатор html, запускати потрібно за наявності html-файлів у теці **app**.

`npm run code` - запускає editorconfig-checker для перевірки відповідності конфіг-файлу.

## Робота з html

Завдяки плагіну **gulp-file-include** можна розділяти html-файл на різні шаблони, які повинні зберігатися в теці **partials**. Зручно поділяти html-сторінку на секції.

> Для вставки html-частин у головний файл використовуйте `@include('partials/filename.html')`

Якщо ви хочете створити багатосторінковий сайт – копіюйте **index.html**, перейменовуйте як вам потрібно, та використовуйте.

При використанні команди `gulp build` ви отримаєте мінімізований html-код в один рядок для всіх html-файлів.

## Робота з CSS

У складання використовується препроцесор **sass** у синтаксисі **scss**.

Стилі, написані в **components**, слід підключати до **main.scss**.
**ВАЖЛИВО:** Обов'язково видалити стилі, які написані в **main.scss** для `.page__body`.

Щоб підключити сторонні css-файли (бібліотеки) - покладіть їх у теку **vendor** та підключіть у файлі **\_vendor.scss**

Якщо ви хочете створити свій міксин - робіть це в теці **mixins**, а потім підключайте файл **\_mixins.scss**.

Якщо ви хочете використовувати scss-змінні - підключіть **\_vars.scss** також до main.scss або в будь-яке інше місце, де він потрібен, але обов'язково видаліть **:root**.

> Для підключення файлів css використовуйте директиву `@import`

У підсумковій теці **app/css** створюються два файли: <br> **main.css** - для стилів сторінки, <br> **vendor.css** - для стилів усіх бібліотек, що використовуються в проекті.

При використанні команди `gulp build` ви отримаєте мінімізований css-код в один рядок для всіх css-файлів.

## Робота з JavaScript

Для складання JS-коду використовується webpack.

JS-код краще ділити на компоненти - невеликі js-файли, які містять свою ізольовану один від одного реалізацію. Такі файли розміщуйте в теці **components**, а потім імпортуйте у файл **\_components.js**

У файлі **vars.js** повинні зберігатися базові змінні проекту, на зразок знаходження елементів і т.д.

У файлі **main.js** нічого міняти не потрібно, він зроблений просто як результуючий.

Підключати сторонні бібліотеки можна через npm для цього існує файл **\_vendor.js**. Імпортуйте туди підключення.

Якщо якоїсь бібліотеки немає в npm або просто потрібно підключити щось локальним файлом - кладіть його в теку **vendor** і так само імпортуйте, але вже з шляхом до файлу.

При використанні команди `gulp build` ви отримаєте мінімізований js-код в один рядок для всіх js-файлів.

## Робота зі шрифтами

Ми не підтримуємо IE11, у збірці реалізована підтримка лише формату **woff2** (це означає, що у міксині підключення шрифтів використовується лише даний формат).

Завантажуйте файли **woff2** у теку **resources/fonts**, а потім викликайте міксин `@font-face` у файлі **\_fonts.scss**.

Також не забудьте прописати ці ж шрифти в <link preload> в html.

## Робота із зображеннями

Будь-які зображення, крім **favicon**, кладіть у теку **img**.

Якщо потрібно зробити svg-спрайт, кладіть потрібні для спрайту svg-файли в теку **img/svg**. При цьому такі атрибути як fill, stroke, style будуть автоматично видалятися. Інші svg-файли просто залишайте у теці **img**.

При використанні команди `gulp build` ви отримаєте мінімізовані зображення в підсумковій теці **img**.

У збірці доступна підтримка **webp** та **avif** форматів. Підключити їх можна через тег `picture`. Для background можна використовувати звичайні **jpg** або **png**, або використовувати image-set там, де це можливо.

## Робота з іншими ресурсами

Будь-які ресурси (ассети) проекту, під які не відведена відповідна тека, повинні зберігатися у теці **resources**. Це можуть бути відеофайли, php-файли (як, наприклад, файл відправки форми), favicon та інші.
