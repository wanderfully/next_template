---
title: react中px转rem或vw
date: 2019-05-22 15:32:22
tags: 
- react
- webpack
top: 100
categories: 
- React
copright: 版权所有
---
<!-- 已经写好的文章在对应的md文件头部添加top：{number}即可 数值越大优先级越高 -->
### 一、px转rem

#### 执行显示webpack配置
`npm run eject`

#### 安装 postcss-px2rem-exclude、lib-flexible、sass-loader、node-sass
`npm insatll lib-flexible sass-loader node-sass postcss-px2rem-exclude --save`

#### 修改webpack.config.js,先引入postcss-px2rem-exclude,然后在postcss-loader的plugins中加入
`const px2rem = require('postcss-px2rem-exclude');`
```javascript
    {
        loader: require.resolve('postcss-loader'),
        options: {
            ident: 'postcss',
            plugins: () => [
            require('postcss-flexbugs-fixes'),
            autoprefixer({
                browsers: [
                '>1%',
                'last 4 versions',
                'Firefox ESR',
                'not ie < 9', // React doesn't support IE8 anyway
                ],
                flexbox: 'no-2009',
            }),
        //px2rem加在这里
            px2rem({
                remUnit:75,exclude: /node_modules/i
            })
            ],
        },
    },
```
#### 在react入口文件引入lib-flexible
`import "lib-flexible"`

#### 修改index.html 禁止缩放
`<meta content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport" />`

#### 完成后重启才能生效
`npm start`

### 二、px转vw

#### 安装需要的插件
```javascript
npm install -D postcss-import postcss-url cssnano-preset-advanced 
npm install -S postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg postcss-cssnext cssnano postcss-viewport-units
```
#### index.html修改禁止缩放
`<meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no,width=device-width">`

#### 新建配置文件 .postcssrc.js 并配置
```javascript
module.exports = {
  "plugins": {
    "postcss-import": {},
    "postcss-url": {},
    "postcss-aspect-ratio-mini": {},
    "postcss-write-svg": {
      utf8: false
    },
    "postcss-cssnext": {},
    "postcss-px-to-viewport": {
      viewportWidth: 750,
      viewportHeight: 1334, // (Number) The height of the viewport. 
      unitPrecision: 3, // (Number) The decimal numbers to allow the REM units to grow to. 
      viewportUnit: 'vw', // (String) Expected units. 
      selectorBlackList: ['.ignore', '.hairlines'], // (Array) The selectors to ignore and leave as px. 
      minPixelValue: 1, // (Number) Set the minimum pixel value to replace. 
      mediaQuery: false // (Boolean) Allow px to be converted in media queries.
    },
    "postcss-viewport-units": {},
    "cssnano": {
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false
    }
  }
}
```
#### vw的兼容处理（有些手机不支持vw单位）
`npm install viewport-units-buggyfill --save`

在项目入口处（如main.js）引入
```javascript
let hacks = require('viewport-units-buggyfill.hacks');
require(‘viewport-units-buggyfill‘).init({
    hacks: hacks
});
```
或者在index.html中引入并初始化
```javascript
<script src="//g.alicdn.com/fdilab/lib3rd/viewport-units-buggyfill/0.6.2/??viewport-units-buggyfill.hacks.min.js,viewport-units-buggyfill.min.js"></script>
<script>
    window.onload = function () {
      window.viewportUnitsBuggyfill.init({
        hacks: window.viewportUnitsBuggyfillHacks
      });
    }
</script>
```

#### 在webpack.config.js中引入并在 postcss-loader 中使用插件
```javascript
const postcssAspectRatioMini = require('postcss-aspect-ratio-mini')
const postcssPxToViewport = require('postcss-px-to-viewport')
const postcssWriteSvg = require('postcss-write-svg')
const postcssCssnext = require('postcss-cssnext')
const postcssViewportUnits = require('postcss-viewport-units')
const cssnano = require('cssnano')
```
```javascript
{
    loader: require.resolve('postcss-loader'),
    options: {
      // Necessary for external CSS imports to work
      // https://github.com/facebookincubator/create-react-app/issues/2677
      ident: 'postcss',
      plugins: () => [
        require('postcss-flexbugs-fixes'),
        autoprefixer({
          browsers: [
            '>1%',
            'last 4 versions',
            'Firefox ESR',
            'not ie < 9' // React doesn't support IE8 anyway
          ],
          flexbox: 'no-2009'
        }),
        postcssAspectRatioMini({}),
        postcssPxToViewport({
          viewportWidth: 750, // (Number) The width of the viewport.
          viewportHeight: 1334, // (Number) The height of the viewport.
          unitPrecision: 3, // (Number) The decimal numbers to allow the REM units to grow to.
          viewportUnit: 'vw', // (String) Expected units.
          selectorBlackList: ['.ignore', '.hairlines'], // (Array) The selectors to ignore and leave as px.
          minPixelValue: 1, // (Number) Set the minimum pixel value to replace.
          mediaQuery: false // (Boolean) Allow px to be converted in media queries.
        }),
        postcssWriteSvg({
          utf8: false
        }),
        postcssCssnext({}),
        postcssViewportUnits({}),
        cssnano({
          preset: 'advanced',
          autoprefixer: false,
          'postcss-zindex': false
        })
      ]
    }
}
```

#### 重启后生效
`npm start`

***