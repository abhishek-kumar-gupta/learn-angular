# Route_Guard

## 1 CanActivate
##### auth.guard.ts (service)
```
import { CanActivate } from '@angular/router';
import { Injectable } from '@angular/core';

import { AuthService } from 'path_here';
@Injectable()
export class AuthGuard implements CanActivate{
    // now we must have "canActivate()" method so it works properly.
    
    constructor(private authService:AuthService){}
    
    canActivate(route:ActivatedRouteSnapshot,state:RouteStateSnapshot): Observable<boolean> |
    Promise<boolean> | boolean
    {
        // takes two args
        let isLoggedIn = this.authService.isAuthenticated();
        if( isLoggedIn == true ){
            return true;
        }
        else{
            this.route.navigate('/');
            return false;
        }
        
    }
    
}
```

##### auth.service.ts
```
export class AuthService{
    loggedIn = false;
    
    isAuthenticated(){
        return this.loggedIn;
    }
    
    login(){
        this.loggedIn = true;
    }
    
    logout(){
        this.loggedIn = false;
    }
}
```

##### app.module.ts
```
const appRoutes:Routes = [
    { path:'dashboard', component:MyComponent, canActivate:[AuthGuard] }
];

@NgModule:({
    providers:[..., AuthGuard, AuthService ]
})
```

## 2 CanActivateChild
```
import { canActivate,canActivateChild } from '@angular/router';
import { Injectable } from '@angular/core';

import { AuthService } from 'path_here';
@Injectable()
export class AuthGuard implements CanActivate{
    // now we must have "canActivate()" method so it works properly.
    
    constructor(private authService:AuthService){}
    
    canActivate(route:ActivatedRouteSnapshot,state:RouteStateSnapshot): Observable<boolean> |
    Promise<boolean> | boolean
    {
        ...
    }
    
    canActivateChild(route:ActivatedRouteSnapshot,state:RouteStateSnapshot): Observable<boolean> |
    Promise<boolean> | boolean
    {
        this.canActivate(route,state);
    }
```

##### app.module.ts
```
const appRoutes:Routes = [
    { path:'dashboard', component:MyComponent, canActivateChild:[AuthGuard] }
];

@NgModule:({
    providers:[..., AuthGuard, AuthService ]
})
```

## 3. canDeactivate
##### can-deactivate-service.ts
```
import { Observable } from 'rxjs/Observable';

export interface CanComponentDeactive{
    canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean
}
export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate>{
    canDeactive(component:CanComponentDeactivate,
                currentRoute:ActivatedRouteSnapshot,
                currentState: RouteStateSnapshot,
                nextState?:RouterStateSnapshot):Observable<boolean>|Promise<boolean>|boolean
    {
        // called when we try to leave a route
        return component.canDeactivate();
    }
}
```
##### app.module.ts
```
{ path:'/', canDeactivate:[CanDeactivateGuard], ...}
```
##### component.ts
```
export class CanDeactivateComponent implements CanDeactivate<CanComponentDeactivate>{
    canDeactivate():Observable<boolean>|Promise<boolean>|boolean
    {
        // this is the logic that will run whenever we try to leave the route
        condition1 
            return true;
        else
        return false;
    }
}
```

## Passing Static Data To Route
##### app.module.ts
```
{ path: '/', data:{my_data:"DATA"}, ...}
```
##### component.ts
```
constructor(const route:ActivatedRoute){}
data = this.route.snapshot.data['message'];
```

## Getting Dynamic Data
Resolver basically preloads the data before rendering the route.