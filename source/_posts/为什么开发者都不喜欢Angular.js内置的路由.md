title: UI-Router:为什么开发者都不喜欢Angular.js内置的路由
tags:
    - '译文'
-----

**Angular.js** 是一个用来构建“富客户端”的神奇JavaScript框架。但是事实却是许多开发者却不使用其内置的路由模块。反而使用**AngularUI**项目的 **UI-Router**模块来代替之。

这是因为**UI-Router**有两个重要的特性：
* 多样化视图
* 嵌入式视图

这篇文章将解释这两个特性，和运用现实生活的例子展现这两个特性的重要性。

## 为什么你需要使用UI-Router

### 多样化视图
大多数的应用程序都可以分解为一个一个区块。最简单的情况，一个应用程序有头部（header）,主体内容（main content area），以及一个尾部（footer）。

通常一个应用程序会有一个额外的侧边栏（sidebar ）在页面的左边或者右边。

整体结构如下图所示：

![应用结构图](http://7xpgwr.com1.z0.glb.clouddn.com/311153-cf44cd10702f8668.jpg)

大多数用例中，这些区块将同时显示在页面上。**Angular.js** 的内置路由***ngRoute***只允许一个视图（ng-view）出现在页面上。这样限制的情况下，人们可以使用包含页面（ng-include）或者 其他的变通方法为应用创建一个布局（layout）或主页（master page）。**UI-Router**支持多样化视图，并且每一个视图都有自己相应的控制，所以每个区块都是封装好，可以复用到整个应用程序需要的地方。

### 嵌入式视图

常见的例子中，一个应用的嵌入式页面一般是主页的详情页面，更具体的说，就是列表的详情页面。许多应用程序，都有列表页面，点击其中一个列表元素，可以进入到列表的详情页面。更进一步说，你点击列表中一个行的连接，进入一个 **可查看** 详情页面或是一个 **可编辑** 的表单。

如下图所示：
![嵌入式视图示意图](http://7xpgwr.com1.z0.glb.clouddn.com/311153-380ffd07e591e1cb.jpg)

如果列表页面和详情页面是单独分开的（或者他们被Angujar.js回调），使用**Angular.js**的内置路由***ngRoute*** 是非常容易完成的。然而，如果你想要保持列表不变，而详情页面出现在列表的右边或者下面，这样就变得非常具有挑战性了。

需要澄清的是，这样的要求可以使用***ngRoute***来完成。但是你需要让两个控制器（一个用于列表，一个用于显示和隐藏详情）共享一个视图。这样的结果不是理想的，因为我们想要列表和详情页面有各自的控制器和视图，并且职责单一（显示列表或者显示列表项目的详情）。封装这些用户接口模块到它们各自的视图，这样我们就有更多的“可组合UI”，允许我们将各个区块整合到一起，或者根据需求拆分。嵌入式视图，不仅能够让这些视图同时出现，还能让一个视图嵌入到另一个视图中。

### 历史

当**Angular.js**首次发布***ngRoute***的时候，是有类似功能的路由存在的。这样路由包含在**Backbone.js**中，以及独立路由库**History.js**和**Sammy.js**。总是，他们映射一个路由或者是URL所需要运行的JavaScript代码，当URL改变时，需要将其增加到浏览器的历史记录中，防止按回退按钮不会破坏路由。

最终获胜的JavaScript**MV***框架，是想**Ember.js**和**Durandal.js **这样，创建出更健壮的路由，以支持多样化视图和内嵌式视图，并且在内部使用“状态机”设计模式。

AngularJS官方回应称，从1.1.6版本将***ngRoute***从angular.js核心中删除（更多的说法是1.2）。***ngRoute***依然可以从AngularJS的官网上获得，但是它早已不在核心之中。

AngularJS的社区认为，更受欢迎的路由库是**AngularUI **项目的**UI-Router**。

AngularJS的几个顾问包括Rob Eisenberg（Durandal.js 和Aurelia的创建者），正在重写***ngRoute***，并声称最终将在某个时间点一直回来，预期的版本是Angular.js 2.0。（注1）

如果你想了解更详细的历史和各种路由的优缺点，你可以查看Angular.js 2.0的路由设计公开文档。（需翻墙）

[https://docs.google.com/document/d/1I3UC0RrgCh9CKrLxeE4sxwmNSBl3oSXQGt9g3KZnTJI/edit#heading=h.xgjl2srtytjt](https://docs.google.com/document/d/1I3UC0RrgCh9CKrLxeE4sxwmNSBl3oSXQGt9g3KZnTJI/edit#heading=h.xgjl2srtytjt)

这个文档你可以点击右上方的绿色按钮，选择建议模式（suggesting）和查看模式（viewing），使页面更清晰。

![建议模式](http://7xpgwr.com1.z0.glb.clouddn.com/311153-f6e886a8b9891708.png)

![查看模式](http://7xpgwr.com1.z0.glb.clouddn.com/angularjs-router-design-document-2.png)

### 安装
使用**UI-Router**，基于Angular.js 1.2.x或Angular.13.x，你可以通过以下一种方式获得其**JavaScript**源码:

#### 下载
下载源码文件，或者混淆压缩后版本：
* src版本：[http://angular-ui.github.io/ui-router/release/angular-ui-router.js](http://angular-ui.github.io/ui-router/release/angular-ui-router.js)
* min版本：[http://angular-ui.github.io/ui-router/release/angular-ui-router.min.js](http://angular-ui.github.io/ui-router/release/angular-ui-router.min.js)

#### bower install
`$ bower install angular-ui-router`

#### npm install
`$ npm install angular-ui-router`

### 引入文件
引入`angular-ui-router.js`或`angular-ui-router.min.js`到你的`index.html`，必须Angualr.js核心文件之后，如下：

```
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.1.5/angular.min.js">
</script><script src="js/angular-ui-router.min.js"></script>
```
(注2)

### 引入依赖
将“ui.router”依赖添加到你的主Angular.js module中。

`var myApp = angular.module('myApp', ['ui.router']);`

> 注意：是`ui.router`不是`ui-router`，后者是许多人经常犯的错误。


## 路由状态机
**UI-Router** 引进了状态机设计模式，抽象高于传统的路由。**路由成了状态，URL就成了状态的一个简单属性。**

```
var app = angular.module('demo', ['ui.router']);

app.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {
    $urlRouterProvider.otherwise('/');

    $stateProvider
        .state('home', {
            url:'/',
            templateUrl: 'templates/home.html',
            controller: 'HomeController'
        })
        .state('about', {
            url:'/about',
            templateUrl: 'templates/about.html',
            controller: 'AboutController'
        })

}]);
```

当你想通过`ui-sref`创建一个链接是，使用的是状态而不是URL。

使用：
`<a ui-sref="home">Home</a>`

放弃：
`<a href="#/">Home</a>`

在上面的例子中，***ui-sref***可以这样理解：***ui***是**AngularUI**项目所有指令的前缀，***sref***是包装了传统HTML锚点标签的***href***属性和状态判断。

### 控制器中使用
下面例子展示的是，如果在一个控制器中重定向一个状态。
```
$scope.redirectToAbout = function(){
    $state.go('about');
}
```

### $stateProvider 替换 $routeProvider
当使用**UI-Router**时，为Angular.js服务注入路由支持，就由***$routeProvider***和 *** ngRoute***变成了***$stateProvider***。

### $urlRouterProvider
**$urlRouterProvider** 在这里有两个主要目的。一是建立一个默认路由，用于管理未知的URL（统一跳转到某处）。
```
app.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {
    $urlRouterProvider.otherwise('/');
    ...
}]);
```

二是监听浏览器地址栏URL的变化，重定向到路由定义的状态中。
```
app.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {
    $urlRouterProvider
        .when('/legacy-route', {
            redirectTo: '/'
        });
}]);
```

总之，**$urlRouterProvider**让我们处理状态机抽象的***$stateProvider***没有检测到的情况。

现在你对**UI-Router**有了一个基本认识，**UI-Router**的这些特性，让它比**ngRoute**更好。

## UI-Router实践
我们将先看到一个嵌入式视图的例子，然后在看到一个多样化视图的例子。之后，我们再重头看怎样将二者一起应用到一个现实世界的例子。

### UI-Router嵌入式视图案例
**UI-Router**嵌入式视图的列表详情页面。这个例子显示的是一个电视节目的列表。
![电视节目列表](http://7xpgwr.com1.z0.glb.clouddn.com/311153-b099aa3e5bcbf581.png)

如果你点击其中一行，你可以看到这行的详情描述。
![节目详情](http://7xpgwr.com1.z0.glb.clouddn.com/102.png)

#### index.html
AngularJS的应用程序是单页应用程序，视图是插入到shell页中的。这里就是我们的shell页——index.html:
```
<!doctype html>
<html id="data-ng-app" data-ng-app="demo">
    <head>
        <meta charset="utf-8">
        <title>ui router demo</title>
        <style type="text/css">
            .selected{background-color: #efefef; width:120px; }
            .detail{width: 300px;margin: 30px;border-top: 1px solid #efefef;}
        </style>
        <!-- IE8-HTML5: https://code.google.com/p/html5shiv/ -->
        <script src="js/libs/html5shiv.js"></script>

    </head>
    <body id="index">

        <!-- Angular UI Router Directive for template insertion -->
        <div id="content" ui-view></div>

        <script src="js/libs/angular.js"></script>
        <script src="js/libs/underscore.js"></script>
        <script src="js/libs/angular-ui-router.js"></script>        
        <script src="js/main.js"></script>      
    </body>
</html>
```
**UI-Router** 将第一级视图或是父视图（在例子中是shows.html）显示在`<div id="content" ui-view></div>`这个`div`之中。

#### 主页视图（templates/shows.html）
**shows.html**是列表页面。
```
<ul>
    <li ui-sref-active="selected" ng-repeat="show in shows">
        <a ui-sref="shows.detail({id: show.id})">{{show.name}}</a>
    </li>
</ul>

<div class="detail" ui-view></div>
```
正如前面所提到的，index.html中有一个`ui-view`属性指令，当相应的路由被请求时，视图（shows.html）则会渲染在这个`div`中。

请注意，这里有另一个`ui-view`嵌入在shows.html中。这个`ui-view`代表的是一个当父视图已渲染之后，再出现的子视图。在这个例子中，是shows-detail.html。

#### 显示详情视图（templates/shows-detail.html）
**shows-detail.html**是详情页面。
```
<h3>{{selectedShow.name}}</h3>
<p>
    {{selectedShow.description}}
</p>
</code>
```

#### 控制器
下面是各个视图相应的控制器。

##### ShowsController
**ShowsController** 从**ShowsService**加载一个内存中的数组显示。
```
app.controller('ShowsController', ['$scope','ShowsService', function($scope, ShowsService) {
    $scope.shows = ShowsService.list();
 }]);
```

##### ShowsDetailController
**ShowsDetailController** 从**ShowsService**获取要显示项的id，并设置给`$scope.selectedShow
 `。
```
app.controller('ShowsDetailController', ['$scope','$stateParams', 'ShowsService', function($scope, $stateParams, ShowsService) {
        $scope.selectedShow = ShowsService.find($stateParams.id);
 }]);
```

#### 配置
我们需要使用***$stateProvider***配置**UI-Router**。

当我们按照`父状态名.子状态名`的方式定义一个状态，**UI-Router**便知道子状态是内嵌在父状态中的。
```
app.config(['$stateProvider', '$urlRouterProvider', function($stateProvider, $urlRouterProvider) {
    $urlRouterProvider.otherwise('/shows');

    $stateProvider
        .state('shows', {
            url:'/shows',
            templateUrl: 'templates/shows.html',
            controller: 'ShowsController'
        })
        .state('shows.detail', {
            url: '/detail/:id',
            templateUrl: 'templates/shows-detail.html',
            controller: 'ShowsDetailController'
        });
}]);
```
真正伟大的是，嵌入式视图是列表控制器实现列表和详情相关的部分，而详情控制器只负责显示详情。

演示如何解耦合，我们只需要更改如有配置，将嵌入式页面更改为独立的两个虚拟页（一个列表路由，一个详情路由）。更具体的说，就是将`shows.detail`更改为`detail`。
```
$stateProvider
    .state('shows', {
        url:'/shows',
        templateUrl: 'templates/shows.html',
        controller: 'ShowsController'
    })
    .state('detail', {
        url: '/detail/:id',
        templateUrl: 'templates/shows-detail.html',
        controller: 'ShowsDetailController'
    });
    ...
```
并且将链接状态的地方由`<a ui-sref="shows.detail({id: show.id})">{{show.name}}</a>`，更改为`<a ui-sref="detail({id: show.id})">{{show.name}}</a>`。

现在我们的例子，变成了连个独立的页面分别显示。

#### Service
**ShowsService**在这个例子中，使我们的数据访问层。它的职责就是保持一个数组在内存中，使用`underscore.js`(注3)非常容易实现这点。

```
app.factory('ShowsService',function(){
    var shows = [{
        id: 1,
        name: 'Walking Dead',
        description: 'The Walking Dead is an American post-apocalyptic horror drama television series developed by Frank Darabont. It is based on the comic book series of the same name by Robert Kirkman, Tony Moore, and Charlie Adlard. It stars Andrew Lincoln as sheriff\'s deputy Rick Grimes, who awakens from a coma to find a post-apocalyptic world dominated by flesh-eating zombies.'
    },
    {
        id: 2,
        name: 'Breaking Bad',
        description: 'Breaking Bad is an American crime drama television series created and produced by Vince Gilligan. The show originally aired on the AMC network for five seasons, from January 20, 2008 to September 29, 2013. The main character is Walter White (Bryan Cranston), a struggling high school chemistry teacher who is diagnosed with inoperable lung cancer at the beginning of the series.'   
    },
    {
        id: 3,
        name: '7D',
        description: 'The 7D is an American animated television series produced by Disney Television Animation, and broadcast on Disney XD starting in July 7, 2014. It is a re-imagining of the titular characters from the 1937 film Snow White and the Seven Dwarfs by Walt Disney Productions'
    }];


    return {
        list: function(){
            return shows;
        },
        find: function(id){
            return _.find(shows, function(show){return show.id == id});
        }
    }
 });
```

### UI-Router多样化视图案例
下面这个例子有多个区块在一个页面，有`header`，`content`，`footer `。它们被**UI-Router**用多样化视图所管理。

![多样化视图案例](http://7xpgwr.com1.z0.glb.clouddn.com/103.png)

这是一些主导航，和在这个应用中根据用户导航填充的各式各样的虚拟页。

![子页](http://7xpgwr.com1.z0.glb.clouddn.com/104.png)

#### index.html
```
<!DOCTYPE html>
 <html class="no-js">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title>Index</title>
        <meta name="description" content="">
        <meta name="viewport" content="width=device-width, initial-scale=1">


    </head>
    <body ng-app="demo">

       <div ui-view="header"></div>
       <div ui-view="content"></div>
       <div ui-view="footer"></div>

    <script src="/js/bower_components/angular/angular.js"></script>
    <script src="/js/bower_components/angular-ui-router/release/angular-ui-router.js"></script>
    <script src="/js/main.js"></script>

    </body>
</html>
```
请注意，这里的`ui-view`属性指令都被赋予了一个名字：`header`，`content`，`footer`。这些名字是当我们在配置路由，要指定`view/template`和控制器需要作用于页面上的那个区块时引用。

#### Templates/Views
这些模版是直接简单的例子。**Header.html**有一些导航。这些导航使用`ui-sref`指令导航到指定的状态。

##### partials/header.html
```
<div class="ul">
    <li><a ng-href="/">Home</a></li>    
    <li ui-sref-active="active"><a ui-sref="dashboard">Dashboard</a></li>
    <li ui-sref-active="active"><a ui-sref="campaigns">Campaigns</a></li>
</div>
```

##### partials/content.html
```
<p>This is the default content.</p>
```

##### partials/footer.html
```
<p>This is the footer.</p>
```

##### partials/dashboard.html
```
<h2>Dashboard</h2>
```

##### partials/campaigns.html
```
<h2>Campaigns</h2>
```

当相应的路由被请求时，将用相应的模版，如`dashboard.html`，`campaigns.html`替换掉`content.html`。

#### 配置
像上一个例子中那样，我们使用`$stateProvider`配置状态（路由）。

下面的关键是注意，一个`url`不是一个只有一个`templateUrl`和`controller`属性了，而是使用`views`属性，赋予它一个各自的`templateUrl`和`controller`的集合。

所以原来这样：

```
.state('home',{
        url: '/',
        templateUrl: '/templates/partials/header.html',
        controller: 'HomeController'
    })
```

就变成了这样：

```
.state('home',{
        url: '/',
        views: {
            'header': {
                templateUrl: '/templates/partials/header.html',
                controller: 'HeaderController'
            },
            'content': {
                templateUrl: '/templates/partials/content.html',
                controller: 'ContentController'
            },
            'footer': {
                templateUrl: '/templates/partials/footer.html',
                controller: 'FooterController'
            }
        }
    })
```

下面是完整的代码。（请注意：为了保持完整性，我把不需要控制器的写在上面）

```
var app = angular.module('demo', ['ui.router']);

app.config(function($stateProvider, $urlRouterProvider){

    $urlRouterProvider.otherwise('/');

    $stateProvider
    .state('home',{
        url: '/',
        views: {
            'header': {
                templateUrl: '/templates/partials/header.html'
            },
            'content': {
                templateUrl: '/templates/partials/content.html'
            },
            'footer': {
                templateUrl: '/templates/partials/footer.html'
            }
        }
    })

    .state('dashboard', {
        url: '/dashboard',
        views: {
            'header': {
                templateUrl: '/templates/partials/header.html'
            },
            'content': {
                templateUrl: 'templates/dashboard.html',
                controller: 'DashboardController'
            }
        }

    })

    .state('campaigns', {
        url: '/campaigns',
        views: {
            'content': {
                templateUrl: 'templates/campaigns.html',
                controller: 'CampaignController'
            },
            'footer': {
                templateUrl: '/templates/partials/footer.html'
            }
        }

    })
});
```

另外需要注意的是，如果我没有填充一个区块或试图，那么用户导航到该路由时，将不会显示。这是不理想的，并且违反**DRY**原则（注4）。所以接下来的部门，我们将看到如何使用内嵌式视图去除冗余。

### UI-Router的内嵌式视图和多样化视图案例
现在我们了解了这些伟大的特性（内嵌式视图和多样化视图），让我们用这些特性一起应用到一个真实世界的应用程序中。

#### 配置
因为视图的模板与多样化视图的例子相同，所用我们复用它的配置。

```
var app = angular.module('demo', ['ui.router']);

app.config(function($stateProvider, $urlRouterProvider){

    $urlRouterProvider.otherwise('/');

    $stateProvider
    .state('app',{
        url: '/',
        views: {
            'header': {
                templateUrl: '/templates/partials/header.html'
            },
            'content': {
                templateUrl: '/templates/partials/content.html'
            },
            'footer': {
                templateUrl: '/templates/partials/footer.html'
            }
        }
    })

    .state('app.dashboard', {
        url: 'dashboard',
        views: {
            'content@': {
                templateUrl: 'templates/dashboard.html',
                controller: 'DashboardController'
            }
        }

    })

    .state('app.campaigns', {
        url: 'campaigns',
        views: {
            'content@': {
                templateUrl: 'templates/campaigns.html',
                controller: 'CampaignController'
            }
        }

    })

    .state('app.subscribers', {
        url: 'subscribers',
        views: {
            'content@': {
                templateUrl: 'templates/subscribers.html',
                controller: 'SubscriberController'      
            }
        }

    })
    .state('app.subscribers.detail', {
        url: '/:id',
        /*
        templateUrl: 'templates/partials/subscriber-detail.html',
        controller: 'SubscriberDetailController'
        */

        views: {
            'detail@app.subscribers': {
                templateUrl: 'templates/partials/subscriber-detail.html',
                controller: 'SubscriberDetailController'        
            }
        }

    });

});
```

我们在`/`路由上建立一个默认的状态`app`。在`app`路由上定义默认的内容区块，头部区块和尾部区块。然后，我们想在这个应用中定义被的路由，只需要在`app`后使用`.`的语法，如：`app.campaigns`。请注意，我们只需要替换内容区块（ui-view='content'），除非我们想要改变头部和尾部。因为这些视图都是定义在`app`路由之下的。

#### 状态名
在上面的代码中，最难理解的概念是状态名中的`@`语法。状态名的这个语法可以做如下解释：

写一个状态名，需要回答两个问题：

1. 当路由被请求时，我应该拿我的模板去替换那个区块？更具体的说，状态名就是`ui-view`属性指令的值。下面一个例子展示了`ui-view`属性指令和它对应的区块：
  + ui-view='content' = content
  + ui-view='header' = header
  + ui-view='footer' = footer

2. 哪里可以找到`ui-view`所指向的区块？
  + 在`ui-view`使用的不是直接的`templateUrl`，而是包含该模板的状态
  + 当`ui-view`和视图区块包含在应用程序的壳模板（index.html）中时，因为index.html没有定义任何状态，你应该设置为空字符串或者不设置。

把这两个放在一起说，这个如同`question1@question2`的语法，更具体的说，其实是`区块@状态名`。

所以，你需要在index.html页面上找内容区块时，你需要这样写:`content@`。

* 这之后的两个都是在`@`符后是空白的状态名，说明他们都是在index.html上的区块。

如果你要找到subscribers.html上的详情内容区块，你需要这样写：`detail@app.subscribers`。

#### 壳页面（index.html）
在上面的例子中，index.html并没有发生改变，只是简单地定义了各个区块：头部，尾部和内容。

#### Views/Templates

##### Header (partials/header.html)
Header的更新，是通过`ui-sref`引用嵌入的状态。如`.campaigns`不是调用`campaigns`状态，而是根据当前状态，推断出`.campaigns`的父状态。
```
<div class="ul">
    <li><a ng-href="/">Home</a></li>    
    <li ui-sref-active="active"><a ui-sref=".dashboard">Dashboard</a></li>
    <li ui-sref-active="active"><a ui-sref=".campaigns">Campaigns</a></li>
    <li ui-sref-active="active"><a ui-sref=".subscribers">Subscribers</a></li>
</div>
```

下面是一个新用户的模板的例子。

##### partials/subscribers.html
```
<h2>Subscribers</h2>

<ul>
    <li ng-repeat="subscriber in subscribers">

        <a ui-sref=".detail({id: subscriber.id})" > {{subscriber.name}}</a>
        {{subscriber.email}}
    </li>
</ul>

<div ui-view="detail"></div>
```

##### partials/subscriber-detail.html
```
{{selected.description}}
```

## 结论
掌握这样的状态名语法是有些困难的。但是有一个健壮的路由的好处是，允许你封装`view/controller`对组成你的用户界面。我觉得这样的困难是有价值的。所以，放心使用最后一个例子作为起点，构建一个神奇的，可维护的应用。让我知道你是否使用UI-Router，或者其他什么问题，请评论。

## 译者注
* 注1: 2.0最近发布了，还没去看有没有回来
* 注2: 由于国内网络的不可抗因素，不建议使用谷歌的cdn
* 注3: Underscore.js 是一个javascript工具库，提供了一整套函数式编程支持。在angular中使用[参考](http://www.oodlestechnologies.com/blogs/Use-of-Underscore-js-in-AngularJS)。
* 注4：DRY原则，Don't Repeat Yourself。不要重复自身，即降调代码是去冗余，同样的代码只写一次，不同地方调用。

原文地址：[http://www.funnyant.com/angularjs-ui-router/](http://www.funnyant.com/angularjs-ui-router/)
