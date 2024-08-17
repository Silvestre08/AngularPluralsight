# AngularPluralsight

Angular is an open source front-end framework developed and maintained by Google designed for building modern, dynamic, and scalable web applications.
Or differently, angular is a framework to build websites, no matter the size and level of detail needed.
It is framework so it is pre-written code, with useful tools and structured guidelines all packaged to create web applications of any size.
Angular breaks down large projects into smaller parts that are easier to manage, maintain and with functionality that can be reused. These parts are called _Components_.
Each component manages what the user sees through the use of templates, by mixing HTML elements with project data. Each component template can be styled with CSS.
Angular provides a way to use logic in the templates via directives (conditial, etc).
Components are self-contained chunks of functionality managing how something shows up for the user and all the logic for that piece of the application.
When some heavier work that needs to be done, something that will be used across the entire application (authentication, notifictions) we can use what angular calls services. The service is the place to put logic that can be reused.
Angular uses typescript that when compiled is is transformed in javascript. It allows types, cleaner code and less errors.

# Components

They are the foundation of every angular app. They break large projects into smaller more manageable pieces.
The _Index_ file holds everything together. It is not a component but it is the base.
When the angular app first loads, this is where the user starts.
In this example app, there are only six components.
The app component, which acts as a shell or a base for all the other compoenents, and every angular app has one of these.
Then we have the topbar component, which handles navigation, it is shared across component, etc...
It is up to us to choose which components to create, how to style and how they interact with the rest of the app.
Each components should be small and focused on one responsibility.

Each component has the following files:
![](doc/componentFiles.PNG)

The first component to look at is the app component, which is common to all Angular apps and act as the foundation of our app and the inclusion of other components.
Each component is dividend in 3 sections:

1. The class itself (where we define core functionality of the component).
1. The metadata of the component. Information angulars need to connect all pieces.
   The selector is how we identify the name of a custom html element that will be used inside a template:
   ![](doc/componentDecorator.PNG)
   We are identifying where this component will appear in our app. Example in the index.html file, that lives at the root of the project, we have the same app root tag:

![](doc/approot.PNG)

So whatever happens inside the app component, all the html is going to be dynamically generated in this exact location in the HTML code.
How it is structured and how it looks are configured in the next two pieces of metadata: the template url (the html code for this component) and style url (css code for this component).
Standalone equals true means that we don't have to use Angular or Ng modules to define template dependencies.
The last piece defines the dependencies we want to use inside our templates. In this case we want to have a top bar always available so it is on our template. We also define routing. Routing should be cheked here.

