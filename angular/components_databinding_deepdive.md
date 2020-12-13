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