---
title: Добавление файла манифеста
---

Если вы проводили [аудит с помощью Lighthouse](/docs/audit-with-lighthouse/), то могли заметить невысокие баллы в категории "Progressive Web App". Давайте разберёмся, как можно увеличить этот показатель.

Но для начала выясним, чем _является_ PWA?

Это обычные сайты, использующие функциональность современных браузеров для расширения обычного поведения сайта специальными возможностями, подобно приложениям. Посмотрите [обзор от Google](https://developers.google.com/web/progressive-web-apps/), чтобы понять, что предоставляет PWA, и [документацию по прогрессивным веб-приложениям (PWA)](/docs/progressive-web-app/), чтобы узнать, почему сайт на Gatsby является PWA.

Подключение файла манифеста веб-приложения ― это одно из трех общепринятых [основных требований PWA](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1).

Цитируя [Google](https://developers.google.com/web/fundamentals/web-app-manifest/):

> Манифест веб-приложения ― это простой JSON-файл, который сообщает браузеру о веб-приложении и о том, как оно будет работать, когда "установится" на телефон или компьютер.

[Плагин манифеста Gatsby](/packages/gatsby-plugin-manifest/) позволяет Gatsby создавать `manifest.webmanifest` файл при каждой сборке сайта.

### Использование `gatsby-plugin-manifest`

1.  Установите плагин:

```shell
npm install --save gatsby-plugin-manifest
```

2. Добавьте фавиконку приложения в папку `src/images/icon.png`. Эта иконка требуется для сборки всех изображений в манифесте. Для более детальной информации ознакомьтесь с документацией в репозитории [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md).

3. Добавьте плагин в массив `plugins` в файле `gatsby-config.js`.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: "GatsbyJS",
        short_name: "GatsbyJS",
        start_url: "/",
        background_color: "#6b37bf",
        theme_color: "#6b37bf",
        // Включает отображение диалогового окна "Добавить на домашний экран" и выключает интерфейс браузера (вместе с кнопкой "назад")
        // см. https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: "standalone",
        icon: "src/images/icon.png", // путь относительно корня сайта.
        // Дополнительный атрибут, добавляющий проверку CORS.
        // Если вы не укажите опцию crossOrigin, настройки CORS не будут добавлены в манифест.
        // Любая некорректная или пустая строка по умолчанию преобразуется в `anonymous`
        crossOrigin: `use-credentials`,
      },
    },
  ]
}
```

Это всё, что нужно для добавления манифеста в Gatsby-сайт. Этот пример иллюстрирует базовую настройку плагина ― изучите [его описание](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode), чтобы узнать о расширенных настройках.
