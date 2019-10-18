---
title: Добавление файла manifest
---

Если Вы запускали [аудит с Lighthouse](/docs/audit-with-lighthouse/), Вы наверняка заметили невысокий рейтинг в категории "Progressive Web App". Давайте разберёмся, как можно увеличить этот рейтинг.

Но для начала разберёмся: чем _на самом деле_ является PWA?

PWA - обычные веб-сайты, которые используют возможности современных браузеров, чтобы расширить обычное поведение сайта функциями и выгодами, как в приложениях. Изучите [обзор от Google](https://developers.google.com/web/progressive-web-apps/), чтобы понять, какие выгоды даёт PWA и [документацию про Прогрессивные веб Приложения (PWAs)](/docs/progressive-web-app/), чтобы разобраться, как Gatsby реализует эти возможности.

Подключение файла веб-приложения manifest - это один из трех общепринятых [основных требований PWA](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1).

Цитируя [Google](https://developers.google.com/web/fundamentals/web-app-manifest/):

> Манифест веб-приложения -  это простой JSON файл, который рассказывает браузеру о Вашем веб-приложении и о том, как оно будет себя вести, когда оно "установится" на телефон или компьютер пользователя.

[Gatsby's manifest plugin](/packages/gatsby-plugin-manifest/) позволяет Gatsby создать `manifest.webmanifest` файл при каждой сборке сайта.

### Использование `gatsby-plugin-manifest`

1.  Установите плагин:

```shell
npm install --save gatsby-plugin-manifest
```

2. Добавьте фавиконку Вашего приложения в папку `src/images/icon.png`. Иконка требуется для сборки всех иконок в манифесте. Для более детальной информации ознакомьтесь с репозиторием [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md).

3. Добавьте `plugins` массив в Ваш файл `gatsby-config.js`.

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
        // Включает запрос "Добавить на домашний экран" и выключает интерфейс браузера (и кнопку "назад")
        // см. https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: "standalone",
        icon: "src/images/icon.png", // путь относительно корня сайта.
        // Дополнительные атрибут, добавляющий проверку CORS.
        // Если Вы не укажите опицию crossOrigin, настройки CORS не будут указаны в манифесте.
        // Любая некорректная или пустая строка по умолчанию преобразуется в `anonymous`
        crossOrigin: `use-credentials`,
      },
    },
  ]
}
```

Это всё, что вВам нужно для добавления манифеста в сайт Gatsby. Этот пример иллюстрирует базовую настройку плагина -- изучите [его описание](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode), чтобы узнать о других опциях.
