# Webpack Static Pages Template

Optimizing static website with webpack 5

## Template feature

Run development server in localhost with Hot reload.

Build code for production:
  - Bundle code (multiple pages).
  - Minify HTML + CSS, Minify + Obfuscate Javascript.
  - Rename CSS + JS file by hash content to Cache Busting.
  - Compress images.

## Install

NPM: `npm install`

Yarn: `yarn install`

## Run development server

NPM: `npm start`

Yarn: `yarn start`

## Build production

NPM: `npm run build`

Yarn: `yarn build`

## Xử lý Golang HTML Template
Go HTML template dùng `{{}}`

Nếu không cấu hình đúng sẽ gặp phải lỗi khi trong inline javascript cũng có thẻ `{{}}` ví dụ
```js
<script>
  let foo = {{ Foo }}
  var bar = {{ Bar }}
  
  function factorialize(num) {
    if (num < 0)
      return -1;
    else if (num == 0)
      return 1;
    else {
      return (num * factorialize(num - 1));
    }
  }
  factorialize(5);
</script>
```
Code chi tiết ở file [webpack.prod.js](webpack.prod.js)
Cách xử lý là cấu hình `HtmlMinimizerPlugin` như sau:


1. Bổ xung `ignoreCustomFragments: [ /{{.*}}/ ],` để bỏ qua `{{}}`
2. Bật `minifyJS: true`
3. Bật `continueOnParseError: true,`

Cấu hình đã giúp WebPack minify được HTML chứa template syntax `{{}}` trong HTML và cả inline JavaScript
```js
new HtmlMinimizerPlugin({
      minimizerOptions: {
        ignoreCustomFragments: [ /{{.*}}/ ],
        minifyJS: true,
        removeComments: true,
        continueOnParseError: true,
      }
    }),
```

