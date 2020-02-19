---
title: "Добавление пользовательской конфигурации webpack"
---

_Перед созданием пользовательского файла конфигурации Webpack поищите уже готовый Gatsby-плагин, который решает вашу проблему, в [разделе плагинов](/docs/plugins/). Если ничего найти не удалось, а ваш случай распространен, пожалуйста, добавьте ваш плагин в Библиотеку Плагинов Gatsby, чтобы другие люди смогли им воспользоваться (включая вас самого в будущем 😀)._

Чтобы добавить пользовательскую конфигурацию Webpack, создайте (если уже не создан) `gatsby-node.js` файл в корневой директории. Внутри этого файла экспортируйте функцию с именем `onCreateWebpackConfig`.

При создании собственной webpack-конфигурации Gatsby вызовет эту функцию, позволяя вам изменить настройку webpack по умолчанию с помощью [webpack-merge](https://github.com/survivejs/webpack-merge).

Gatsby генерирует ряд webpack-сборок с несколько отличной друг от друга конфигурацией. Каждую из таких сборок мы называем "стадия". Существуют следующие стадии:

1.  develop: при запуске `gatsby develop` команды. Включает настройку для горячей перезагрузки и добавления CSS на страницу.
2.  develop-html: то же самое, что и develop, но без react-hmre в конфигурации babel для рендеринга HTML-компонента.
3.  build-javascript: продакшен-сборка JavaScript и CSS. Создает как маршрут для JS-бандлов, так и чанки с общим кодом для JS и CSS.
4.  build-html: продакшен-сборка статических HTML-страниц.

Ознакомьтесь с [webpack.config.js](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby/src/utils/webpack.config.js) в качестве источника.

Примеры использования этого API можно найти в плагинах из репозитория Gatsby, например, [Sass](/packages/gatsby-plugin-sass/), [TypeScript](/packages/gatsby-plugin-typescript/), [Glamor](/packages/gatsby-plugin-glamor/) и многих других!

## Примеры

Пример добавления дополнительной глобальной переменной с помощью `DefinePlugin` и `less-loader`:

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({
  stage,
  rules,
  loaders,
  plugins,
  actions,
}) => {
  actions.setWebpackConfig({
    module: {
      rules: [
        {
          test: /\.less$/,
          use: [
            // Добавлять ExtractText плагин не надо, потому что
            // gatsby уже содержит его и следит за тем, чтобы запускался
            // он только там, где нужно, к примеру, не в develop
            loaders.miniCssExtract(),
            loaders.css({ importLoaders: 1 }),
            // в загрузчике postcss уже есть подходящие настройки по умолчанию,
            // включая autoprefixer для указанных нами браузеров
            loaders.postcss(),
            `less-loader`,
          ],
        },
      ],
    },
    plugins: [
      plugins.define({
        __DEVELOPMENT__: stage === `develop` || stage === `develop-html`,
      }),
    ],
  })
}
```

### Абсолютные импорты

Вместо написания `import Header from '../../components/header'` снова и снова, вы можете сократить запись до `import Header from 'components/header'` с абсолютными импортами:

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({ stage, actions }) => {
  actions.setWebpackConfig({
    resolve: {
      modules: [path.resolve(__dirname, "src"), "node_modules"],
    },
  })
}
```

Более подробная информация о _resolve_ и других опциях в официальной [документации Webpack](https://webpack.js.org/concepts/).

### Изменение загрузчика babel

Это понадобится вам, если захотите сделать что-то наподобие транспилинга отдельных частей `node_modules`.

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({ actions, loaders, getConfig }) => {
  const config = getConfig()

  config.module.rules = [
    // Опустим правило по умолчанию, где test === '\.jsx?$'
    ...config.module.rules.filter(
      rule => String(rule.test) !== String(/\.jsx?$/)
    ),

    // Создадим его заново со своим исключающим фильтром
    {
      // Вызванный без аргументов `loaders.js` вернет
      // объект вида:
      // {
      //   options: undefined,
      //   loader: '/path/to/node_modules/gatsby/dist/utils/babel-loader.js',
      // }
      // Если вы не собираетесь менять Babel на другой транспилер, вам, вероятно,
      // понадобится это для того, чтобы Gatsby применил необходимые пресеты/плагины
      // Babel. Также будет добавлена ваша конфигурация из
      // `babel.config.js`.
      ...loaders.js(),

      test: /\.jsx?$/,

      // Исключаем все node_modules из транспиляции, кроме 'swiper' и 'dom7'
      exclude: modulePath =>
        /node_modules/.test(modulePath) &&
        !/node_modules\/(swiper|dom7)/.test(modulePath),
    },
  ]

  // Это полностью заменит конфигурацию webpack на измененный объект.
  actions.replaceWebpackConfig(config)
}
```
