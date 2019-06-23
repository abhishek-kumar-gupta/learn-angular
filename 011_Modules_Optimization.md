# Modules
### Module { Components + Directives + Other Modules etc. }
All the elements that make up our app have to be registered into the module(or mutliple modules).
So Bascially it allows us to choose what we need ( and more importantly ) what we don't need.

### Declarations
It consists of Components, Directives and Pipes that our Module uses.

### Imports
The other modules this module uses.
When we import a module we basically get everything that it exports.
Eg: FormsModule defines some directives that we use while dealing with forms.

The **providers** array simply defines which services we may use in this module.
Everything we provide here will be available for the whole app.

The **bootstrap** array defines the root component. The main component where we start registering other components.

## Feature Modules
A custom module that contains a certain feature that make up some part of the app.
##### auth.module.ts
```
import { NgModule } from '@angular/core';
@NgModules({
    declarations : [ All the components used by this feature ],
    import : [],
    providers : [ CommonModule ]
    // Common Modules is to be required in almost every feature module
    // it provides access to directives such as ngFor,ngIf etc.
})
export class AuthModule{
    
}
```
##### app.module.ts
```
imports:[ AuthModule ]
```
##### - Nothing ( Pipe,Component,Directive etc ) could be declared in two modules. 
##### - Services could be provided in multiple modules.
##### - BrowserModules ( CommonModule + some additional features ) contains features that are required mostly when the application starts. So it's only required by the main Module.

# Routing
If we create a feature module we have to move the routes related to that feature module.
##### auth-routing.module.ts
```
const authRoutes:Routes=[];
@NgModule({
 imports:[
 RouterModule.forChild()
 ],
 exports :[ RouterModule ]
})
export class AuthRoutingModules{
    
}
```
##### auth.module.ts
```
@NgModule({
    imports : [ AuthRoutingModule ]
})

```

# Shared Module
Say we have a custom directive that is being used by multiple Feature Modules. So in that case we build Shared Module.
So to share the part that is being used by multiple shared modules.
##### shared.module.ts
```
import { NgModules } from '@angular/core';
@NgModules({
    declarations : [ DropDownDirective ],
    exports : [ DropDownDirective ]
})
export SharedModule{}
```
##### feature.module.ts
```
@NgModule({
    imports : [ SharedModule ]
})
```

# Lazy Loading
Instead of loading all the components at the beginning it allows us to load the Modules when we visit any route leading to that module.
By default the app loads **Eagerly**, that is when we load the application everything in the imports get loaded.
To implement **Lazy Loading**.
##### app-routing.module.ts
```
// remove all modules to lazy load from app.module.ts
 { path :'dashboard', loadChildren:'./path_here/dashboard-module#DashboardModule' }
 // path#ClassName
```

# Module and Service Injection
