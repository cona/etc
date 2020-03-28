# 概要

CasperJSでcookieをHTTP headerに付与してリクエストする方法。

# 環境

node: v4.4.5
phantomjs: 2.1.1
casperjs: 1.1.1

# コード

```test.js
var casper = require('casper').create();

var cookies = [
    {name:'siki', value:'nyaha'},
    {name:'miku', value:'nya'}
];
var serialize = function(cookies){
    return cookies.map(function(v){
        return ' '+v.name+'='+v.value;
    }).join(';');
}

casper.start().then(function() {
    this.open('http://localhost:8080/', {
        method: 'get',
        headers: {
            'Cookie': serialize(cookies)
        }
    });
});

casper.then(function(){
    casper.capture('out.png');
});

```

# 動作確認

てきとうなスクリプトを用意。


```/usr/share/nginx/docker_html/index.php
<style>
body{ background-color:white }
</style>
<?php
var_dump($_COOKIE);
```

実行結果の out.png を閲覧する。
![image](https://qiita-image-store.s3.amazonaws.com/0/45991/7273c2c8-38a1-db0c-6593-316f545baafd.png)

問題なくセットできている。


# 参考

- Casper.page.content adds html tags for non-html content types · Issue #178 · casperjs/casperjs https://github.com/casperjs/casperjs/issues/178
