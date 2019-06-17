# HTTP Request
Since with Angular we are building SPAs so when we connect to the server we don't recieve a new Page.
Rather we just get back the data in Javascript Object Format.

## Sending POST Request
##### server.service.ts
```
import { Injectable } from '@angular/core';
@Injectable()
export class ServerService{
    constructor( private http:Http ) {}
    storeServers(servers:any[]){
        this.http.post( URL_HERE, servers );
        // 1st arg -->> URL
        // 2nd arg -->> Data/Payload
    }
}
```

##### component.ts
```
this.serverService.storeServers(this.servers) // request is send when we subscribe
    .subscribe(
        (response) => console.log(response),
    (errors) => console.log(errors)
    );
```