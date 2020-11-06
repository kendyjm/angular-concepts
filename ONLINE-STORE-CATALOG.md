# Getting started : Online store catalog

See it live: <https://stackblitz.com/edit/angular-online-store-catalog>

- *ngFor
- *ngIf
- Interpolation {{ }}
- Property binding [ ]
- Event binding ( )

## Directives

`<div *ngFor="let product of products"> </div>` With *ngFor, the `<div>` repeats for each product in the list.

_*ngFor_ is a "**structural directive**". Structural directives shape or reshape the DOM's structure, typically by adding, removing, and manipulating the elements to which they are attached. **Directives with an asterisk, \*, are structural directives.**

```html
## Angular only creates the <p> element if the current product has a description.
  <p *ngIf="product.description">
    Description: {{ product.description }}
  </p>
```

## Interpolation

```html
  <h3>
      {{ product.name }}
  </h3>
```

To display the names of the products, use the interpolation syntax {{ }}.
Interpolation {{ }} lets you render the property value as text

## Property binding

```html
    <a [title]="product.name + ' details'">
      {{ product.name }}
    </a>
```

Property binding **[ ]** lets you use the property value in a template expression.

## [Input](https://angular.io/start#input)

 The **@Input()** decorator indicates that the property value passes in from the component's parent, the product list component.

```typescript
// child component
import { Input } from '@angular/core';
export class ProductAlertsComponent implements OnInit {
  //  The @Input() decorator indicates that the property value passes in from the component's parent, the product list component.
  @Input() product?;

// parent template
<div *ngFor="let productItem of products">
    <app-product-alerts [product]="productItem"> </app-product-alerts>
</div>
```

## [Output](https://angular.io/start#output)

```typescript
// child component
import { Output, EventEmitter } from '@angular/core';
export class ProductAlertsComponent implements OnInit {
// This allows the product alert component to emit an event when the value of the notify property changes.
  @Output() notify = new EventEmitter();

// child template
<button (click)="notify.emit()">Notify Me</button>

// parent template
<div *ngFor="let productItem of products">
    <app-product-alerts [product]="productItem" (notify)="onNotifyAlert()"> </app-product-alerts>
</div>

// parent component
onNotifyAlert() {
    window.alert("You will be notified when the product goes on sale");
}

```

## [Navigation](https://angular.io/start/start-routing)

This guide shows you how to use Angular routing to give the user in-app navigation. In a single-page app, instead of loading new pages, you show different components and data to the user based on where the user is in the application.

### [Registering a route](https://angular.io/start/start-routing#registering-a-route)


```typescript
// In app.module.ts, add a route for product details, with a path of products/:productId and ProductDetailsComponent for the component.
    RouterModule.forRoot([
      { path: '', component: ProductListComponent },
      { path: 'products/:productId', component: ProductDetailsComponent }, // this is it
    ])

// template
<div *ngFor="let product of products; index as productId">
    <a [title]="product.name + ' details'" [routerLink]="['/products', productId]">
      {{ product.name }}
    </a>
```

**The RouterLink directive gives the router control over the anchor element.**
In this case, the route, or URL, contains one fixed segment, /products, while the final segment is variable, inserting the id property of the current product. For example, the URL for a product with an id of 1 will be similar to https://getting-started-myfork.stackblitz.io/products/1

### [Using route information](https://angular.io/start/start-routing#using-route-information)

The product details component handles the display of each product.
The **Angular Router** displays components based on the browser's URL and your defined routes.

```typescript
// In target component
import { ActivatedRoute } from '@angular/router';
export class ProductDetailsComponent implements OnInit {
  product;
  constructor(
    private route: ActivatedRoute,
  ) { }

  //  subscribe to route parameters and fetch the product based on the productId.
  ngOnInit() {
  this.route.paramMap.subscribe(params => {
    this.product = products[+params.get('productId')];
  });
}
```

