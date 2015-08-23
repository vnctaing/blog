---
layout: post
title: A quick overview of Angular.js
---


##Introduction
_I've been working in a startup called Bandsquare, for 6 months and we used Angular.js in our projects. I've learned Angular from scratch. I think I had the time to explore and experiment the principles features in Angular._ 



###Classic side-server & Single-Page Application

Angular lets you create SPA ( Single Page Application ). If you are already familiar with Single page app you can skip that part. 

Whenever you are browsing classic websites, you are doing the following schema :   

* You send a http request to a server
* This server treat your request while you are waiting for the response
* The server responds you by sending the data you requested 

Each time you need new data, you repeat the same schema. This works well and has been working since the beginning of the Web. Very popular programming languages such as Ruby or Python able you to create this kind of web app thanks to their web frameworks. These languages are called _synchronous programming languages_ you have to have to wait the request to be completed in order to move on to the next task, 

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

> Two-way data binding is a fundamental concept in Angular, it automatically sync your data with the view displayed.

If you modify the view, the model is also modified and if you modify the model, the view is also modified. It makes really easy to use a WYSIWYG app like in this fiddle.
        
  <iframe width="100%" height="300" src="//jsfiddle.net/vnctaing/36twf7qn/1/embedded/html,js,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

If you try to type in the first or the third input. You will see that the other element that contained a **directive** `ng-model="foo"`  are updated by same the value just typed. The elements with are all binded to the property `foo`. Angular is automatically listening to changement on each element and magically updates the other elements. 

### Controllers

Controllers are the logic behind a View. Each view is associated to a controller. To load a controller inside the template you simply declare it inside the element with the directive with `ng-controller=`, and everything inside this element will be in the controller scope. The controller will have access to this part of the DOM ( Document Object Model which is every elements displayed, it is basically HTML ). You declare a controller like this : 

my-view.html : 

{% highlight html linenos %}
<html ng-app="myApp">
  <div ng-controller="myCtrl">
  </div>
</html>
{% endhighlight %}

myCtrl.js :

{% highlight javascript linenos %}
angular.module('myApp')
  .controller('myCtrl', function () {

});
{% endhighlight %}
`ng-app` is a directive to limit and name your angular app, everything inside the ng-app can be accessed with Angular. Here, the name of the app is `myApp`.

### Dependency Injection

> **Dependency Injection** is a software design pattern, that let you declare every dependencies needed a part from your component and makes it available in the component. In Angular you simply declare the name of the component and Angular makes the dependency available for you. Angular is going to parse your function and find the component associate to the parsed parameter.

{% highlight javascript linenos %}
// firstDependency is a dependency that I use somewhere else in the app
angular.module('myApp')
  .controller('myCtrl', function (firstDependency,secondDependency) {
   // I can use firstDependency.doSomething() since it's injected in the controller
  });
{% endhighlight %}

Note : If you minify your javascript ( and you should ), there is a dirty hack to do. You have to declare your dependencies like this 

> Minification is the process of removing all unnecessary characters from source code without changing its functionality


{% highlight javascript linenos %}
angular.module('myApp')
  .controller('myCtrl', ['firstDependency', 'secondDependency',
   function (firstDependency,secondDependency) {
     
  ]})
{% endhighlight %}


That is because in order to save space, minifiers change the name of function params like this :

{% highlight javascript linenos %}
angular.module('myApp')
  .controller('c', function (a,b) {
   //  a.doSomething() => undefined, there is no dependency called 'a'
  });
{% endhighlight %}

by declaring dependencies in an array, it forces the minifier to not change the name of the params and keep the Dependency Injection working.


###The $scope object

`$scope` is fundamental in Angular 1.X, it is an injectable object. The view access to the data via `$scope`. Whenever you try to interpolate something with the double curly braces ```{% raw %}{{ }}{% endraw %}```. Angular is going to look in `$scope` properties.

myCtrl.js : 

