# Observables
An Observable can be thought of as a data source.
Such as : (User Input) Events, Http Requests, Triggered in Code etc.
For more go through official docs of rxjs.

Here we have two things Observable and Observer.
And in between them we have a timeline of events emitted by the observable.
Observer is our code (.subscribe()).Here we have three ways of handling data:
- Handle Data
- Handle Errors
- Handle Completion

 In these hooks are data gets executed.
 **NOTE: Observables not always Complete.**
 
 ## Built-in Observable
 ##### component.ts
 ```
 ngOnInit(){
     this.route.params.subscribe(
     (params:Params) => {
         this.id = +params['id'];
     }
     // we also have two more methods one for Errors( if it fails) and Other if it completes.
     ).
 }
 ```
 
 # Custom Observable
 ##### component.ts
 ```
 import { Observable } from 'rxjs/Observable';
 import 'rxjs/Rx';
 
 ngOnInit(){
     const myNumbers = Observable.interval(1000);
     // interval basically will now emit data automatically with the interval of 1sec.
     myNumbers.subscribe(
     // we can pass three arguments(Three Callback functions) to the subscribe method
     // 1st -->> Callback to handle normal data
     // 2nd -->> Callback for handling errors
     // 3rd -->> Callback for handling completion of the observable
     (number:number)=>console.log(number),
     );
 }
 ```
 
 # 2. Custom Observable
 ##### component.ts
 ```
 import { Observable } from 'rxjs/Observable';
 import 'rxjs/Rx';
 import { Observer } from 'rxjs/Observer';
 ngOnInit(){
 const myObservable = Observable.create((observer:Observer<string>) => {
        // it takes one arg, a function that contains our async code.
        setTimeout(()=>{
            observer.next('first package');
            // next emits normal data packet
        },2000);
        setTimeout(()=>{
            observer.next('second package');
            // next emits normal data packet
        },4000);
        setTimeout(()=>{
            observer.error('This doesn't work');
        },5000);
        setTimeout(()=>{
            observer.complete('This doesn't work');
        },7000);
    });
    
    myObservable.subscribe(
        (data:string) => {console.log("data")},
        (error:string) => { console.log("error")},
        () => console.log("Completed")
    );
 }
 ```
 Once the observable is used if something was left it's not send.
 
 ## Unsubscribe
 If your observable is not going to complete it'll keep on running even if the component is destroyed. 
 And that creates a memory leak. Therefore we must unsubcribe before we leave the area where we are handling the Observable.
 
##### component.ts
```
ngOnInit(){
    this.myCustomObservable = myObservable.subscribe();
}
ngOnDestroy(){
    this.myCustomObservable.unsubscribe();
}
```
**Also, keep in mind Built-in Observables in Angular clean themselves automatically.**

## Subject  
Subject is basically like an observable but it allows you to push it to emit new data.
A Subject is an Observable and an Observer at the same time.
Event Emitter is built on top of a Subject, prefer using subject instead of EventEmitter.
##### users.service.ts
```
import { Subject } from 'rxjs';
export class UsersService{
    userActivated = new Subject();
    
}
```
##### user.component.html
```
<button (click)="onActivate()">Activate</button>
```
##### user.component.ts
```
onActivate(){
    this.userService.userActivated.next(this.id);
}
```
##### app.component.ts
```
user1Activated = false;
user2Activated = false;
ngOnInit(){
    this.userService.userActivated.subscribe(
    if(id==1){
        this.user1Activated=true;
    }
    );
}
```

## Observales Operators
##### component.ts
```
import 'rxjs/Rx'; // important to use operators
const myNumbers = Observable.interval(1000).map(
                            (data:number)=>(data*2) );
// Operator returns a new Observable so we can chain these operators togther.
```

