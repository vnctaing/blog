---
layout: post
title: A quick overview of Angular.js
---


##Introduction
_I've been working in a startup called Bandsquare, for 6 months and we used Angular.js in our projects. I've learned Angular from scratch. I think I had the time to explore and experiment the principles features in Angular._ 



###Classic side-server & Single-Page Application

Angular lets you create SPA ( Single Page Application ). If you are already familiar with this expression you can skip that part. 

Whenever you are browsing classic websites, you are doing the following schema :   

* You send a http request to a server
* This server treat your request while you are waiting for the response
* The server responds you by sending the data you requested 

Each time you need new data, you repeat the same schema. This works well and has been working since the beginning of the Web. Very popular programming languages such as Ruby or Python able you to create this kind of web app thanks to their web frameworks. Because you have to have to wait the request of the other part to continue in your process, these languages are called _synchronous programming languages_

But it presents some flaws : 

* There are redundacy in the resources that you request, you are asking to the server to render the almost same template again and again, while you only really need the data to change.
* On each action, you need to wait the server's reponse to continue your navigation, which might make you feel like the web app is slow. Especially if you have a bad internet connection or are using your smartphone. 
  

*Single-Page Application* are different, there are still a server and a client of course. But they interact differently, the workload on the server is reduced, indeed : 

* You send a first http request to the server
* The server sends you an unique page ( _the template_ )
* Whenever you need new data, the browser is only requesting the data that it needs. The response is then formated in JSON, which is much more lighter. 

The user that is navigating through your website can feel that it is _insanely_ faster than classic server-side application since the DOM is compiled and live rendered by your browser. It only needs the few bytes of corresponding data that the server provides quickly.
Angular.js ables you to build single page application.

Of course there are flaws : 

- The first request is often long since it requests the whole template
- Not every browser or computer supports well Javascript, it may take a very long time to compile the JS for old devices.
- Currently, the SEO is a big flaw, the robots that index websites doesn't know how to executes the JavaScript
- Many more ... 

---

## Installation 

Personnally, I like to use [Yeoman](http://yeoman.io/). Yeoman is a tool that let you download generators. These generators scaffold seed app, you don't need to figure out how to structure your code and find the CDN on the Internet. The interface in the console is very user-friendly, Yeoman asks you which options do you prefer and then generate an empty app for you. 

The Angular team provides a seed app for angular, you can download/clone the repository from [Github](https://github.com/angular/angular-seed)


## Overview of Angular.js

Angular is based on a MVC architecture. If you are not familiar with the MVC architecture. I suggest you have a quick read about it and come back then ( [Wikipedia](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) is perfectly fine ).  Angular might be confusing and surprising at first because it adds weird attributes inside HTML element which are called **directives** in Angular. But you will get quickly used to Angular's paradigm and be familiar with the common **directives**. 

Principles points about Angular :

- It uses the **two-way data binding**. Which means, the View and the Model ( which contains the data) are automatically sync. If you modify the View, it modifies the Model and inversely. 
- It divides your app in small components, each logic is associated with its view.
- Moreover, having specifics small component with a purpose makes your app easier to test.
- Dependency Injection (DI) , enables you to inject dependencies in your component and makes it available to use inside your component. 
- Directives are very powerful features, it enables you to create component in the DOM that can be used like re-usable and indepedent block.
- Angular has its own templating langage, which let you put use interpolate your variables or use logic in your template

### Two-way data binding

Two-way data is a fundamental concept in Angular, it automatically sync your data with the view displayed. If you modify the view, the model is also modified and if you modify the model, the view is also modified. It makes really easy to use a WYSIWYG app like in this fiddle.
        
  <iframe width="100%" height="300" src="//jsfiddle.net/vnctaing/36twf7qn/1/embedded/html,js,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

If you try to type in the first or the third input. You will see that the other element that contained a **directive** `ng-model="foo"`  are updated by same the value just typed. The elements with are all binded to the property `foo`. Angular is automatically listening to changement on each element and magically updates the other elements. 

### Controllers

Controllers are the logic behind a View. Each view is associated to a controller. To load a controller inside the template you simply declare it inside the element with the directive, and everything inside this element will be in the controller scope. The controller will have access to this part of the DOM ( Document Object Model which is every elements displayed, it is basically HTML ). You declare a controller like this : 

my-view.html : 

```html
<html ng-app="myApp">
  <div ng-controller="myCtrl">
  </div>
</html>
```

myCtrl.js :

```javascript
angular.module('myApp')
  .controller('myCtrl', function () {

});
```
`ng-app` is a directive to limit and name your angular app, everything inside the ng-app can be accessed with Angular. Here, the name of the app is `myApp`.


###The $scope object

`$scope` is fundamental in Angular 1.X, it is an injectable object. The view access to the data via `$scope`. Whenever you try to interpolate something with the double curly braces ```{% raw %}{{ }}{% endraw %}```.Angular is going to look in `$scope` properties.

myCtrl.js : 

``` javascript
angular.module('myApp')
  .controller('myCtrl', function ($scope) {
    $scope.name = "John"; // $scope.name is accessible from the view
  });
```

my-view.html :

```html
  <html ng-app="myApp">
    <div ng-controller="MyCtrl">
      {% raw %}<p>{{name}}</p>{% endraw %} <!-- displays "John" -->
    </div>
  </html>
```

### Services


Service are objects that lets you share code across your app. You can inject services into controllers. You are more likely to use services when you want to re-use function across your app, i.e : you probably want to use services to fetch data from an API.

You can create a service with a "factory". A factory is a way to create services,  Like this : 

myService.js :

```javascript
angular.module('myApp')
  .factory('myService', function () {
    function sayHi () {
      alert('Hi !');
    }

    function sayBye () {
      alert('Bye');
    }
  
  return {
    sayHi: sayHi,
    sayBye: sayBye
  }  
});
```

You can then use them into differents controller 

mySecondCtrl.js : 

```javascript
angular.module('myApp')
  // myService is "injected" in mySecondCtrl
  .controller('mySecondCtrl', function ($scope, myService) {
    myService.sayHi(); // fires sayHi() in myService
  });
```

myCtrl.js : 

```javascript
angular.module('myApp')
  // myService is "injected" in mySecondCtrl
  .controller('myCtrl', function ($scope, myService) {
    myService.sayBye(); // fires sayBye() in myService
  });
```

### Directives

Angular provides many built-in directives, you will have to discover them and learn their functionning. 
An example of common directive is `ng-repeat`, if you want to iterate and display each elements of an array. You should use the directive `ng-repeat` that let you iterate through an array attache to `$scope` . The syntax is : `ng-repeat="item in collection"`

  <iframe width="100%" height="300" src="//jsfiddle.net/vnctaing/wambs075/4/embedded/html,js,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>