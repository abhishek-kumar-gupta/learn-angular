# Route_Guard

##### auth.guard.ts (service)
```
import { CanActivate } from '@angular/router';
import { Injectable } from '@angular/core';

@Injectable()
export class AuthGuard implements CanActivate{
    // now we must have "canActivate()" method so it works properly.
    
    constructor(private authService:AuthService){}
    
    canActivate(route:ActivatedRouteSnapshot,state:RouteStateSnapshot): Observable<boolean> |
    Promise<boolean> | boolean
    {
        // takes two args
        
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