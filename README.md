# Angular Concepts

## [Architecture](https://angular.io/guide/architecture)

![architecture](assets/architecture.png)

- Together, a _component_ and _template_ define an _Angular view_.
  - A decorator @Component on a component class adds the metadata, including a pointer to the associated template.
  - _Directives_ and _binding markup_ in a component's template modify views based on program data and logic.
- The dependency injector provides _services_ to a component, such as the router service that lets you define navigation among views.

### [Components](https://angular.io/guide/architecture#components)

Each **component** defines a class that **contains application data and logic**, and is _associated_ with an HTML template that defines a **view** to be displayed in a target environment.

The **_@Component()_** decorator identifies the class immediately below it as a component, and provides the template and related component-specific metadata.

See [Component interaction](https://angular.io/guide/component-interaction) for more information about passing data from a parent to child component, intercepting and acting upon a value from the parent, and detecting and acting on changes to input property values.

Decorators are functions that modify JavaScript classes, [lean more](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841#.x5c2ndtx0).

### [Templates, directives, and data binding](https://angular.io/guide/architecture#templates-directives-and-data-binding)

A _template_ combines HTML with Angular markup that can modify HTML elements before they are displayed.

_Template directives_ provide program logic, and binding markup connects your application data and the DOM. There are two types of data binding:

- _**Event binding**_ lets your app respond to user input in the target environment by updating your application data.
- _**Property binding**_ lets you interpolate values that are computed from your application data into the HTML.

For a more details, see [Introduction to components](https://angular.io/guide/architecture-components).

### [Services and dependency injection](https://angular.io/guide/architecture#services-and-dependency-injection)

- For data or logic that isn't associated with a specific view, and that you want to share across components, you create a service class. A service class definition is immediately preceded by the _**@Injectable()**_ decorator.

- Dependency injection (DI) lets you keep your **component classes** lean and efficient. They **don't fetch data from the server, validate user input, or log directly to the console**; they **delegate such tasks to services**.

For more details, see [Introduction to services and DI](https://angular.io/guide/architecture-services)

### [Routing](https://angular.io/guide/architecture#routing)

The Angular Router NgModule provides a _service_ that lets you define a navigation path among the different application states and view hierarchies in your app.

The router interprets a link URL according to your app's view navigation rules and data state. You can navigate to new views when the user clicks a button or selects from a drop box, or in response to some other stimulus from any source. The router logs activity in the browser's history, so the back and forward buttons work as well.
To define navigation rules, you **associate navigation paths with your components**.

For a more details, see [Routing and navigation](https://angular.io/guide/router).

## Next steps

- [This tutorial](ONLINE-STORE-CATALOG.md) introduces you to the essentials of Angular by walking you through a simple e-commerce site with a catalog, shopping cart, and check-out form.
