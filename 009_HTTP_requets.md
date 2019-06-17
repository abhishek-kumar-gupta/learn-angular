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
        // 3rd arg -->> Headers {headers:headers}
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

## Adjusting Request Headers
##### server.service.ts
```
import { Injectable } from '@angular/core';
@Injectable()
export class ServerService{
    constructor( private http:Http ) {}
    storeServers(servers:any[]){
        const headers = new Headers({'Content-Type' : 'application/json'});
        this.http.post( URL_HERE, servers, headers );
    }
}
```

## Sending GET Request
To get back data through HTTP request.
##### server.service.ts
```
import { Injectable } from '@angular/core';
@Injectable()
export class ServerService{
    constructor( private http:Http ) {}
    getServer(){
        return this.http.get(URL);
    }
}
```

##### component.ts
```
import { Response } from '@angular/http';
onGet(){
    this.serverService.getServer().subscribe(
        (response:Response) => {
            const data = response.json()
            console.log(data);
        },
        (error) => console.log(error)
    );
}
```

## Sending PUT Request
It doesn't append the data rather it overrides it.
##### servers.service.ts
```
storeServers(servers:any[]){
        return this.http.put(URL,Data_here,{'headers':headers});
}
```

## Transforming response using Observables
##### servers.service.ts
```
import 'rxjs/RX';
import { Response } from '@angular/http';
getServers(){
    return this.http.get(URL_HERE).map(
            (response:Response) => {
                const data = response.json();
                return data;
            }
        )
}
```
##### component.ts
```
onGet(){
    this.serverService.getServer().subscribe(
        (data) => {
            console.log(data);
        },
        (error) => console.log(error)
    );
}
```