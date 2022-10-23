# test-sail-laravel9-and-vue3

## sailでプロジェクトを作成

aliasの追加
```.bashrc
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'
```

```bash
curl -s https://laravel.build/practice-sail-laravel9-and-vue3 | bash
cd practice-sail-laravel9-and-vue3
sail up -d
```

## vueのインストール

```bash
sail npm install @vitejs/plugin-vue --save-dev
```

## App.vueコンポーネントの作成
resources/js/componentsにApp.vueを作成

```vue
<template>
    <h1>Hello World</h1>
</template>
```

## app.jsにApp.vueをインポートする

```js
import './bootstrap';
import { createApp } from "vue";
import App from "./components/App.vue"

const app = createApp(App);
app.mount("#app");
```

## welcom.blade.phpを編集
```php
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <title>Laravel Vite Vue</title>
        @vite(['resources/css/app.css', 'resources/js/app.js'])
    </head>
    <body>
        <div id="app"></div>
    </body>
</html>
```

## vite.config.jsを編集

```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
    //WSL2でvite開発サーバーへアクセスするために必要
    server: {
        host: true,
        hmr: {
            host: 'localhost',
        },
    },
    plugins: [
        laravel({
            input: ['resources/css/app.css', 'resources/js/app.js'],
            refresh: true,
        }),
        vue({
            template: {
                transformAssetUrls: {
                    base: null,
                    includeAbsolute: false,
                },
            },
        }),
    ],
});
```

## vite開発サーバーを立ち上げる

```bash
sail npm run dev
```

[localhost](http://localhost/)へアクセスしHello Worldと表示されていたらOK