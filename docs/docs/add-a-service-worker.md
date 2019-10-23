---
title: Добавление сервис-воркера
---

### Что такое сервис-воркер

Сервис-воркер ― это скрипт, исполняемый браузером в фоновом режиме, отдельно от основного кода, открывающий возможности для скриптов, которым не нужна веб-страница или взаимодействие с пользователем. Они повышают доступность вашего приложения в условиях нестабильного подключения и существенно помогают улучшить удобство работы с сайтом.

Он поддерживает возможности отправки уведомлений и фоновой синхронизации.

### Использование сервис-воркера в Gatsby с помощью `gatsby-plugin-offline`

У Gatsby есть прекрасный плагин для создания и загрузки сервис-воркера на ваш сайт: [gatsby-plugin-offline](https://www.npmjs.com/package/gatsby-plugin-offline).

Мы рекомендуем использовать этот плагин вместе с [плагином манифеста](https://www.npmjs.com/package/gatsby-plugin-manifest). (Не забудьте добавить плагин offline после плагина манифеста, чтобы файл манифеста был включен в сервис-воркер.)

### Установка `gatsby-plugin-offline`

`npm install --save gatsby-plugin-offline`

Добавьте этот плагин в `gatsby-config.js`

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-offline`]
```

## Ссылки

- [Сервис-воркеры: введение](https://developers.google.com/web/fundamentals/primers/service-workers/)
- [Сервис-воркеры: API](https://developer.mozilla.org/ru/docs/Web/API/Service_Worker_API)
