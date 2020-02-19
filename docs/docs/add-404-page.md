---
title: "Добавление страницы 404"
---

<<<<<<< HEAD
Для добавления страницы 404 создайте страницу, путь которой соответствует регулярному выражению
`^\/?404\/?$` (`/404/`, `/404`, `404/` или `404`). Как правило, React-компонент для такой страницы будет находиться по пути
`src/pages/404.js`.

Gatsby позаботится о том, чтобы 404 страница была доступна как `404.html`, поскольку большинство хостингов
статичных файлов по умолчанию использует это имя файла для обработки ошибки 404. Если в вашем случае
это не так, то потребуется дополнительно настроить свой хостинг на использование именно этого файла для 
страницы ошибки 404. 

Поскольку Gatsby по умолчанию создает эту страницу, 
вам не нужно дополнительно настраивать его в файле `gatsby-node.js`.

При разработке с помощью команды `gatsby develop`, Gatsby использует собственную страницу 404,
которая переопределяет вашу страницу 404. Тем не менее, вы всё равно можете посмотреть вашу страницу 404, 
нажав "Preview custom 404 page", чтобы убедиться, что она корректно отображается. Это удобно
во время разработки, потому что можно посмотреть все доступные страницы.
=======
To create a 404 page create a page whose path matches the regex `^\/?404\/?$` (`/404/`, `/404`, `404/` or `404`). Most often you'll want to create a React component page at `src/pages/404.js`.

Gatsby ensures that your 404 page is built as `404.html` as many static hosting platforms default to using this as your 404 error page. If you're hosting your site another way you'll need to set up a custom rule to serve this file for 404 errors.

Because Gatsby creates this page for you by default, there is no need to configure it in your `gatsby-node.js` file.

When developing using `gatsby develop`, Gatsby uses a default 404 page that overrides your custom 404 page. However, you can still preview your 404 page by clicking "Preview custom 404 page" to verify that it's working as expected. This is useful when you're developing so that you can see all the available pages.
>>>>>>> 90932a06db2e297cf416552b84e48b4b82e56fbc

The screenshot below shows the default 404 page that Gatsby creates. It also lists out all the pages on your website. Clicking the "Preview custom 404 page" button will allow you to view the 404 page you created.
![Gatsby Default 404 Page](./images/gatsby-default-404.png)

The screenshot below shows the custom 404 page.
![Gatsby Custom 404 Page](./images/gatsby-custom-404.png)
