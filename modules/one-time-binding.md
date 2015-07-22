<!--
{
"name" : "one-time-binding",
"version" : "0.1",
"title" : "AngularJS one-time binding syntax",
"description": "For developers, adopting one-time bindings is really simple, and the performance gains for our apps are huge.",
"freshnessDate" : 2014-12-12,
"homepage" : "http://toddmotto.com/",
"canonicalSource" : "http://toddmotto.com/angular-one-time-binding-syntax/",
"license" : "MIT License"
}
-->

<!-- @section -->
# Overview

Angular 1.3 shipped with an awesome new performance enhancing feature - one-time binding. What does this mean for us Angular developers and the performance of our apps? A lot! For us developers, adoption is really simple, and the performance gains for our apps are huge.

Let's look at the problem really quick, starting with the `$digest` cycle. The `$digest` cycle is essentially a loop through all our bindings which checks for changes in our data and re-renders any value changes. As our apps scale, binding counts increase and our `$digest` loop's size increases. This hurts our performance when we have a large volume of bindings per application view. So what can we do to optimise this?

Thankfully, Angular 1.3 has put a lot of effort into performance, so we can utilise their new syntaxes and APIs to build faster apps.

<!-- @section -->
### What does it mean?

One-time binding is very simple, from the docs: _One-time expressions will stop recalculating once they are stable, which happens after the first digest if the expression result is a non-undefined value._

In a nut shell, when we declare a value such as `{{ foo }}` inside the DOM, once this value becomes defined, Angular will render it, unbind it from the watchers and thus reduce the volume of bindings inside the `$digest` loop. Simple!

<!-- @section -->
### The Syntax

The syntax is actually very simple, typically we're used to seeing something like this:

```html
<p>
  {{ vm.user }}
</p>
```

The new syntax adds `::` in front of any values, which declares we want one time binding:

```html
<p>
  {{ ::vm.user }}
</p>
```

Once `vm.user` becomes defined and contains a value, Angular will unbind it and any Model updates won't affect the View. A simple example is if we did this:

```html
<input type="text" ng-model="vm.user">
<p>
  {{ ::vm.user }}
</p>
```

Anything typed into the input wouldn't render the Model value out in the View, consider it a "render-once" type method rather than bind once.

<!-- @section -->
### Code examples

We already covered a simple example above using curly handlebars, we can also use it for things such as `ng-if`:

```html
<div ng-if="::vm.user.loggedIn"></div>
```

This helps us serve and maintain specific pieces of functionality on the front-end that we might want to simply destroy permanently based on a user's role or state (for example).

Let's see `ng-class`:

```html
<div ng-class="::{ loggedIn: vm.user.loggedIn }"></div>
```

Runs once, again great for initial states.

My favourite is still on the ng-repeat, instead of constructing our own navigation (for example) or using an `ng-repeat` _without_ one-time binding, we're going to be doing extra work or have our binding count higher when we really don't need it to be.

The syntax is pretty simple inside an `ng-repeat`:

```html
<ul>
  <li ng-repeat="user in ::vm.users"></li>
</ul>
```

Once the `ng-repeat` is populated, we're all set and everything is unbinded.

Syntax is pretty simple, albeit a little "weird", but after using it in production it sure helps other developers to know what's binding once.
