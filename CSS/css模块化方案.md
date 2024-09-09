##### CSS 模块化方案

###### 什么是 CSS 模块化

> CSS 本身没有模块化和作用域的概念，根据选择器名称去全局匹配元素，在页面的两个不同地方使用了一个相同的类名，先定义的样式就会被覆盖掉，所以在定义选择器名称时，总会顾及其他文件中是否有使用相同的命名。于是，CSS 社区诞生了相应的模块化解决方案：BEM，OOCSS，SMACSS，ITCSS 以及 CSS Modules 和 CSS-IN-JS 等

`CSS模块化是一种将样式文件分割为独立，可复用，具有局部作用域的模块的开发方式。`

###### 为什么要使用 CSS 模块化

`避免全局命名冲突`：传统 CSS 是全局作用域，所有类名和样式都是共享的，这可能导致不同组件之间的样式冲突。而 CSS 模块化使样式局部化，确保各组件的样式互不影响，避免全局命名冲突。

`提高代码的可维护行`：模块化的样式组件方式更加清晰，每个组件或功能模块都有自己独立的样式文件，修改时不必担心影响其他部分，提高了维护效率

`增强可复用性`：通过模块化，组件样式可以更容易的复用。样式与组件逻辑结合，使得组件可以被打包，移植到其他项目中使用，而无需担心样式冲突和依赖问题

###### 如何使用 CSS 模块化

`根据模块化方案的特点，将方分为三大类:`

