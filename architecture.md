# Modules
Every angular application must have at least one module, the root module, mostly
called `AppModule` . In most of our applications this is possibly the only
module you will ever use. This file can be found at `src/app/app.component.ts` and it looks like:

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {}
```

**@NGMOdule:** The tag `@NgModule` is called a decorator which we will discuss in detail later.
This decorator takes an object with different properties, such as `declarations`,
`imports`, `providers` and `bootstrap`. We will mostly be working
with the `providers` and `declarations`.

**providers:** Services are values, functions, or features that your application needs. We will come back to a more detailed description about services later. All our services are usually registered with the application by adding them to this section. By doing this you make the services available for use in the entire application.

**declarations** our components are usually declared in this section. In our case
the generated `AppComponent` is declared here.

## Angular modules are not Javascript modules 
OK, as we had learnt earlier on JavaScript has modules too but it is different
from Angular modules. An NgModule is decorated by `@NgModule` and its is
completely unrelated to the JavaScript modules.

TODO: we must have a previous section explaining what javascript modules are


# Components
Components control a section of the whole page that the user sees, so let's put this
into context. Go ahead and have a look at the YouTube screenshot below:

TODO: add youtube video screenshot

From the screenshot above we can see that we can structure the page in different sections:

1. section for playing the video
2. section containing video description
3. section for adding comment and displaying comments
4. section for suggesting other video

Each of the sections is modeled as single components that are stitched together to make up the whole page.

### What components look like
```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-suggestions',
  templateUrl: './suggestions.component.html',
  styleUrls: ['./suggestions.component.css']
})
export class SuggestionsComponent {

  constructor() { }

  playVideo(video){
    ...
  }

}
```

We start by importing various libraries we need from the Angular core. In this case 
we have `Component` and `OnInit`. 

The `@Component` decorator then tells us that the `SuggestionsComponent` class declared below 
it is a component. This decorator has metadata about the `selector` , `templateUrl`, 
`styleUrls`, which we will discuss the details later

Following the decorator is the class called `SuggestionsComponent`, which has some
functions in it:

 -  `constructor()`: used for initialization when instantiating an object out of the 
 class much like ruby class initializers
- `playVideo()`: is a function that can be called from the template

### Review the structure of the component
Lets have a look at the component, which was generated for us at
`src/app/application.component.ts` and identify everything from top to bottom, so that
you are able to identify each part. If you get stuck and unable to understand each part, go back
and re-read the previous sections.


## Metadata
In TypeScript, by using a decorator you are able to attach metadata to a class in order to attach additional information for example about the role the class, what it depends on and where the styling is. 

Look at the code below and you can see that we have a `@Component`, which is the decorator. The decorator holds a JavaScript object (between `{`and `}`, remember?), which carries additional information about the class `SuggestionsComponent`. 

```typescript
@Component({
  selector: 'app-suggestions',
  templateUrl: './suggestions.component.html',
  styleUrls: ['./suggestions.component.css']
})

export class SuggestionsComponent {
...
}

```
Basically the decorator is used to give us a brief summary about the class, much like
hospital wrist bands worn by patients in hospitals.

![Hospital wrist band](http://www.globalnerdy.com/wordpress/wp-content/uploads/2016/11/hospital-wristband-2.jpg)


```typescript
@Component({
  selector: 'app-suggestions',
  templateUrl: './suggestions.component.html',
  styleUrls: ['./suggestions.component.css']
})

export class SuggestionsComponent {
...
}

```
In the code above you see that the metadata has three properties:

- `selector` : This is a css selector that tells angular where to place
this particlar component in the page. To use this component we will later in
out view put in something like `<app-suggestions></app-suggestions>`
- `templateUrl`: Tells angular where the view for this  component is located. In
out code above its at `./suggestions.component.html`
- `styleUrls`: By now you can guess what this does, tells angular where our
stylesheet for the component is

Go to `src/index.html`, can you identify how the selector that has been used? Make
sure you get this right before moving on.


## Templates
The template can be said to be the view of the component. By now you should be
able to tell that a component has its own template, stylesheet and typescript
file. Templates are much like plain HTML, but with a few extra
supermagical abilities and extra tags.

Look closely at the syntax in the code below and you will see a few additions like `*ngFor`, `(click)`, `[video]`
`*ngIf`. Don't sweat over all these new weird-looking things, we will cover them later.

```html
<h2>Suggested Videos</h2>

<p><i>Click on a video to play</i></p>
<ul>
  <li *ngFor="let video of videos" (click)="selectVideo(video)">
    {{video.name}}
  </li>
</ul>

