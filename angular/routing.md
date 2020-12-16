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

## Fetching Query Parameters and Fragments
---
`console.log(this.route.snapshot.queryParams);`

`console.log(this.route.snapshot.fragment);`

The above two will have same problem as params as discussed above. Alternative is below.

`this.route.queryParams.subscribe();`

`this.route.fragment.subscribe();`

## Setting up Child (Nested) Routes
---
`const appRoutes: Routes = [`

`{"path": "/server", "component": "ServersComponent",`

`"children": [{"path": "/id", "component": "ServersComponent"}`

`]}]`

This is how we nest child routes. Apart from this, we also need to use `<router-outlet>` for places where we place the child components.

In the `router.navigate` method we can also pass a parameter called as `queryParamsHandling: 'preserve'` which preserves our query parameters and similarly `queryParamsHandling: 'merge'` to merge the old query parameters with new ones (if any).

## Redirection and WildCard Routes
---
    const appRoutes: Routes = [
        {"path": "not-found", "component": "NotFoundComponent"},
        {"path": "**", "redirectTo": "/not-found"},
    ]

The `**` wildcard is a special wildcard which tells us how to handle routes which are not handled by our app. It should be placed at the very end of our routes section otherwise all routes after this will give not found.

`redirectTo` property tells us which route to navigate to incase no matching route is found.

Since the default matching strategy is `"prefix"` , Angular checks if the path you entered in the URL does start with the path specified in the route. Of course every path starts with ''  (Important: That's no whitespace, it's simply "nothing").

To fix this behavior, you need to change the matching strategy to `"full"` :

`{ path: '', redirectTo: '/somewhere-else', pathMatch: 'full' } `

Now, you only get redirected, if the full path is ''  (so only if you got NO other content in your path in this example).

## Introduction to Guards
---
Sometimes, in our app, we want to protect certain routes from the user. Therefore, Angular provides us with guards.

Angular implements this guard functionality by providing `canActivate` interface exposed by `@angular/router`.

If implemented this inferface provides a `canActivate` method which will be called before a component associated with a route is called.

`canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean>| Promise<boolean>|boolean {}`

The `canActivate` method returns either an Observable or Promise of the type boolean or a boolean itself because there might be some instance where you actually need to fetch some data from server before making a decision to load or guard a route.

In the AppRoutes, we specify on how to use this functionality in the following way.

`const appRoutes: Routes = [`

`{path: 'servers', canActivate: [AuthGuard], component: 'ServersComponent'}`

`]`

Now what will happen is that when someone tries to load the servers route, it will first go to AuthGuard service where our `canActivate` method is implemented. If the method returns `true`, only then it navigates to ServersComponent, else it handles it according to implementation in `canActivate` method. The `canActivate` method also protects all the child routes.

If you want to protect just the child routes and not the main route, then you can implement the `canActivateChild` method. Everything is similar like the `canActivate` method.
Only difference will be there in the `appRoutes` file where in the parent route we will use `canActivateChild` instead of `canActivate`.

**Read about `canDeactivate()`. It gives us the functionality to prevent user from accidentally navigating away from a route. Bit confusing.**

## Passing Static and Dynamic Data to a Route
---
Sometimes, we need to pass on some data also via our route. This data can either be static or dynamic. Lets see how we can do that.
### Static Data
[{path:"servers", component: "ServersComponent", data:{"message": "This route was served by a genius"}}];

And in the component, you can get the data using

`this.route.snapshot.data["message"];`
or

`this.route.data.subscribe((data: Data) => {`

  someValue = data["message"];

`})`

### Dynamic Data

Now you do not essentially need out-of-the-box functionality to do this as you can always add a busy indicator till the time backend data is being fetched in say ngOnInit. But Angular provides us a way to preload this data before we actually navigate to our route.

For this we will need to implement the `Resolver` interface exposed by the `@angular/router` and implement its `resolve()` method.

The `resolve()` method signature looks like this -

`resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<response> | Promise<response> | response {}` 

In the AppRoutes we add property like

`const appRoutes: Routes = [`

`{path: 'servers', resolve: {server: ServerResolver}, component: 'ServersComponent'}`

`]`

`ServerResolver` is the path where we have implemented our `resolve()` method.

And in the component where we are supposed to get the dynamic data, we simply do

    this.route.data.subscribe((data: Data) => {
        console.log(data['server']);
})

The property name 'server' should be the same as defined in the resolve property in app routes.


## Understanding Location Strategies
---
There are two types of location Strategies used in Angular.

1. *PathLocationStrategy*

- **PathLocationStrategy** is the *default* location strategy used by Angular.  
- It uses pushstate API to change the URL so the browser doesn't request the page from the server.  
- The routes look like this - `localhost:4200/servers` or `localhost:4200/users`.  
- Using this strategy, routes like /servers or /users might work on localhost but there is no guarantee that they will work on the web as these url/routes are first parsed/handled by the web server and the server will look for these routes first at the server level before delegating it to Angular app . 
- Therefore, we need a server that sends back the same page for all URLs. The server hosting your SPA application has to be configured such that in case of a 404 error also, it returns the `index.html` file.
- **Angular Universal** also supports only this strategy.

2. *HashLocationStrategy*
- **HashLocationStrategy** uses a hash fragment.
- It is easier to set up. `imports : [RouterModule.forRoot(appRoutes, {useHash: true})]`. This sets up hash location based routing.
- Also, this does not require any help from the server. So you can host it on any type of server.
- There is no support for this strategy in Angular Universal.
- HashLocationStrategy is needed if you need to work with legacy browsers like <= IE9.
- Route looks like localhost:4200/#/server or localhost:4200/#/users.