[![pAZGzKf.webp](https://s21.ax1x.com/2024/09/05/pAZGzKf.webp)](https://imgse.com/i/pAZGzKf)

###### **CSS 命名方法论**

`通过人工的方式来约定命名规则`

- **BEM**

`通过组件名唯一性来保证选择器的唯一性，从而保证样式不会污染到组件外`

<span style="color: red">命名规范：**.block-name_elememt-name--modifier-name**</span>

**BEM 把样式按照 B E M 三种来进行命名：**block\_\_elem--mod

> Block（块）：代表一个独立的功能模块，header、menu、container
>
> Element（元素）：块的一部分但是自身没有独立的含义，不能在块的外部使用，header title(是什么元素)
>
> Modifier（修饰符）(是什么状态)：块或者元素的一些状态或者属性标志,不必须，可以选择使用，small、checked

```html
<article class="article">
  <h2 class="article__title"></h2>
  <p class="article__content"></p>
</article>

<button class="btn btn--disabled"></button>
<form class="search-form">
  <!-- `search-form` 块中的 `input` 元素 -->
  <input class = "search-form__input />
</form>
<ul class="list">
  <li class="list__item">learn html</li>
  <li class="list__item list__item--underline">learn css</li>
  <li class="list__item">learn js</li>
</ul>
```

- **OOCSS**

  `结构和样式分开：将元素结构和元素外观分开，增强css的复用性`

  `容器和内容分开：最好不要直接使用标签名定义样式，减少对html的依赖`

  结构是不可见的视觉属性

  - Height
  - Width
  - Position
  - Margin
  - Padding
  - Overflow

  样式是可见的视觉属性

  - Color
  - Fonts
  - Shadows
  - Gradients
  - BackgroundColors

  ```css
  <div class="size1of4 bgBlue solidGray mt-5 ml-10 mr-10 mb-10"></div>

  <style>
    .size1of4 { width: 25%; }
    .bgBlue { background: blue; }
    .solidGray { border: 1px solid #ccc; }
    .mt-5 { margin-top: 5px; }
    .mr-10 { margin-right: 10px }
    .mb-10 { margin-bottom: 10px; }
    .ml-10 { margin-left: 10px; }
  </style>
  ```

- **SMACSS**

  `编写模块化，结构化和可扩展的CSS`

  > SMACSS 的核心是分类，具体把项目分为了五类：
  >
  > 1. Base(基础)
  > 2. Layout(布局)
  > 3. Module(模块)
  > 4. State(状态)
  > 5. Theme(主题)

  **Base 基础**

  `一般指默认样式，默认样式基本都是元素选择器，也可以包含属性选择器，伪类选择器，孩子选择器，兄弟选择器。`<span style="color: red">本质上来说，一个基础样式定义了元素在页面的任何位置应该是怎么样的</span>

  ```css
  html {
    margin: 0;
    font-family: sans-serif;
  }
  a {
    color: #000;
  }
  button {
    color: #ababab;
    border: 1px solid #f2f2f2;
  }
  ```

  **Layout 布局和 Module 模块**

  [![pAZaP9H.png](https://s21.ax1x.com/2024/09/05/pAZaP9H.png)](https://imgse.com/i/pAZaP9H)

  1. 基础规则是直接作用于元素，不需要前缀
  2. 布局的前缀是`l-或者layout-`。例如`.l-table,.layout-grid`
  3. 模块的前缀是`m-`或模块自身的命名,例如`.m-nav,.card,.field`
  4. 状态的前缀是`is-`,例如`.is-active,.is-current`
  5. 主题的前缀是`theme-`例如`.theme-light,.theme-dark`

  ```css
  <form class="layout-grid">
    <div class="field">
      <input type="search" id="searchbox" />
      <span class="msg is-error">There is an error!</span>
    </div>
  </form>
  ```

  **state 状态**

  `定义了模块或者布局在特殊的状态下应该呈现怎样的效果，是hidden还是expanded，是active还是inactive，类名通常使用.is- 来开头`

  ```css
  .is-collapsed {
  }
  .is-expanded {
  }
  .is-active {
  }
  .is-highlighted {
  }
  .is-hidden {
  }
  .is-shown {
  }
  .is-successful {
  }
  ```

  **Theme 主题**

  `Theme是定义公共类名的地方`

  [![pAZaJbV.png](https://s21.ax1x.com/2024/09/05/pAZaJbV.png)](https://imgse.com/i/pAZaJbV)

  **ACSS**

  `ACSS的核心是将样式划分为小的，独立的原子类，通过组合这些类来构建页面的样式`

  > **原子类**：ACSS 将样式划分为原子类，每个原子类仅包含一个样式规则。这意味着每个类都负责应用一个特定的样式，例如边距，颜色，字体大小等
  >
  > **无状态**：ACSS 是无状态的，即每个原子类之间没有相互依赖，它们是相互独立的
  >
  > **可组合性**：可以通过不同的原子类来构建复杂的样式，这种可组合性使得样式的书写更灵活
  >
  > **工具类**：ACSS 中的原子类通常被称为工具类
  >
  > 样式的命名规范：通常采用一定的命名规范，以确保类名的语义清晰，易于理解，.bg-blue

  ACSS 库：**TailwindCSS, WindiCSS, Tachyons**

  ##### CSS 模块化方案

  > 目前主流的 CSS 模块化主要是两种：CSS Modules 和 CSS in JS 两种方案

  **CSS Modules**

  `CSS Modules指的是像import js一样去引入CSS代码，代码中的每一个类名都是引入对象的一个属性，编译时会将CSS类名加上唯一hash`

  `CSS Module需要webpack配置css-loader或者scss-loader，module为true`

  ```json
  {
    "loader": "css-loader",
    "options": {
      "modules": true, // 开启模块化
      "localIdentName": "[path][name]-[local]-[hash:base64:5]"
    }
  }
  ```

  **localIdentName**: 自定义生成类名格式，可选参数有:

  - [path]: 表示样式表相对于项目根目录所在的路径

  - [name]: 表示样式表文件名称

  - [local]: 表示样式表的类名定义名称

  - [hash: length]: 表示32位的hash值

    ```js
    // JSX
    import React from 'react';
    import style from './index.css';
    const Content = () => {
      return <div className={style.title}>Content title</div>;
    };
    export default Content;
    ```

    ```css
    // css
    .title {
      color: #983636;
      font-size: 40px;
    }
    ```

    编译后

    [![pAezmf1.png](https://s21.ax1x.com/2024/09/09/pAezmf1.png)](https://imgse.com/i/pAezmf1)

  **CSS Module 作用域**

  - 作用域认为 local 即只在当前模块生效
  - global：被:global 包裹起来的类名，不会被模块化

  ```css
  /* 加上 :global 会全局样式 */
  :global(.global-color) {
    color: blue;
    :global(.common-width) {
      width: 200px;
    }
  }
  ```

  ```js

  ```

  **CSS IN JS**

  `是一种技术，而不是一个具体的库实现。允许将样式作为js代码的一部分编写。每个组件拥有其独立的样式，完全隔离，方式样式冲突`

  `简单来说，CSS-IN-JS就是将css样式写在js文件里面，而不是独立的一些.css,.scss之类的文件，这样既可以在css中使用一些属于js的诸如模块声明，变量定义，函数调用和条件判断等语言特性来提供灵活的可扩展的样式定义`

  相关库

  - styled-components

    `React中被广泛使用`

    ```js
    // styles.js
    import styled, { css } from 'styled-components';

    // 创建一个名为 Wrapper 的样式组件 (一个 section 标签, 并带有一些样式)
    export const Wrapper = styled.section`
      padding: 10px;
      background: deepskyblue;
    `;

    // 创建一个名为 Title 的样式组件 (一个 h1 标签, 并带有一些样式)
    export const Title = styled.h1`
      font-size: 20px;
      text-align: center;
    `;

    // 创建一个名为 Button 的样式组件 (一个 button 标签, 并带有一些样式, 还接收一个 primary 参数)
    export const Button = styled.button`
      padding: 10px 20px;
      color: #333;
      background: transparent;
      border-radius: 4px;

      ${(props) =>
        props.primary &&
        css`
          color: #fff;
          background: blue;
        `}
    `;
    ```

    ```js
    // App.js
    import React from 'react';
    import { Wrapper, Title, Button } from './styles';

    // 然后，像使用其他 React 组件一样使用这些样式组件
    export default function App() {
      return (
        <Wrapper>
          <Title>Hello World, this is my first styled component!</Title>
          <Button>Normal Button</Button>
          <Button primary>Primary Button</Button>
        </Wrapper>
      );
    }
    ```

    `在Vue中使用Scoped CSS，为style区块添加scoped，开启组件样式作用域,编译打包之后，会为该组件的所有元素添加一个全局唯一的属性选择器,形如[data-v-323562623]`

    ```html
    <template>
      <header class="header">header</header>
    </template>

    <style scoped>
      .header {
        background-color: green;
      }
    </style>
    ```

    使用 CSS Modules

    `为style区块添加module属性即可开启CSS Modules,Vue会为组件注入一个名为$style的计算属性，并混淆类名`

    ```vue
    <template>
      <header :class="$style.header">header</header>
    </template>

    <style module>
    .header {
      background-color: green;
    }
    </style>
    ```

  - emotion

  - Radium

  - Style System

  - styled-jsx

  - JSS
