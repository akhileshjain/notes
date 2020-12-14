# Services and Dependency Injection
---
A service in Angular is simply a TypeScript file. As such, we don't need any decorator to tell it is a service file and we can call the service by simply creating an instance of the service class. But that is not how we call services in Angular. To call a service in Angular, we use a concept called as **Hierarchical Injection**.

Angular's dependency injector works on the principle of hierarchical injection. So, if we provide the service to app-component, then all the children of app-component would get the same instance of the service defined in the `AppComponent`. But if we again provide the service in one of the child component, then it will create a new instance of that service.

Logically, the top level where we can define the service is in the `app-module.ts`.

Now, if we want to inject a service into another service we need to add some metadata into the **receiving** service. The metadata is `@Injectable()`.

In general, it is a safe practice to add @Injectable() in all the service files.

## Services in Angular 6+
---
If you're using Angular 6+, you can provide application-wide services in a different way.

Instead of adding a service class to the `providers[]`  array in `AppModule` , you can set the following config in `@Injectable()` :

`@Injectable({providedIn: 'root'})`.


Using this new syntax is completely optional, the traditional syntax (using `providers[]` ) will still work.

The "new syntax" does offer one advantage though: Services can be loaded lazily by Angular (behind the scenes) and redundant code can be removed automatically. This can lead to a better performance and loading speed.