1. The imports of the class (like usings in c#).

# Templates

Templates are all about the user, what they see. This is where HTML lives. It decides how the app looks like, but on a smaller scale. It is tied to the component. Example of our app component html:

```
<app-top-bar></app-top-bar>

<section class="main-content">
  <router-outlet></router-outlet>
</section>
```

This is the app main component. Top bar always visible. It is going to be the selector of content that hold the navigation for the app.
The _<router-outlet>_ element is the built-in router destination we imported in the app component.
This is where all the components that are controlled by the navigation will be displayed, like shopping kart, etc.

If we inspect now the top bar component, we see a few differences.

```
import { Component, computed, inject } from '@angular/core';
import { RouterLink } from '@angular/router';
import { CartService } from '../services/cart.service';
import { NgIf } from '@angular/common';

@Component({
    selector: 'app-top-bar',
    templateUrl: './top-bar.component.html',
    styleUrls: ['./top-bar.component.css'],
    standalone: true,
    imports: [RouterLink, NgIf]
})
export class TopBarComponent {
    cart = inject(CartService); // Inject the CartService.

    cartCount = this.cart.cartCount;
}

```

1. The files for the template and styles are now different.
1. We have a router link related dependency as well.

It is normal differences between components, because components have a single responsibility.
Looking at the top bar template, we can see that navigation related functionality stands out, by seeing several [routerLinks]: one for the shop page, another for the contact page, etc:

```
<header class="site-header">
    <div class="site-header-wrapper">
      <h1><a [routerLink]="['/']"><img src="./assets/img/png//logo.png" alt="Bethany's Pie Shop Logo" /></a></h1>
      <nav>
        <ul>
          <li><a [routerLink]="['shop']">Shop</a></li>
          <li><a [routerLink]="['contact']">Contact</a></li>
        </ul>
      </nav>
      <div class="header-icons">
        <ul>
          <li class="cart-icon">
            <a [routerLink]="['cart']"><img class="cart-img" src="./assets/img/png//cart-icon.png" alt="shopping cart icon"/></a>
            <span *ngIf="cartCount()" class="cart-count">{{cartCount()}}</span>
          </li>
        </ul>
      </div>
    </div>
</header>

```

Everything in this template is vanilla HTML code except for this router link.
Going back to the app component template, we see the router outlet is the placeholder in the template where the components will be shown, rotated in and out and the [routerLink] is how we tell angular which component we want to show up.
How we define the routes? Routes are defined ahead of time in the _app.routes.ts_. We are mapping routes here with a path, which is sufficient for now.
How is this routes file loaded? This file is configured in a file called _app.config.ts_ that contains not only the routes but the configuration of our app.
App.config.ts is loaded by the file _main.ts_.
_main.ts_ is considered the entry point of the application.

# Directives, pipes and Data binding

Directives in angular are special instructions that we can place directly inside the HTML elements to modify the behavior or appearance of those elements.
They are a way for us to accomplish common programming, like controlling the flow but inside the HTML template.
With directives we can conditionally control visibility of elements, apply custom class names, CSS styles, base don different conditions.
We can see a first example in the product-list component: the _ng-for_ directive.
We iterate over a collection of products, defined in the _product.ts_ folder and display each one in the page:

```
<h2>Pies and Cakes</h2>
<div class="gallery-wrapper">
    <div *ngFor="let product of products" class="pie-item">
        <img [src]="product.image" alt="image of {{ product.name }}" />
        <div class="pie-info">
            <h4>{{ product.name | titlecase }}</h4>
            <p>{{ product.price | currency}}</p>
        </div>
        <div class="add-to-cart">
            <p><a (click)="addToCart(product)">+ Add to cart</a></p>
        </div>
    </div>
</div>
```

In our example we used a static file, real world scenario would come from a database. For each product we show a name, price, and add an event handler, mapped to the add to cart function in the component file. The product is a type model, defined as a typescript interface.
This mechanism of getting the actual data into the html template is called data binding.
We surround the models property name under curly braces when we want to render the value as text. This is one way Angular does data binding, called interpolation.
When we want to use the data as a property in HTML like a class id or as a src for an image we use square brackets (property binding).
Like we have said, we handke events, like buttons clicks to add the pies to the cart. This is called event binding.

Pipes is another feature of Angular that allows the transformation of data in our templates. they make it easier to modify the appearance or behavior in the template.
Angular has a lot of built in pipes. Examples in our template:

```
<p>{{ product.price | currency}}</p>
```

We are showing here the product price, followed by the pipe char and we specify the pipe currency. This tells angular to display the price in a currency format.

# Services

We need to share functionality across the applications, functionality that is cross component, like adding items to the cart from a prodcut list page.
We also need to interact with a cart when we finalize an order.
Services is what is used in angular to share code and data between components and other parts of the application.
Services can be used for:

1. Fetch data from APIs
1. Manage authentication
1. Business logic
1. Custom functionality shared across the app.

Let's inpect our CartService:

```
import { HttpClient } from '@angular/common/http';
import { Product } from '../models/products';
import { Injectable, inject, signal } from '@angular/core';
import { ShippingPrice } from '../models/shippingPrice';

@Injectable({
  providedIn: 'root' // This service is provided in the root injector for your app and should be available globally.
})
export class CartService {
  private http = inject(HttpClient); // Inject HttpClient to make HTTP requests.
  items: Product[] = []; // Array to hold the items in the cart.
  cartCount = signal<number>(0); //use signal to create a reactive variable

  // Method to add a product to the cart.
  addToCart(product: Product) {
    this.items.push(product); // Add the product to the items array.
    this.cartCount.set(this.items.length); // Update the cart count signal.
  }

  // Method to get the items in the cart.
  getItems() {
    return this.items;
  }

  // Method to clear the cart.
  clearCart() {
    this.items = []; // Empty the items array.
    this.cartCount.set(0); // clear the cart count signal.
    return this.items;
  }

  // Method to get the shipping prices from a JSON file.
  getShippingPrices() {
    return this.http.get<ShippingPrice[]>('./assets/shipping.json');
  }

  // Method to get the subtotal of the items in the cart.
  getSubtotal() {
    return this.items.reduce((sum, item) => sum + item.price , 0); // Use reduce to sum up the prices of the items in the cart.
  }
```
