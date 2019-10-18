---
title: Добавление Service Worker
---

### Что такое service worker

Service worker - это скрипт, который Ваш браузер выполняет в фоне, отдельно от основного кода вкладки, что открывает возможности для скриптов, которым не нужна веб-страница или взаимодейтсвие с пользователем. Они улучшают стабильность Вашего приложения в условиях плохого подключения и существенно помогают улучшить пользовательский опыт.

Он поддерживает возможности отправки уведомлений и синхронизации в фоне.

### Использование service worker в Gatsby с `gatsby-plugin-offline`

Gatsby предоставляет прекрасный плагин для создания и добавления service worker на ваш сайт: [gatsby-plugin-offline](https://www.npmjs.com/package/gatsby-plugin-offline).

Мы рекомендуем использовать этот плагин вместе с [manifest плагином](https://www.npmjs.com/package/gatsby-plugin-manifest). (Не забудь указать offline плагин строго после manifest плагина, чтобы service worker можно было включить в манифест приложения).

### Установка `gatsby-plugin-offline`

`npm install --save gatsby-plugin-offline`

Добавьте этот плагин в `gatsby-config.js`

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-offline`]
```

## References

- [Service Workers: Введение](https://developers.google.com/web/fundamentals/primers/service-workers/)
- [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
