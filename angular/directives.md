# Directives - Deep Dive
---
### Creating a Basic Attribute Directive
---
We create a custom directive of our own by creating a new TypeScript class and using the `@Directive()` decorator which we import from `@angular/core`.

Now similar to a component, we also have to pass a selector for a directive.

`@Directive({`

   `selector: [appDirection]`

`})`

And the place from where we call this directive the call is like this- 

`<p appDirection>This is my directive!</p>`

Now in the directive file, we can access the calling element, in this case this `p` tag.

1. **Element Ref**

`constructor(private elementRef: ElementRef)`

`this.elementRef.nativeElement.style.color = red`

2. **Renderer**

`constructor(private elRef: ElementRef, private renderer: Renderer2)`

`ngOnInit() {`

`this.renderer.setStyle(this.elRef.nativeElement, 'color', 'red') }`


The second approach is better than the first because of two reasons.
- We should not try to manipulate the DOM elements manually.
- When you are running your application outside the browser or other use cases like WebWorkers then first approach is not advisable. Using the Renderer is preferred of doing it.

## Using HostListener to Listen to Host Events
---

Suppose you want to trigger any change in your directive based on event in the calling HTML tag. For that, you can use the above `@HostListener`. Like suppose if we want to trigger mouseenter event on the `p` tag which calls our appDirection directive and we want to change its color on mouseenter, this is the way to achieve it.

`@HostListener('mouseenter') mouseover(eventData: Event) {`

`this.renderer.setStyle(this.elRef.nativeElement, 'color', 'red') }`

`}`

## Using HostBinding to Bind to Host Properties
---
HostBinding allows us to bind to properties of the host element without the need of using the renderer.

`@HostBinding('style.backgroundColor') bgCol: string`

On the element where this directive sits, please access the `style` property and then subproperty `backgroundColor` and then bind it to the `bgCol` property of this directive. Now if you update the bgCol property it should essentially update the Host property.

Now when we do:

`this.bgCol = red`

It updates the background color of the host element to red.

You can use property binding in a component in the same way as we use it in a component.

**NOTE - You can't bind two structural directives on same element.**

**NOTE - Not all the lifecycle hook methods are available on directives as a directive does not have a template.**

## Structural Directive
---
The * in the structural directive converts the enclosing directive into an ng-template.

`<ng-template [ngIf]='<condition>'>`

`</ng-template>`

## *ngSwitch
---

`<div [ngSwitch]="value">`

`<p *ngSwitchCase="5">Value is 5</p>`

`<p *ngSwitchCase="15">Value is 15</p>`

`<p *ngSwitchDefault>Value is default</p>`