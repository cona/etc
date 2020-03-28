
#概要

Slim3+Twigでrenderした結果を表示せずに取得したいことがありましたので、そのときのやり方を残しておきます。

#環境
- PHP 5.6.21
- slim/slim 3.4.1
- slim/twig-view 2.1.1

#導入
Slim3にTwigを導入するのは以下を参考にしました。
[GitHub - slimphp/Twig-View: Slim Framework view layer built on top of Twig](https://github.com/slimphp/Twig-View)

```php:src/dependencies.php
<?php
$container = $app->getContainer();

// Register Twig View helper
$container['view'] = function ($c) {
    $view = new \Slim\Views\Twig('../templates', []);

    // Instantiate and add Slim specific extension
    $basePath = rtrim(str_ireplace('index.php', '', $c['request']->getUri()->getBasePath()), '/');
    $view->addExtension(new Slim\Views\TwigExtension($c['router'], $basePath));

    return $view;
};
//以下略
```

#コード
##ふつうに表示(サンプル通り)
```php:src/routes.php
$app->get('/hello/{name}', function ($request, $response, $args) {          
    return $this->view->render($response, 'profile.html', [                 
        'name' => $args['name']                                             
    ]);                                                                     
});                                                                         
```

##表示内容を取得
```php:src/routes.php
$app->get('/hello/{name}', function ($request, $response, $args) {                                                                                   
    $html = $this->view->fetch('profile.html', [                             
        'name' => $args['name']                                              
    ]);
    // $htmlを使った何らかの処理

    return $response->withJson(['status'=>'success']);
});                                                                                                                                                                               
```


メールの内容を組み立てるときなどにも便利かもしれません。

#参考
- [Twig-View/Twig.php at master · slimphp/Twig-View · GitHub](https://github.com/slimphp/Twig-View/blob/master/src/Twig.php#L88)
- [GitHub - slimphp/Twig-View: Slim Framework view layer built on top of Twig](https://github.com/slimphp/Twig-View)