{% highlight javascript linenos %}
angular.module('myApp')
  .controller('myCtrl', function ($scope) {
    $scope.name = "John"; // $scope.name is accessible from the view
  });
{% endhighlight %}

my-view.html :

{% highlight html linenos %}
  <html ng-app="myApp">
    <div ng-controller="MyCtrl">
      {% raw %}<p>{{name}}</p>{% endraw %} <!-- displays "John" -->
    </div>
  </html>
{% endhighlight %}

### Services


Service are objects that lets you share code across your app. You can *inject* services into controllers.



 You are more likely to use services when you want to re-use function across your app, i.e : you probably want to use services to fetch data from an API.

You can create a service with a "factory". A factory is a way to create services,  Like this : 

myService.js :

{% highlight javascript linenos %}
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
{% endhighlight %}

You can then use them into differents controller 

mySecondCtrl.js : 

{% highlight javascript linenos %}
angular.module('myApp')
  // myService is "injected" in mySecondCtrl
  .controller('mySecondCtrl', function ($scope, myService) {
    myService.sayHi(); // fires sayHi() in myService
  });
{% endhighlight %}

myCtrl.js : 

{% highlight javascript linenos %}
angular.module('myApp')
  // myService is "injected" in mySecondCtrl
  .controller('myCtrl', function ($scope, myService) {
    myService.sayBye(); // fires sayBye() in myService
  });
{% endhighlight %}

### Directives

Angular provides many built-in directives, you will have to discover them through [Angular Offical Documentation](https://docs.angularjs.org/api) and learn how they work. 

An example of commonly used directive is `ng-repeat`, if you want to iterate and display each elements of an array. You should use the directive `ng-repeat` that let you iterate through an array attached to `$scope.collection`. The syntax is : `ng-repeat="item in collection"` . Inside the element that contains the directive `ng-repeat` let you access `item`  like in the following fiddle :

  <iframe width="100%" height="300" src="//jsfiddle.net/vnctaing/wambs075/4/embedded/html,js,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>


### Custom Directives

Last but not least, in Angular you can create your own directives. It lets you create small, independant and re-usable components. Basically you will able to create a custom tag `<sign-up-form></sign-up-form>` and display it anywhere in your app. This makes the HTML more expressive. You will have to name your directive in [camelCase](https://en.wikipedia.org/wiki/CamelCase) , but you will have to call it in your template in [snake-case](https://fr.wikipedia.org/wiki/Snake_case).
To create a custom directive :


{% highlight javascript linenos %}
angular.module('myApp')
.directive('myDirective', function() {
  return {
    restrict: 'E', // E stand for element, it will let you create <custom-element></custom-element>
    // A stands for attributes, it will let you create <div custom-attributes></div>
    templateUrl: 'path/to/your/template.html', 
    link: function (scope, element, attributes){
      // put logic in here. i.e : 
      scope.name = "Foobar"; // name will be available inside the directive

    },
    scope: true, // can be true, false or {}. 
    // Let you set up if your directive has access to his parents scope.
    // For now, set it to true, once you will be familiar with creating custom directive
    // I sugggest you to read this very good article about scope :
    // http://www.undefinednull.com/2014/02/11/mastering-the-scope-of-a-directive-in-angularjs/
  }
  }
})
{% endhighlight %}

Once you created your directive you will be able to call it in your template : 

{% highlight html linenos %}
<my-directive></my-directive>
{% endhighlight %}


### Conclusion

- Two way data binding is the synchronisation between the view and the $scope.
- Controller are the logic behind the view and associate data to the $scope object
- $scope object is what let you access data from your template
- Service lets you share code globally across your app
- Directives are powerful component inside the template that be used for anything
- Dependy Injection is a magically handled by Angular for resolving dependency


I wanted to describe briefly the features that I judged fundamental, Angular comes with many more features. I recommend you to follow the very good [Angular](https://docs.angularjs.org/api) doc to know more about them. I hope this helped you to have an overview of Angular.
