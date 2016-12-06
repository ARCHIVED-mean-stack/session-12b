#MEAN Session 12

##Angular 2 Sample

- uses import (modules) instead of `<script ... >`
- uses typescript (.ts) files
- uses Classes
- no angular.module() in the traditional sense

Note: "@" indicates a scoped package. 
e.g. `import { NgModule } from '@angular/core';`
`{ ... }` curly brackets indicate a single method from a module

[Angular 2 packages](https://angular.io/docs/ts/latest/guide/npm-packages.html)


##ES6 Modules

Not yet supported by all browsers. One solution that is common:

* Webpack for modules
* Babel for ES6

###Babel es6 Conversion

`https://babeljs.io` 

Try it out:

```js
const box = document.querySelector('.box');
  box.addEventListener('click', function() {

    let first = 'opening';
    let second = 'open';

    if(this.classList.contains(first)){
      [first, second] = [second, first];
    }
    this.classList.toggle(first);
    setTimeout(() => {
      this.classList.toggle(second);
    }, 500);
  });
```


##Webpack

`$ mkdir es6modules`
`$ cd es6modules`
`$ touch app.js`

- getting modules: npm install with the following payload:

```json
{
  "name": "es6modules",
  "version": "1.0.0",
  "description": "",
  "main": "app.js",
  "scripts": {
    
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "flickity": "^1.2.1",
    "insane": "^2.5.0",
    "jquery": "^3.0.0",
    "jsonp": "^0.2.0",
    "lodash": "^4.13.1"
  },
  "devDependencies": {

  }
}
```

index.html

```html
<!DOCTYPE html>
<html>

<head>
    <title>Recipes</title>
</head>

<body>

</body>
<script src="app.js"></script>

</html>
```

In app.js:

```js
import { uniq } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
```

- Error: import not supported

###Babel

1. First Install your dependencies:

```bash
$ npm install webpack@beta babel-loader babel-core babel-preset-es2015-native-modules --save-dev
```

2. Create a `webpack.config.js` file:

```js
const webpack = require('webpack');

const nodeEnv = process.env.NODE_ENV || 'production';

module.exports = {
  devtool: 'source-map',
  entry: {
    filename: './app.js'
  },
  output: {
    filename: '_build/bundle.js'
  },
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
          presets: ['es2015-native-modules']
        }
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false
      },
      output: {
        comments: false
      },
      sourceMap: true
    }),
    new webpack.DefinePlugin({
      'process.env': { NODE_ENV: JSON.stringify(nodeEnv) }
    })
  ]
};
```

3. Setup the build npm script in `package.json`:

```json
  "scripts": {
    "build": "webpack --progress --watch"
  },
```

`$ npm run build`

- change script src in index.html to point to `_build/bundle.js`

- use lodash in app.js

```js
import { uniq } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';

const ages = [2, 34, 6, 1, 86, 34];

console.log(uniq(ages));
```

Note that Webpack runs on save and uses js mapping in the browser console


###Roll Your Own Module 

`$ mkdir mymodule`
`$ touch config.js`

```js
const apikey = '123abc';
// default 
export default apikey;
```

default and named exports (for methods and variables)

```js
import { uniq } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import apikey from './mymodule/config';
```

`console.log(apikey)`

Default exports can be renamed on import. 

Named export:

```js
// named
export const apikey = '123abc';
// default 
// export default apikey;
```

Check terminal for webpack message. 

Named exports cannot be renamed on import.

We need to import it as follows:

`import { apikey } from './mymodule/config';`

e.g.

```js
import { uniq } from 'lodash';
import insane from 'insane';
import jsonp from 'jsonp';
import { apikey } from './mymodule/config'
```


##Classes

- not new to JS - just a new syntax

- prototypal inheritance review

`classes.html`

In browser console:

`> const names = ['hamburger', 'fries']`
`> names.join()`
`> names.pop()`
`> Array.prototype` and examine the prototype
`> lasagna.eat()`
`> lasagna` and examine the prototype

Declaration and mandatory constructor method:

```js
class Recipe {
    constructor(name, country){
        this.name = name;
        this.country = country;
    }
}

const lasagna = new Recipe('Lasagna', 'Italy');
const pho = new Recipe('Pho', 'Vietnam');
```

`> lasagna`

Creating methods inside a class (no comma)

```js
class Recipe {
    constructor(name, country){
        this.name = name;
        this.country = country;
    }
    cook(){
        console.log(`I like to cook ${this.name}`);
    }
    eat(){
        console.log(`Yummy!`);
    }
}
const lasagna = new Recipe('Lasagna', 'Italy');
const pho = new Recipe('Pho', 'Vietnam');

```

`> lasagna.cook` returns the function

`> lasagna.cook()` to run the function

Getters, setters, and statics methods

```js
class Recipe {
    constructor(name, country){
        this.name = name;
        this.country = country;
    }
    cook(){
        console.log(`I like to cook ${this.name}`);
    }
    eat(){
        console.log(`Yummy!`);
    }
    get description() {
      return `${this.name} is from ${this.country}.`;
    }
    get description() {
      return `${this.name} is from ${this.country}.`;
    }
    set ingredients(value) {
      this.ingredient = value.trim();
    }
    get ingredients() {
      return this.ingredient.toUpperCase();
    }
}
const lasagna = new Recipe('Lasagna', 'Italy');
const pho = new Recipe('Pho', 'Vietnam');
```

`> lasagna.description`
`> lasagna.ingredients = '  pasta  '`
`> lasagna.ingredients`


##TypeScript

A superset of the JavaScript programming language that adds the concept of static typing to the core features of JavaScript.

###Babel vs TypeScript

Playgrounds:
`http://www.typescriptlang.org/play/index.html`

Note the example using classes.

`https://babeljs.io/repl/`


Run this script in both Babel and TypeScript

```js
const box = document.querySelector('.box');
  box.addEventListener('click', function() {

    let first = 'opening';
    let second = 'open';

    if(this.classList.contains(first)){
      [first, second] = [second, first];
    }
    this.classList.toggle(first);
    setTimeout(() => {
      this.classList.toggle(second);
    }, 500);
  });
```

Note: Type errors do not prevent JavaScript emit.

To see variable type checking in action:

```js
var foo = 123;
foo = '456';
```

or 

```
function speak(value: string){
  console.log(value);
}
var greeted = 'world';
var greeting = 'hello';
var whatToSay = greeting + greeted;
speak(whatToSay)
```

Typescript will catch errors before you run them. Babel will not.


###Running Typescript

`npm install -g typescript@next`

`tsc` to verify

Make directory `typescript` and open in VSCode

index.html:

```html
<!doctype html>

<head>
    <title>TypeScript</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css">
</head>

<body>
    <div id="container" class="container">
    </div>
    <script type="text/javascript" src="app.js"></script>
</body>

</html>
```

Create `app.ts`

`$ tsc ./app.ts` or ``tsc -w ./app.ts` then 

```js
let myString: string = "Hello world"
string = 12;
```

`$ tsc` to see the configuration options.

`tsconfig.json`

```
{
    "compilerOptions": {
        "target": "es5"
    }
}
```

####Template Strings

```js
var container = document.getElementById('container');
var todo = {
    id: 123,
    name: 'Pick up drycleaning',
    completed: true
}
container.innerHTML = `
<div todo='${todo.id}' class="list-group-item}">
    <i class="${ todo.completed ? "" : "hidden" } text-success glyphicon glyphicon-ok"></i>
    <span class="name">${todo.name}</span>
</div>
`
```

####For in loops

```js
var array = [
    "Pick up drycleaning",
    "Clean Batcave",
    "Save Gotham"
];
// try for in first
// need to grab the value via the index
// for (var index in array) {
//     var value = array[index];
//     console.log(`${index}: ${value}`);
// }
for (var value of array) {
    console.log(`${value}`);
}
```

####this rebinding with fat arrows

old:

```js
var container = document.getElementById('container');
function Counter(el) {
    this.count = 0;
    el.innerHTML = this.count;
    el.addEventListener('click', 
        function(){
          this.count += 1;
          el.innerHTML = this.count;
    })
}
new Counter(container);
```
NaN

```js
var container = document.getElementById('container');
function Counter(el) {
    this.count = 0;
    el.innerHTML = this.count;

    let _this = this;

    el.addEventListener('click',
        function() {
            _this.count += 1;
            el.innerHTML = this.count;
        })
}
new Counter(container);
```

```js
var container = document.getElementById('container');
function Counter(el) {
    this.count = 0;
    el.innerHTML = this.count;
    el.addEventListener('click',
        () => {
            this.count += 1;
            el.innerHTML = this.count;
        })
}
new Counter(container);
```

####Destructuring

```
var a = 1;
var b = 5;
[a,b] = [b,a];
```


##Angular 2

`$ npm install`

`$ npm start`

app.component.ts (AppComponent class)

```js
export class AppComponent {
  title = 'Recipe List';
  recipe = 'Lasagna';
}
```
@Component

```js
template: `<h1>{{title}}</h1><h2>{{recipe}} details</h2>`

```

app.component.ts (Recipe class)

- convert hero from a literal string to a class:

```js
import { Component } from '@angular/core';

export class Recipe {
  id: number;
  name: string;
}
```

Now that we have a class let's refactor the component's recipe property to be of type Recipe and initialize it with values:

```js
export class AppComponent {
  title = 'Recipe List';
  recipe: Recipe = {
    id: 1,
    name: 'Lasagna'
  };
}
```

Our template needs updated bindings to refer to the recipe's name property:

```js
template: `
<h1>{{title}}</h1>
<h2>{{recipe.name}} details!</h2>`
```

Expand the HTML:

```js
template: `
<h1>{{title}}</h1>
<h2>{{recipe.name}} details.</h2>
<div><label>id: </label>{{recipe.id}}</div>
<div>
  <label>name: </label>{{recipe.name}}
  <input value="{{recipe.name}}" placeholder="name">
</div>`
```

###Binding

There is no more $scope!

Before we can use two-way data binding for form inputs, we need to import the FormsModule package in our Angular module. We add it to the NgModule decorator's imports array. This array contains the list of external modules used by our application. Now we have included the forms package which includes ngModel.

app.module.ts (FormsModule import)

```js
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { FormsModule }   from '@angular/forms';

import { AppComponent }  from './app.component';
@NgModule({
  imports: [
    BrowserModule,
    FormsModule
  ],
  declarations: [
    AppComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }

```

Replace the `<input>` with the following HTML

```html
<input [(ngModel)]="recipe.name" placeholder="name">
```

###Master Detail

Add data to our app using a typed constant:

```js
const RECIPES: Recipe[] = [
    { id: 1309, name: 'Lasagna' },
    { id: 1310, name: 'Pho Noodle Soup' },
    { id: 1311, name: 'Hamburger' },
    { id: 1312, name: 'Guacamole' }
];
```

Create a public property in AppComponent that exposes the heroes for binding

app.component.ts (recipe array property)

```js
recipes = RECIPES;
```

e.g. 

```js
export class AppComponent {
    title = 'Recipe List';
    recipes = RECIPES;
```

We do not have to define the recipes type. TypeScript can infer it from the RECIPES array.

Displaying recipes in a template.

Insert the following chunk of HTML below the title and above the recipe details.

```html
<h2>Recipe List</h2>
<ul class="recipes">
  <li>
    <!-- each recipe goes here -->
  </li>
</ul>
```

First modify the `<li>` tag by adding the built-in directive `*ngFor`

```html
<li *ngFor="let recipe of recipes">
```

The (*) prefix to ngFor indicates that the `<li>` element and its children constitute a master template.

The ngFor directive iterates over the recipes array returned by the AppComponent.recipes property and stamps out instances of this template.

The quoted text assigned to ngFor means “take each recipe in the recipes array, store it in the local recipe variable, and make it available to the corresponding template instance”.

The let keyword before "recipe" identifies recipe as a template input variable. We can reference this variable within the template to access a recipe's properties.

```html
<li *ngFor="let recipe of recipes">
  <span class="badge">{{recipe.id}}</span> {{recipe.name}}
</li>
```

Modify the `<li>` by inserting an Angular event binding to its click event.

```html
<li *ngFor="let recipe of recipes" (click)="onSelect(recipe)">
```

`selectedRecipe: Recipe;`

```js  
export class AppComponent {
    title = 'Recipe List';
    recipes = RECIPES;
    selectedRecipe: Recipe;
    onSelect(recipe: Recipe): void {
        this.selectedRecipe = recipe;
    }
}

```

```
<div>
    <label>name: </label>
    <input [(ngModel)]="selectedRecipe.name" placeholder="name"/>
</div>
```

When our app loads we see a list of recipes, but a recipe is not selected. The selectedRecipe is undefined. That’s why we'll see an error in the browser’s console.

Wrap the HTML recipe detail content of our template with a `<div>`. Then add the ngIf built-in directive and set it to the selectedRecipe property of our component.

```
<div *ngIf="selectedRecipe">
  <h2>...
</div>
```

Be sure to refer to the new selectedRecipe prop:

```
<div *ngIf="selectedRecipe">
<h2>{{selectedRecipe.name}} details.</h2>
<div><label>id: </label>{{selectedRecipe.id}}</div>
<div>
    <label>name: </label>
    <input [(ngModel)]="selectedRecipe.name" placeholder="name"/>
</div>
</div>
```

Manipulating classes in Angular 2:

```
<ul class="recipes">
    <li *ngFor="let recipe of recipes" [class.selected]="recipe === selectedRecipe" (click)="onSelect(recipe)">
        <span class="badge">{{recipe.id}}</span> {{recipe.name}}
    </li>
</ul>
```

Add CSS to our @Component:

```js
  styles: [`
  .selected {
    background-color: #333 !important;
    color: white;
  }
`]
```

Square brackets ([]) are the syntax for a property binding in which data flows one way from the data source (the expression recipe === selectedRecipe) to a property of class.

###Detail Component

Add a new file `app/recipe-detail.component.ts`

```
import { Component, Input } from '@angular/core';

@Component({
    selector: 'my-recipe-detail',
})

export class RecipeDetailComponent {

}

```

Begin by importing the Component and Input decorators from Angular because we're going to need them.

Create metadata with the @Component decorator where we specify the selector name that identifies this component's element. Then we export the class to make it available to other components.

When we finish, we'll import it into AppComponent and create a corresponding <my-recipe-detail> element.

Replace selectedRecipe with recipe in the template:

```
<div *ngIf="recipe">
    <h2>{{recipe.name}} details</h2>
    <div><label>id: </label>{{recipe.id}}</div>
    <div>
        <label>name: </label>
        <input [(ngModel)]="recipe.name" placeholder="recipe name" />
    </div>
</div>
```

The recipe property - `recipe: Recipe;` - is needed in both components so we make a new file.

```js
export class Recipe {
    id: number;
    name: string;
}
```

then in both components:

```js
import { Recipe } from './common/recipe';
```

Declare recipe as an input:

```js
export class RecipeDetailComponent {
    @Input()
    recipe: Recipe;
}
```

Final recipe-detail.component:

```js
import { Component, Input } from '@angular/core';
import { Recipe } from '../common/recipe';

@Component({
    selector: 'my-recipe-detail',
    templateUrl: './app/recipe-detail/recipe-detail.html'
})

export class RecipeDetailComponent {
    @Input()
    recipe: Recipe;
}
```

Now the app.module

```js
import { RecipeDetailComponent } from './recipe-detail/recipe-detail.component';
...
@NgModule({
  imports: [BrowserModule, FormsModule],
  declarations: [AppComponent, RecipeDetailComponent],
  bootstrap: [AppComponent]
})
```

bind the selectedRecipe property of app.component to the recipe in recipe-detail.component :

```
<my-recipe-detail [recipe]="selectedRecipe"></my-recipe-detail>
```

Final app.component:

```js
import { Component } from '@angular/core';
import { Recipe } from './common/recipe';

const RECIPES: Recipe[] = [
    { id: 1309, name: 'Lasagna' },
    { id: 1310, name: 'Pho Noodle Soup' },
    { id: 1311, name: 'Hamburger' },
    { id: 1312, name: 'Guacamole' }
];

@Component({
    selector: 'my-app',
    templateUrl: './app/app.component.html'
})
   
export class AppComponent {
    title = 'Recipe List';
    recipes = RECIPES;
    selectedRecipe: Recipe;

    onSelect(recipe: Recipe): void {
        this.selectedRecipe = recipe;
    }
}
```



