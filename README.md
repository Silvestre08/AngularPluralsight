# AngularPluralsight
Angular is an open source front-end framework developed and maintained by Google design for building modern, dynamic, and scalable web applications.

Or differently, angular is a framework to build websites, no matter the size, level of detail needed.

It is framework so it is prewritten code, with useful tools and structured guidelines all packaged to create web applications of any size.

Angular breaks down large project into smaller parts. It is easier to manage, maintain and resue functionality. 

Each component manages what the user sees thorugh the use of templates by mixing HTML elements with project data. Each component template can be styled with CSS.

Angular provides a way to use logic in the templates via directives (conditial, etc)/

Components are self contained chunks of functionality managing how something shows up for the user and all the logic for that piece of the application.

When some heavir work that needs to be done, something that will be used acroos the entire application (authenticicatio, notifictions) we can use what angular calls services. The service is to place logic that can be reused.

Angular uses typescript, when compiled is javascript. It allows types, cleaner code and less errors.

# Components
The foundation of every angular app. It breaks large projects into smaller more manageable pieces.
The index files holds everything together.
it is not a component but it is the base.
When the angular app first loads, this is where the user starts.

In our example, there are only six components. 
The app component, which acts as a shell or a base for all the other compoenents, and every angular app has one of these.
The we have th topbar component, which handles navigation, it is shared across component, etc...

It is up to us to choose which components to create, how to style and how they interact with the rest of the app.
Each components should be small and focused on one responsibility.

Each component has the following files:
![](doc/componentFiles.PNG)

The first component to look at is the app component, which is common to all Angular apps and act as the foundation of our app and the inclusion of other components.
Each component is dividend in 3 sections:

1. The class itself (where we define core functionality of the )
1. The metadata of the component. Information angular need to connect all pieces.
The selector is how we identify the name of a custom html element that will be used inside a template:
![](doc/componentDecorator.PNG)
We are identifying where this component will appear in our app. Example in the index.html file, that lives at the root of the project, we have the same app root tag:

![](doc/approot.PNG)

So whatever happens inside the app component, all the html is oing to be dynamically generated in this exact location in the HTML code. 

How it is structured and how it looks are configured in the next two pieces of metadata: the template url (the html code for this component) and style url (css code for this component).
Standalone true means we don't have to use Angular or Ng modules to define template dependencies.
The last piece defines the dependencies we want to use inside our templates. In this case we want to have a top bar always available so it is on our template. We also define routing. Routing should be cheked here.
1. The imports of the class (like usings in c#).

# Templates
Templates are all about the user, what they see. This is where HTML lives. It decides how the app looks like, but on a smaller scale. It is tied to the component. Example of our app component
```
<app-top-bar></app-top-bar>

<section class="main-content">
  <router-outlet></router-outlet>
</section>
```

