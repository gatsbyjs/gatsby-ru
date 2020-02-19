---
title: "Быстрый старт"
---

Быстрый старт предназначен для средних и продвинутых разработчиков. Для более легкого начала работы с Gatsby, [начните с нашего руководства](/tutorial/)!

## Использование Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Быстрый старт с Gatsby: создавайте, разрабатывайте и собирайте Gatsby сайты из командной строки"
/>

### Установка Gatsby CLI

```shell
npm install -g gatsby-cli
```

> The above command installs Gatsby CLI globally on your machine.

### Создание нового сайта

```shell
gatsby new gatsby-site
```

### Изменение директории на папку с сайтом

```shell
cd gatsby-site
```

### Запуск сервера разработки

```shell
gatsby develop
```

Gatsby запустит рабочее окружение с горячей перезагрузкой доступное по умолчанию на `http://localhost:8000`.

Попробуйте изменить страницы с JavaScript в `src/pages`. Сохранённые изменения автоматически перезагружаются в браузере.

### Создание продакшен-сборки

```shell
gatsby build
```

Gatsby выполнит оптимизированную продакшен-сборку для вашего сайта, генерируя статические HTML и бандлы JavaScript для каждого маршрута.

### Запуск продакшен-сборки локально

```shell
gatsby serve
```

Gatsby запускает локальный HTML сервер для тестирования созданного вами сайта. Не забудьте создать свой сайт, используя `gatsby build`, перед использованием этой команды.

### Доступ к документации для CLI команд

Чтобы просмотреть подробную документацию по CLI командам, выполните `gatsby --help` в терминале.

Для определённых команд выполните `gatsby НАЗВАНИЕ_КОМАНДЫ --help`, например `gatsby new --help`.

Для получения дополнительной информации о Gatsby CLI посетите [раздел CLI](/docs/gatsby-cli/) в документации.
