# Angular

Angular is a JavaScript Framework which allows you to create Single Page applications (SPAs)

A Single-Page application is basically an application with one HTML file and a bunch of JS Code we got from the server and every change on the screen is rendered on the browser (re-rendering of DOM).

**Angular CLI** is a common line interface for creating Angular applications because Angular projects are bit more elaborate regarding their build workflow. There are some files which need to be converted before they can run in the browser and some other heavy lifting which the CLI handles for us.

Angular in the end is a JS framework changing our DOM ('HTML') at runtime!

![Loading](/images/angular-loading.png "Angular Loading")

*Although **index.html** (the only HTML file to be served by the server) may look empty but Angular CLI adds 2-3 JS files like `polyfills.bundle.js` and `main.bundle.js` at the runtime. The main.bundle.js (which was `main.ts`) is the starting file, the one which imports the `app.module.ts` and the `app.module.ts` calls the `app.component.ts` which is the root component file.*

## Components
---
**REMEMBER** - A Component is basically a TypeScript Class at the end of the day.

**NOTE** - Decorators are special features provided by TypeScript which give special meaning to a TS file. They are represented by an **@**.

So, in order to tell if a TS File is a component or a service or anything else, there are decorators. For example, to tell if a TS file is a component, there is a `@Component` decorator. Most of these decorators are to be imported from `@angular/core`.

@Component decorator needs a JavaScript object which must contain atleast the `template` or `templateUrl` property. The other properties are `selector` and `style`  but they are optional.

`@Component({`

`    selector:'app-root',`

`    templateUrl:'./app-component.html'`

`styleUrls: [./app-component.css]`

`})`

Angular uses components to build webpages while it uses modules to bundle components into packages.

By Default, Angular does not scan for all your components which you create. You need to tell that to Angular. This is done in the app.module.ts file which is the base file. 
Here in `@NgModule`, in the declaration, we define all the components that are needed for one app.

If you see the styles declaration, it is defined as an array. Because you can pass in the reference to multiple stylesheets for a component.

Also, selectors are of 3 types -
- **Element Selectors** -> `selector: 'app-root'`. This will be used as `<app-root></app-root>`.
- **Attribute Selectors** -> `selector: '[app-root]'`. This will be used as `<div app-root></div>`.
- **CSS Selector** -> `selector: '.app-root'`. This will be used as `<div class='app-root'></div>`.

*Note - ID Selectors (#) and pseudo-selectors like hover etc. are not supported by Angular.*

## Data Binding in Angular
---
![image](/images/angular-databinding.png "Angular Data Binding Overview")

### String Interpolation
---
Any expression that can be resolved to a string finally can be used while using string interpolation syntax.

`{{anyString}}`

- We can't use multiline expression though, like a block statement etc.
- We can use a ternary expression though or even method call provided it returns a string or something that can be resolved to a string (like a number).

### Property Binding

Property binding is not necessarily limited to HTML DOM properties. It can be applied to directives and components as well.

### When to use String Interpolation vs Property Binding?
- Basically, when you want to output some value simply, then use string interpolation.
- When you want to update or change something, a property in DOM etc, go for Property Binding.
- `[disabled]="allowUser"`. Whole thing inside " " is an typescript expression, so you can use a method as well. 

### Event Binding
---
`(browser_event) = "code to be carried out"`

For example - `(click)='onCreateUser($event)'`

---
### Two-Way Binding

`[(ngModel)]` - Inbuilt directive needed for two-way data binding.

To be able to use `ngModel`, the `FormsModule` (from `@angular/forms`) needs to be added to your imports [] array in the AppModule (should be there by default in the CLI Project).

## Directives
---

Directives are instructions to the DOM!
Components are also directives only. The only difference is that components are directives with templates. 
Directives typically use attribute selectors.


# Built-in Directives
## **Structural Directives**
Directives which change the structure of DOM. They are preceded by *. For example - `*ngIf`, `*ngFor`.

### ***ngIf**
---
`<p *ngIf="serverCreated; else noServer">Server was created</p>`

`<ng-template #noServer><p>No Server Created!</p>
</ng-template>`

`<ng-template` is a directive shipping with Angular which is used to mark places in the DOM which we want to show conditionally. The marking is done using a reference, like #noServer is used above.

### ***ngFor**
---
`<div *ngFor="let item of items; let i=index">`
{{item}}
`</div>`
Here, index gives you the number of current iteration.
And item is the current item in the loop.

## **Attribute Directives**

Unlike structural directives, attribute directives don't add or remove elements. They only change the element they were placed on.
For e.g. - `ngStyle`, `ngClass`.

### **ngStyle**
---
`ngStyle` is used to style elements dynamically. Standalone, it is useless. Therefore, we use it in a property binding context like here -

`[ngStyle]="{backgroundColor: getColor()}"`

### **ngClass**
---
`ngClass` is used to apply CSS classes dynamically.
for example- `[ngClass]="{"classname":"TS Code"}"`

The CSS class is added if it satisfies the TS Code condition.