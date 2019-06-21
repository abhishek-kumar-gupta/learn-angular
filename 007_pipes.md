# Pipes
It allows us to transform output in our template.
EG:
##### component.html
```
<p>{{ serverName | uppercase }}</p>
<!-- uppercase is a pipe -->
```
Here, uppercase is a built-in pipe.

## Parameterized Pipes
##### component.html
```
<p>{{ serverStarted | date:'fullDate' }} </p>
<!-- here "fullDate" is the parameter 
  ... Data | pipe1:arg1:arg2:arg3 | pipe2:arg1    
  Hence, we could chain pipes using the "|" symbol ( applied Left --> Right ).
-->
```

**Note: Look in Docs->API Reference for more Built-in Pipes**


## Custom Pipes
###### shorten.pipe.ts
```
import { Pipe, PipeTranform } from '@angular/core';

@Pipe({
    name:'shorten'
})
export class ShortenPipe implements PipeTransform{
    // we need "transform" method here , that take "transform(value,arg1,arg2,...argN)".
    // Also should return something.
    tranform(value:any){
        return value.slice(0,10); 
    }
}
```
Now to use this pipe we need to add this to "app.module.ts" --> Declarations
##### module.ts
```
@NgModule({
    declarations:[
    ...,
    ShortenPipe
    ]
})
```
Now we are good to go.
##### component.html
```
<p>{{ data | shorthen }}</p>
```

## Parameterized Custom Pipe
##### shorten.pipe.ts
```
import { Pipe, PipeTranform } from '@angular/core';

@Pipe({
    name:'shorten'
})
export class ShortenPipe implements PipeTransform{
    tranform(value:any,limit:number){
        // here limit is a parameter
        return value.slice(0,limit); 
    }
}
```
##### component.html
```
<p>{{ data | shorten:15 }}</p>
```

## Filter Pipe (Example)
```
> ng g p pipe_name
```
##### component.html
```
<div>
    <input type="text"  [(ngModel)]="filterString">
    <li *ngFor="let line of data | filter:filterString:'status' "> {{ line }} <li>
</div>
```
##### component.ts
```
filterString:string="";
```
##### let's create a pipe, ** >> ng g p filter**
##### filter.pipe.ts
```
import { Pipe, PipeTranform } from '@angular/core';

@Pipe({
    name:'filter'
})
export class ShortenPipe implements PipeTransform{
    resultArray;
    tranform(value:any,filterString:string,propName:string){
        for(const item of value){
            if (item[propName] == filterString ){
                this.resultArray.push(item);
            }
        }
    }
} 
```

## Pure and Impure Pipes
Angular doesn't re-run these pipes everytime the data changes. Though we can override this functionality.
EG: Updating Arrays and Objects doesn't trigger it.
To inforce this functionality
##### ....pipe.ts
```
@Pipe({
    name:'my_pipe',
    pure : false
})
// true by default
```

## Async Pipe
##### component.html
```
<p>{{ appStatus | async }}</p> 
```
It waits for it to get it resolved and then updates the data.
