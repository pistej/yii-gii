インストール
============

## Composer パッケージを取得する

このエクステンションをインストールするのに推奨される方法は [composer](http://getcomposer.org/download/) によるものです。

下記のコマンドを実行してください。

```
php composer.phar require --dev --prefer-dist yiisoft/yii2-gii
```

または、あなたの `composer.json` ファイルの `require` セクションに、

```
"yiisoft/yii2-gii": "~2.1.0"
```

を追加してください。


## アプリケーションを構成する

Gii エクステンションがインストールされたら、アプリケーションの構成情報ファイルに以下の行を追加して、Gii を有効にします。

```php
return [
    'bootstrap' => ['gii'],
    'modules' => [
        'gii' => [
            '__class' => yii\gii\Module::class,
        },
        // ...
    ],
    // ...
];
```

そうすると、次の URL で Gii にアクセスすることが出来ます。

```
http://localhost/path/to/index.php?r=gii
```

綺麗な URL を有効にしている場合は、次の URL を使います。

```
http://localhost/path/to/index.php/gii
```

> Note: ローカルホスト以外の IP から Gii にアクセスしようとすると、デフォルトでは、アクセスが拒否されます。
> このデフォルトを回避するためには、許可される IP アドレスを構成情報に追加してください。
>
```php
'gii' => [
    '__class' => yii\gii\Module::class,
    'allowedIPs' => ['127.0.0.1', '::1', '192.168.0.*', '192.168.178.20'] // 必要に応じて修正
],
```

コンソール・アプリケーションの構成情報において同じように Gii を構成すると、次のようにして、コマンド・ウィンドウから
Gii にアクセスすることが出来ます。

```
# パスをアプリケーションのベースパスに変更
cd path/to/AppBasePath

# Gii に関するヘルプ情報を表示
yii help gii

# Gii のモデルジェネレータに関するヘルプ情報を表示
yii help gii/model

# city テーブルから City モデルを生成
yii gii/model --tableName=city --modelClass=City
```

### ベーシック・アプリケーション

ベーシック・プロジェクト・テンプレートの構成情報の構造は少し違っており、
Gii は `config/web.php` の中で構成しなければなりません。

```php
// ...
if (YII_ENV_DEV) {
    // 'dev' 環境のための構成の修正
    $config['bootstrap'][] = 'debug';
    $config['modules']['debug'] = yii\debug\Module::class;

    $config['bootstrap'][] = 'gii';
    $config['modules']['gii'] = yii\gii\Module::class; // <--- ここ
}
```

従って、IP アドレスを調整するためには、次のようにする必要があります。

```php
if (YII_ENV_DEV) {
    // 'dev' 環境のための構成の修正
    $config['bootstrap'][] = 'debug';
    $config['modules']['debug'] = yii\debug\Module::class;

    $config['bootstrap'][] = 'gii';
    $config['modules']['gii'] = [
        '__class' => yii\gii\Module::class,
        'allowedIPs' => ['127.0.0.1', '::1', '192.168.0.*', '192.168.178.20'],
    ];
}
```