By injecting the **ActivatedRoute**, you are configuring the component to use a _service_. The [Managing Data](https://angular.io/start/start-data) page covers services in more detail.

For more information about the Angular Router, see [Routing & Navigation](https://angular.io/guide/router)

## Managing data

This page guides you through creating the shopping cart in three phases:

- Update the product details view to include a "Buy" button, which adds the current product to a list of products that a cart service manages.
- Add a cart component, which displays the items in the cart.
- Add a shipping component, which retrieves shipping prices for the items in the cart by using Angular's HttpClient to retrieve shipping data from a .json file.

### Services

In Angular, a service is an instance of a class that you can **make available to any part of your application** using Angular's dependency injection system.

**Services are the place where you share data between parts of your application**. For the online store, the cart service is where you store your cart data and methods.

See decorator **@Injectable()**

```typescript
// src/app/cart.service.ts
import { Injectable } from '@angular/core';
@Injectable({
  providedIn: 'root'
})
export class CartService {
  items = [];
  constructor() {}
  addToCart(product) {    this.items.push(product);  }
  getItems() {    return this.items;  }
  clearCart() {    this.items = [];    return this.items;
  }  
}

// a component
export class CartComponent implements OnInit {
  items;
  constructor(private cartService: CartService) {}

  ngOnInit() {
    this.items = this.cartService.getItems();
    console.log("items:" + this.items);
  }
}
```

### HTTPClient

The Angular HTTP client, HttpClient, is a built-in way to fetch data from external APIs and provide them to your app as a **stream**.

- Open app.module.ts. This file contains imports and functionality that is available to the entire app.
- Import [HttpClientModule](https://angular.io/api/common/http/HttpClientModule) from the @angular/common/http package at the top of the file with the other imports.
- Add HttpClientModule to the AppModule @NgModule() imports array to register Angular's HttpClient providers globally.

For more information about Angular's HttpClient, see the [Client-Server Interaction guide](https://angular.io/guide/http).

```typescript
//service
  constructor(
    private http: HttpClient
  ) {}
  getShippingPrices() {    return this.http.get('/assets/shipping.json');  }  

//component
  ngOnInit() {
    this.shippingCosts = this.cartService.getShippingPrices();
  }  

// template, NOTE The async pipe
<div class="shipping-item" *ngFor="let shipping of shippingCosts | async">
  <span>{{ shipping.type }}</span>
  <span>{{ shipping.price | currency }}</span>
</div>
```

The async pipe returns the latest value from a stream of data and continues to do so for the life of a given component. When Angular destroys that component, the async pipe automatically stops. For detailed information about the async pipe, see the [AsyncPipe API documentation](https://angular.io/api/common/AsyncPipe).

## Forms

There are two parts to an Angular Reactive form:

- the **objects** that live in the component to **store and manage** the form, 
- the **visualization** of the form that lives in the **template**.

### [Define the checkout form model](https://angular.io/start/start-forms#define-the-checkout-form-model)

**Defined in the component class, the form model is the source of truth for the status of the form.**

First, set up the checkout form model.

```typescript
// COMPONENT
// Angular's FormBuilder service provides convenient methods for generating controls. 
import { FormBuilder } from '@angular/forms';

export class CartComponent implements OnInit {
  // define the checkoutForm property to store the form model.
  checkoutForm;
  constructor(
    private formBuilder: FormBuilder,
  ) {  
    // To gather the user's name and address, set the checkoutForm property with a form model containing name and address fields, using the FormBuilder group() method. Add this between the curly braces, {}, of the constructor.
    this.checkoutForm = this.formBuilder.group({
      name: '',
      address: ''
    });

  // For the checkout process, users need to submit their name and address. When they submit their order, the form should reset and the cart should clear.  
  onSubmit(customerData) {
    // Process checkout data here
    this.items = this.cartService.clearCart();
    this.checkoutForm.reset();

    // and submit the data to an external server
    console.warn('Your order has been submitted', customerData);
  }
}
```

## [Create the checkout form](https://angular.io/start/start-forms#create-the-checkout-form)

Now that you've defined the form model in the component class, you need a checkout form to reflect the model in the view.

```typescript
// TEMPLATE
<form [formGroup]="checkoutForm" (ngSubmit)="onSubmit(checkoutForm.value)">
  <div>
    <label for="name">Name</label>
    <input id="name" type="text" formControlName="name">
  </div>
  <div>
    <label for="address">Address</label>
    <input id="address" type="text" formControlName="address">
  </div>
  <button class="button" type="submit">Purchase</button>
</form>
```

Note the use of **ngSubmit**, for more details see [Submitting And Resetting](https://codecraft.tv/courses/angular/forms/submitting-and-resetting/)

- _submit_ triggers a post call to browser URL/server.
- _ngSubmit_ calls local angular component to do something useful like form validation BEFORE postback to browser url/server. It can be used to hijack form submission or control submit process.
