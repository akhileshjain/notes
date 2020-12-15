# Routing
---

Routing is a concept via which we make our application truly a single page application (SPA). What we mean by SPA is that we send only a single page (index.html) to the client and a bunch of JavaScript (basically that is all the client should have to work on its side).

If we change the routes say `localhost:4200` to `localhost:4200/servers`, we should not be sending a request for new page to the server because essentially the functionality is already there on the client side and we just need to refresh the viewport with requested information.

Now, to use router in angular we need to access it from some place accessible throughout the app. The logical place would be `appModule.ts` and that is where we define our routes.

`const appRoutes: Routes = [{"path": "/server", "component": "ServersComponent"}]`

This is how we define the routes. 
We need to import `Routes` from `@angular/router`.

But merely defining the routes in not enough. We also need to tell our application that we need to use those routes. This is done by importing RouterModule in the imports array of our AppModule.

`imports : [RouterModule.forRoot(appRoutes)]`

The place where we want to load our route component, we simply add the directive 
`<router-outlet>`.

Now, in order that the page do not reload on change of the route, we need to add one last thing to the place which triggers route change.

`<a [routerLink]= "['/servers']">`

or simply

`<a routerLink = '/servers'>`

RouterLink is the special directive which catches the link on the page, prevents the default (which is reloading of the page), parses the route associated with the route link and then sees the available routes if there is any fitting route to handle it and then load that functionality.

This way we do not load the application again and also prevent resetting of the application state.

### Understanding navigation paths
---
/ -> will give you the absolute path

./ or nothing -> will give you the relative path

../ -> will give you the contextual path

### Getting Active Router Link

The directive used for setting the active router link is `routerLinkActive`

You can set it like this- 

`<li [routerLinkActive]="active"><a routerLink = '/'`

`<li [routerLinkActive]="active"><a routerLink = '/users'`

Now whenever a user goes to users route, that particular list item will get active class. But first link will be active both the times because technically /users is also a matching route of /.

In order to avoid this behaviour, we simply add another directive to the first `li` like this.

`<li [routerLinkActive]="active" [routerLinkActiveOptions]="{exact: true}"><a routerLink = '/'`

### Routing Programmatically
---
In order to navigate programmatically, we need an Angular feature called as `Router` from the module `@angular/router` and then inject it into our component.
For example - 

`import {Router, ActivatedRoute} from '@angular/router';`

`constructor(private router: Router, private route: ActivatedRoute) {}`

`someMethod() {`

`this.router.navigate(['servers'], {relativeTo: this.route})`

`}`

Now unlike the `routerLink`, the `navigate()` method does not know which route you are currently on. This can be done using `relativeTo` as shown above.

## Fetching Route Parameters

In case you want a parameterized route it can be configured something like this :

`/user/:id`

where id is the parameterised parameter. In order to fetch such route parameters in our component, the `ActivatedRoute` gives us the params property.

`this.route.snapshot.params["id"]`

So, this will give us the value of route parameter. But there is one small problem with this approach.

If we want to navigate from say
`/user/1` to `/user/3` then the above way will not work as for the router it is the same route. We need to call this route reactively in such a case like this using an Observable exposed by ActivatedRoute -

`this.route.params.subscribe(params: Params => {`
 
 `   console.log(params["id"]);`

`})`

If you know that there is no scenario where a component you are on can be reloaded from the same component, then the first scenario is also good enough.

## Passing Query Parameters and Fragments
---
Query Parameters are part of the URL followed by ? and &.
Fragment is the part of URL followed by #.

In the `<a>` tag you can access query parameters like this -
`<a [routerLink]="/users"`
`[queryParams]="{allowEdit: '1'}"`
`[fragment]="loading">`

Resolves to this url - `/users?allowEdit=1#loading`

In order to do this programmatically -

`this.router.navigate(['servers'], {queryParams: {allowEdit: 1}, fragment: 'loading'})`