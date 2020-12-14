# Component Communication

By default, all properties of a component are available/accessible within that component.

In order to make a property accessible outside the component, we need to add the
`@Input` decorator to that property. Something like this, 

`@Input() element: any;`

You can give the alias name in the parenthesis and access it via the alias name.

`@Input('srvElement') element: any;`

Other way to look at this is that @Input decorator provides a way to pass data from parent component to child component.

![Input](/images/input.svg "Input")


![Input](/images/input-diagram-target-source.svg "Input")

Similar to custom properties on the, we also have custom events. The `@Output()` decorator in a child component or directive allows data to flow from the child to the parent.

 We add `@Output` property to make it listenable outside.

Also, to make it an event, we create the instance of EventEmitter class.

`@Output() serverAdded = new EventEmitter<{abc: String}>()`

![Output](/images/output.svg "Output")


## View Encapsulation

By default, the style applied in CSS of a particular component are applied only to that component. Does not matter if it is a parent, child of any other component. This is the default behavior of Angular and it overrides the default CSS behavior of DOM by emulating a concept called as Shadow DOM.

In case you want to override this default behavior for a component, in its @Component decorator, you can provide this property.

`encapsulation: ViewEncapsulation.None`
`encapsulation: ViewEncapsulation.Native`
`encapsulation: ViewEncapsulation.Emulated`

Default is Emulated. 
Native uses Shadow DOM technology which isn't supported by many browsers. None makes CSS behave normally i.e. styles get applied globally.

### Local References to HTML element in the Template
---
Basically any HTML element can be given a local reference by using `#<name>`. This returns us the reference of the complete HTML element. Now this reference can be used in this template **ONLY**.

`<input type="text" #myName>`

`<button (click)='onClick(myName)'>`

This is how we can pass the local reference of an element in the template.

### Getting access to HTML Element using @ViewChild()
---
In the above Local references section, we said that we can access local references only in the template part i.e. the HTML file only. But what if we want to access that local reference to an HTML element in the TypeScript Code. Well, the way to do that is by using an Angular Directive called as `@ViewChild()`.

This is how we use it.

`@ViewChild('myName') // myName is the name of local reference we defined in template.`

This will be of type ElementRef. So, in order to get the element, we will do `<var_name>.nativeElement`.

In Angular 8+, the @ViewChild() syntax is slightly different

`@ViewChild('serverContentInput', {static: true}) serverContentInput: ElementRef;`

The same change (add `{ static: true }` as a second argument) needs to be applied to ALL usages of `@ViewChild()`. If you DON'T access the selected element in `ngOnInit` (but anywhere else in your component), set `static: false` instead!

If you're using Angular 9+, you only need to add `{ static: true }` (if needed) but not `{ static: false }`. All the above things hold true for `@ContentChild()` as well.

### Projecting content into Components using ng-content

Everything you place between the opening and closing tags of your own components (custom components) is not rendered by default. But in case you want to render it inside your component without placing that HTML inside the component, but by placing between opening and closing tags, you can use the `<ng-content>` directive.

So, for example, if this is the place where we call server.component.ts -

`<app-server>`

`<p>I'm the data coming from outside</p>`

`</app-server>`

Then inside our server.component.ts it is like -

`<div>`

`<h1>Hello</div>`

`<ng-content></ng-content>`

`</div>`

The place where we add ng-content directive is where that `p` tag will go.

What if you could decide which content should be placed where? Instead of every content projected inside a single `<ng-content>`, you can also control how the contents will get projected with the select attribute of `<ng-content>`. It takes an element selector to decide which content to project inside a particular `<ng-content>`

![Multiple Content Projection](/images/multi-content-projection-1.png "Multiple Content Projection")

Calling the component will look like:

![Multiple Content Projection](/images/multi-content-projection-2.png "Multiple Content Projection")


## Angular Lifecycle Hooks
---

![Angular Lifecycle Hooks](/images/lifecycle-hooks.png "Angular Lifecycle Hooks")

`constructor()` is also called before `ngOnInit()`.

---
*Sidenote*:
Similar to @ViewChild(), we also have @ContentChild() which is used to access reference to the HTML element which is passed on via `<ng-content>`. It is not accessible in `ngOnInit()` and can only be accessed in and after `ngAfterContentInit()` lifecycle hooks.

Even `@ViewChild()` reference is not available in `ngOnInit()`, but only in or after `ngAfterViewInit()`.