---
title: "Добавление пользовательского файла конфигурации Webpack"
---

_Перед созданием пользовательского файла конфигурации Webpack проверьте наличие
уже существующего Gatsby плагина, который решает вашу проблему в [разделе плагинов](/docs/plugins/).
Если таковой отсутствует, а ваш случай распространен, пожалуйста, добавьте ваш плагин в
библиотеку плагинов Gatsby. Так им смогут воспользоваться другие люди (включая вас самого в
будущем 😀)._

Чтобы добавить пользовательский файл конфигурации Webpack необходимо создать
(если уже не создан) `gatsby-node.js` файл в корневой директории. Внутри этого файла
сделайте экспорт функции с именем `onCreateWebpackConfig`.

Когда Gatsby будет генерировать собственный файл конфигурации Webpack, эта функция
будет вызвана и позволит вам изменить настройку Webpack по умолчанию с помощью
[webpack-merge](https://github.com/survivejs/webpack-merge).

Gatsby генерирует несколько сборок webpack с несколько отличной друг от друга
конфигурацией. Мы называет каждую из таких сборок "стадия". Существуют следующие стадии:

1.  develop: при запуске `gatsby develop` команды. Имеет настройку для hot reloading и CSS
    инъекций на страницу
2.  develop-html: то же самое, что и develop, но без react-hmre в конфигурации babel для рендеринга
    HTML компонента.
3.  build-javascript: JavaScript и CSS сборка для продакшен.
4.  build-html: продакшен сборка статических HTML страниц

Ознакомьтесь с исходным кодом в
[webpack.config.js](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby/src/utils/webpack.config.js).

There are many plugins in the Gatsby repo using this API to look to for examples
e.g. [Sass](/packages/gatsby-plugin-sass/),
[TypeScript](/packages/gatsby-plugin-typescript/),
[Glamor](/packages/gatsby-plugin-glamor/), and many more!

Примеры использования данного API можно найти в плагинах, находящихся в
Gatsby репозитории, таких как [Sass](/packages/gatsby-plugin-sass/),
[TypeScript](/packages/gatsby-plugin-typescript/),
[Glamor](/packages/gatsby-plugin-glamor/) и многих других.

## Примеры

Пример добавления дополнительной глобальной переменной с помощью `DefinePlugin` и `less-loader`:

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({
  stage,
  rules,
  loaders,
  plugins,
  actions
}) => {
  actions.setWebpackConfig({
    module: {
      rules: [
        {
          test: /\.less$/,
          use: [
            // We don't need to add the matching ExtractText plugin
            // because gatsby already includes it and makes sure its only
            // run at the appropriate stages, e.g. not in development
            loaders.miniCssExtract(),
            loaders.css({ importLoaders: 1 }),
            // the postcss loader comes with some nice defaults
            // including autoprefixer for our configured browsers
            loaders.postcss(),
            `less-loader`
          ]
        }
      ]
    },
    plugins: [
      plugins.define({
        __DEVELOPMENT__: stage === `develop` || stage === `develop-html`
      })
    ]
  });
};
```

### Absolute imports

Instead of writing `import Header from '../../components/header'` over and over again you can just write `import Header from 'components/header'` with absolute imports:

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({ stage, actions }) => {
  actions.setWebpackConfig({
    resolve: {
      modules: [path.resolve(__dirname, "src"), "node_modules"]
    }
  });
};
```

You can always find more information on _resolve_ and other options in the official [Webpack docs](https://webpack.js.org/concepts/).

### Modifying the babel loader

You need this if you want to do things like transpile parts of `node_modules`.

```js:title=gatsby-node.js
exports.onCreateWebpackConfig = ({ actions, loaders, getConfig }) => {
  const config = getConfig();

  config.module.rules = [
    // Omit the default rule where test === '\.jsx?$'
    ...config.module.rules.filter(
      rule => String(rule.test) !== String(/\.jsx?$/)
    ),

    // Recreate it with custom exclude filter
    {
      // Called without any arguments, `loaders.js` will return an
      // object like:
      // {
      //   options: undefined,
      //   loader: '/path/to/node_modules/gatsby/dist/utils/babel-loader.js',
      // }
      // Unless you're replacing Babel with a different transpiler, you probably
      // want this so that Gatsby will apply its required Babel
      // presets/plugins.  This will also merge in your configuration from
      // `babel.config.js`.
      ...loaders.js(),

      test: /\.jsx?$/,

      // Exclude all node_modules from transpilation, except for 'swiper' and 'dom7'
      exclude: modulePath =>
        /node_modules/.test(modulePath) &&
        !/node_modules\/(swiper|dom7)/.test(modulePath)
    }
  ];

  // This will completely replace the webpack config with the modified object.
  actions.replaceWebpackConfig(config);
};
```
