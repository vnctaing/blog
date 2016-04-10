---
layout: post
title: React.js just an another hype JavaScript framework ?
---


*React v.15.x has been just released (cuz fuck semver, we are Facebook.). I saw an opportunity to explain what is React. Few months back, I started programming and heard about React. I asked on Reddit : [Why React is awesome](https://www.reddit.com/r/javascript/comments/3hs4io/why_react_is_awesome/).*


*TL;DR : If you are starting to develop a complex app, React might be a painkiller for you and your team.*

React is a popular library that let your create reusable component in JavaScript, it was open-sourced by Facebook in 2013, since then it got a lot of popularity. But isn't it just an other hype framework ? 

# How is it different from other JavaScript Framework ?


### Other JavaScript frameworks
Currently frameworks are trying to give expressiveness to the DOM. 

i.e : With Angular 1.xx ( apparently it changes in version 2.xx ) 

```html
<div ng-repeat="friend in friends | filter:searchText">
```

With this approach,the `<div>` is becoming *smart enough* to render a list of friends based on a function. This is not the role of the View ( V, of MVC). It is supposed to only represent the data model, nothing more.

### React

I haven't seen any quicker or simpler explanation for React than [@danneu](https://www.reddit.com/r/javascript/comments/3hs4io/why_react_is_awesome/#thing_t1_cua60al)'s : 

>React is simple. It's kinda like `render(data) -> UI`

You simply give data to a function and it renders the view. If the data change, the view *reacts* to it and modifies the least DOM nodes possible, thanks to its diff algorithm.

### Different approaches 

>Most JavaScript frameworks try to give more expressivity to the DOM whereas React uses Javascript expressivity to build the DOM.

The Model is modifying the View and not the other way around.

React is just a library for rendering the view. It **does not** come with a routing library, form validation library, data model library... You are free to use ( and contribute, of course ) to the libraries available that you are comfortable with.

Some people will say : comparing Angular/Ember and React, is like comparing apple vs oranges. 

Yes. React is a library whereas Angular/Ember are frameworks. But if you take an app built with the React ecosystem vs an app built with Angular/Ember, you can definitely compare them. 

# Virtues of React

### A solid and healhty tree structure
**React makes your app more *simple* but it does not mean it will be *easier* to build.**

It is probably not obvious to see React's qualities until you have to maintain an Angular or jQuery spaghetti app where you have plenty of source of truths, where the model can be modified by anybody from the view...

React.js lets you structure your app like a tree/pyramid, the data is updated at the top and is passed to leaf-components to the bottom. Which is simpler to understand and easier to maintain, since a component is only linked to its parents and not to his brother or his third uncle... In this case, you are more likely to end up with an app like that with Angular or jQuery.

Two-way data binding is a bad concept. Few years ago, it was magic, you could create a to-do app really fast, your view and model were synced. But quickly we realize when the app becomes more complexe, this kind of data flow makes your app hard to maintain. Imagine if anybody can modify the view, it modifies also your model, but you forgot that a model depended on your modified model and you will find yourself ( or not, which is worse ) that your created unexpected effects. This is how to start to a spaghetti code.

With the pyramid structure, **the events go up and the data flows down** in this way your app will look much more simpler.

### It is just javascript, easy to test and predictable
React components are just a function that renders the DOM, you pass data to it and React will generate the DOM accordingly to it. You only have to worry on how to feed these React component with the good data. Since it is just about functions, it makes very easy and predictable to test an input and an output. Even the JSX syntax inside React Component, that *imitates* HTML syntax to let your write HTML inside JavaScript, is just plain JavaScript.

### Learn about functionnal programming
The React ecosystem encourages you to learn a lot about functionnal programming. It will make you a better programmer by seeing a new paradigm, in terms of architectures, logic and syntax of your code.

### ES6/ES2015 is strongly recommended

EcmaScript 5 is yesterday's JavaScript. If you haven't heard about EcmaScript 6/ES2015/Harmony ( basically the biggest update of web ), you definitely have to [learn about it](https://developer.mozilla.org/en/docs/Web/JavaScript/New_in_JavaScript/ECMAScript_6_support_in_Mozilla). It is possible to write a React App with ES5, but I strongly recommend you to write your app in ES6, since it was officially approved. Almost every docs (oddly not Facebook's) assume that you are running on Babel and writting ES6.

### Dev mobile is possible

Developping native app is also possible with React Native. Unfortunately, I haven't had the occasion to try this but keep in mind it is possible to write **native app** ( not cross-platform ) in JavaScript with React Native.


# React will come at a price (and headaches )

### You might not need to simplify your app
Don't forget that React was initially built by Facebook for Facebook's problem. If your app is simple enough to be run on something that requires a smaller learning curve, don't use React. 

### Migrating and learning takes time ( and money )

Since it is a whole new paradigm, you and your team will probably not have the time to learn bunch of concepts before starting to be productive.

### Hard to understand for beginners

Infortunately, it **is not** initially aimed for beginner and simple app. I had a hard time to understand what was React and which problem it solves.

As a newbie, I also had so much pain before getting a 'Hello World' ( many hours, I think ). I really did not understood all the hype around the library. That is because I had to understand the problems that I haven't faced before and learn the purposes and how to use every tools (Webpack, Redux, react-router, ES6...) and make them work together. This is the [JavaScript fatigue](https://medium.com/@ericclemmons/javascript-fatigue-48d4011b6fc4#.5pzrqifsl).


### Server-side rendering is painful
The SEO often matters. React is really popular, because it is possible to have server-side rendering, which makes your website crawlable by Google bots. But server-side rendering but from what I heard, is still not an obvious task.


## Conclusion

React had a lot of hype from the differents community, fairly because it is powered by Facebook. Nonetheless React is a significant solution to make your app simplier. The learning curve is pretty high since there is a whole new paradigm to learn. At the end of the day, if you don't feel the need to use React, well nothing force you to use it.