<video-detail *ngIf="selectedVideo" [video]="selectedVideo"></video-detail>
```

### Review the template
Now head over to `application.component.html` and have a look
at the syntax there. Remove all the unordered list items, as well as the image, and save the changes to see the changes in the browser.

## The relationship between the template and the component
- the templates are usually tied to a particular component
- the component identifies their respective template through the decorator
- data can flow from the component to the template, from the template to
the component or both ways

TODO: diagram showing template and component


### Review: Bringing together components and templates

Let's generate a component called `comment`:
```shell
$ng generate component comment
# or a shorter version
$ng g c comment
```
From the files we have just generated, let's identify what we have learnt so far:
- where is the component file?
- what is the decorator, and what information does the decorator have?
- where is the template file?
- add a paragraph with your name in the template
- how do we add our component to a view?
- can we add our component to the index file?
- is it possible to add the component to the `app.component.htm` file?


## Summarizing templates and components
![template component relationship summary](https://angular.io/generated/images/guide/architecture/component-tree.png)

TODO: add explanation

## Data binding
Remember when we said that information is able to flow from the template to the component
and vice versa? Let's talk about how to do this by using data binding.

### from template to the component
Data can flow from our template to the component for example when a user clicks
on a particlar video the component can receive information about the clicked video
Whatever the user does is usually interpreted as an event, this event needs a
function that receives it at the component and perfoms a function

Hint: this usually uses `()` eg `(click)="doSomething()"` where `doSomething()`
is a function in the matching component

Add this to our template `application.compontent.html`
```html
<button (click)="handleClick()">Console log me</button>
```

Add this to our matching component inside our class(we should be able to match
components to templates at this point)
```typescript

@Component({
/*............*/
})
export class AppComponent {
  title = 'app';

  handleClick(){
    console.log('Someone clicked on button on the template :-)')
  }
}
```

Lets try this in the browser, open the app and the developer tools  to see the results.
What did you see? Click on the button, is anything printed on the console?

### from the component to the templates
Information flows in the opposite direction from what we discussed earlier. In
this case we have some information in the compontent that we want to display
in the template. Lets create a video on our component

```typescript
@Component({
/*............*/
})
export class AppComponent {
  title = 'app';
  video = {
    title: 'Despacito',
    views: 2,
    liked: true
  }

  handleClick(){
    console.log('Someone clicked on button on the template :-)')
  }
}
```
Now that we have the video on our component, how do we display it in the template?
We can use the squiggly brackets `{{}}` to achieve this

In our view lets add this below the button

```html
<p>Video title: {{video.title}}</p>
<p>Views: {{video.views}}</p>
<p> Liked: {{video.liked}}
```
This is commonly known as string iterpolation- does this name ring a bell?

NOTE: I am skipping property binding to minimise the number of things
to be learnt in one day but they will come to realise its also present

### two way data binding
In the previous two scenarios data can only flow in one direction, in one way.
In this case, data flows from compontent to template and from template to
component.

This is usally done by using `[()]`

In order to demo this we need to import anglar form module into our
`app.module.ts`, remember how we did our imports?
```typescript
  import { FormsModule } from '@angular/forms';

  [...]

  @NgModule({
    imports: [
      [...]
      FormsModule
    ],
    [...]
  })
```


Now add this to our template
```html
 <p>Video name: {{video.name}}</p> <!-- remember this {{}} gets data from compontent -->
 <input [(ngModel)] = "video.name"/>
```
type into the input field, what happens?


![data binding](https://angular.io/generated/images/guide/architecture/databinding.png)

## Directives

Directives usually change the browser DOM, can change elements in the current
page
There are build in directives that we will be using more so lets have a look
at them

- `*ngFor`: This is used for looping over a collection of data.
In the code shown below we have a collection of videos on the component so we
loop over the videos and for each video we display its name

```html
<ul>
  <li *ngFor="let video of vidoes">
    Name: {{video.name}}
    Liked: {{video.liked}}
  </li>
</ul>
```

Add the code above to your template, now we need the collection of videos in
in our component for this to work. In our component lets add some two videos

```typescript
export class SuggestionsComponent {
 videos = [
    {name: 'video one', liked: true},
    {name: 'video two', liked: false}
 ]

  constructor() { }

  /*.........*/

  }


```

- `*ngIf`: Only renders that section if a certain condition is true

```html
<p *ngIf="isShown">This should be hidden or shown</p>
```

We expect the paragraph above to be shown only if the value of `isShown` is true
if the value of `isShown` is false then the paragraph should not be shown on the
page

We can define the value of `isShown` in the component

```typescript
export class SuggestionsComponent {
 videos = [
    {name: 'video one', liked: true},
    {name: 'video two', liked: false}
 ]

 isShown: boolean = true

  constructor() { }
```
Since we have set the value of `isShown` to true will the paragraph be shown or
not shown on the template? (answer this, try is out before moving on)

Change the value of `isShown` to `false`. What will happen?

## Services
Lets simplify the definition of services to 'that section of our app that
brings us data for use'. The source is most likely an external API

Services can also do some things like validating user input as they type them in

TODO: gif of user validation in IMS


## Dependancy injection
We will be using dependancy injection to give our components the services they
need. Lets say we have a service that gives us back a collection of videos from
youtube, in order to use this service we have to inject it into our component

```typescript
constructor(private videoService: VideoService){}

playVideo(){
 this.videoService.play(video)
}
```

This is how dependancy injection is done in angular

`constructor(private videoService: VideoService){}`
- `private` - shows that whatever we are declaring is private to this class
- `videoService` - the local variable name that we can use within the class.
this is usually preceded by the `this` keyword for example

  ` this.videoService.play(video)`

- `VideoService` - the service that we are instanciating to `videoService` local
variable. This must be imported first
