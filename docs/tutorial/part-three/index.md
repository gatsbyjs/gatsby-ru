---
title: Создание вложенных компонентов разметки
typora-copy-images-to: ./
disableTableOfContents: true
---

Добро пожаловать в третью часть!

## Что вы узнаете в этом уроке?

В этой части, вы узнаете о Gatsby плагинах и о создании компонентов разметки.

Плагины Gatsby это JavaScript пакеты, которые помогают добавлять функциональность Gatsby сайту. Gatsby спроектирован быть расширяемым, это означает что плагины могут расширять и изменять практически всё что Gatsby делает.

Компоненты разметки предназначены для секций вашего сайта которые вы хотите разделить между несколькими страницами. Например, сайты обычно используют макет с общей шапкой и подвалом. Другой частый случай это добавление боковой панели и/или меню навигации. К примеру, на этой странице, шапка в верхней части сайта является частью компонента разметки gatsbyjs.org.

Давайте продолжим погружение.

## Использование плагинов

Вы возможно знакомы с идеей плагинов. Много программных систем поддерживают использование кастомных плагинов для добавления новой функциональности или даже изменения основной функциональности программного обеспечения. Gatsby плагины не исключение.

Участники сообщества (как вы!) могут принять участие в создании плагинов (представляющих собой небольшое количество JavaScript кода) которые могут быть использованы другими пользователями при создании Gatsby сайтов.

> У нас уже сотни плагинов! Откройте для себя [Библиотеку Плагинов](/plugins/) Gatsby.

Наша цель это сделать простым и понятным процесс установки и использования плагинов. Существует большая вероятность того что вы будете использовать их почти в каждом вашем Gatsby проекте. Далее в этом уроке у вас будет много возможностей попрактиковаться в их установке и использовании.

Во вступительной части урока, мы установим и реализуем Gatsby плагин для Typography.js.

[Typography.js](https://kyleamathews.github.io/typography.js/) это JavaScript библиотека, которая создает глобальные стили для типографики вашего сайта. У этой библиотеки есть [соответствующий Gatsby плагин](/packages/gatsby-plugin-typography/), который упрощает её использование в Gatsby сайте.

### ✋ Создание нового Gatsby сайта

Как уже отмечалось во [второй части](/tutorial/part-two/), на данном этапе будет неплохой идеей будет закрыть окно терминала и файлы проекта из предыдущей части этого руководства, чтобы начать всё с чистого листа. Затем, откройте новое окно терминала и выполните следующие команды по созданию нового сайта Gatsby в папке под названием `tutorial-part-three` и перейдите в неё:

```shell
gatsby new tutorial-part-three https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-three
```

### ✋ Установка и настройка `gatsby-plugin-typography`

Два главных этапа использования этого плагина это установка и настройка.

1. Установите `gatsby-plugin-typography` NPM пакет.

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> Примечание: Typography.js требует несколько дополнительных пакетов, поэтому они включены в команду установки. Такие дополнительные требования будут перечислены в команде установки каждого плагина.

2. Измените файл `gatsby-config.js` в корне вашего проекта следующим образом:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

`gatsby-config.js` это ещё один специальный файл, который автоматически распознается Gatsby. В нём обычно содержатся плагины и другие настройки сайта.

> Взгляните на [документацию по gatsby-config.js](/docs/gatsby-config/) чтобы узнать больше.

3. Typography.js требует файла настроек. Создайте новую папку `utils` внутри папки `src`. Затем создайте новый файл под названием `typography.js` в папке `utils` и скопируйте в него следующее содержимое:

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. Запустите сервер разработки.

```shell
gatsby develop
```

Как только сайт загрузится и если вы используете средства разработки Chrome для инпектирования сгенерированного HTML, то вы увидите что плагин типографики добавил `<style>` элемент со сгенерированным CSS в `<head>` элемент:

![typography-styles](typography-styles.png)

### ✋ Внесение изменений в содержимое и стили

Скопируйте следующее содержимое в ваш `src/pages/index.js` файл, чтобы увидеть эффект от применения CSS стилей сгенерированных библиотекой Typography.js.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

Ваш сайт должен выглядеть примерно так:

![no-layout](no-layout.png)

Сделаем небольшое улучшение. Многие сайты используют одну колонку с текстом в середине страницы. Чтобы сделать подобное, добавьте следующие стили в элемент `<div>` в файле `src/pages/index.js`.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </div>
)
```

![with-layout2](with-layout2.png)

Отлично. Вы установили и настроили ваш первый Gatsby плагин!

## Создание компонентов разметки

Перейдем к изучению компонентов разметки. Чтобы подготовиться к этой части, добавьте несколько новых страниц в ваш проект: about и contact.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>About me</h1>
    <p>I’m good enough, I’m smart enough, and gosh darn it, people like me!</p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>I'd love to talk! Email me at the address below</h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

Давайте посмотрим как выглядит страница:

![about-uncentered](about-uncentered.png)

Хмм. Было бы хорошо если бы содержимое двух новых страниц было отцентрировано так же как на стартовой странице. И было бы еще лучше использовать глобальную навигацию для того чтобы посетителям было легче находить и посещать каждую из подстраниц.

Вы решите эту проблему создав ваш первый компонент разметки.

### ✋ Создание вашего первого компонента разметки

1. Создайте новую папку `src/components`.

2. Создайте очень простой компонент в этой папке `src/components/layout.js`:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. Импортируйте вновь созданный компонент разметки в ваш `src/pages/index.js` компонент:

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Hi! I'm building a fake Gatsby site as part of a tutorial!</h1>
    <p>
      What do I like to do? Lots of course but definitely enjoy building
      websites.
    </p>
  </Layout> {/* highlight-line */}
)
```

![with-layout2](with-layout2.png)

Отлично, разметка работает! Содержимое вашей стартовой страницы отцентрировано.

Но попробуйте перейти на страницу `/about/`, или `/contact/`. Содержимое данных страниц все еще не отцентрировано.

4. Импортируйте компонент разметки в `about.js` и `contact.js` компоненты (так же как вы сделали это для `index.js` компонента ранее).

Содержимое всех трех страниц теперь отцентрировано благодаря одному компоненту разметки!

### ✋ Добавление заголовка сайта

1. Добавьте следующую строку кода в ваш новый компонент разметки:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MySweetSite</h3> {/* highlight-line */}
    {children}
  </div>
)
```

Если вы перейдете в любую из трех ваших страниц, вы увидите тот же заголовок добавленный ранее, к примеру, на странице `/about/`:

![with-title](with-title.png)

### ✋ Добавление ссылок для навигации между страницами

1. Скопируйте следующее содержимое в ваш компонент разметки:

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Home</ListLink>
        <ListLink to="/about/">About</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![with-navigation2](with-navigation.png)

И вот что мы имеем! Все три страницы сайта с простой глобальной навигацией.

_Задача:_ Используя ваши новые знания "компонентов разметки", попробуйте добавить шапки, подвалы, глобальную навигацию, боковые панели в ваши Gatsby сайты!

## Что будет дальше?

Перейдём в [четвёртую часть урока](/tutorial/part-four/) где вы начнёте изучать слой данных Gatsby и программно создавать Gatsby страницы!
