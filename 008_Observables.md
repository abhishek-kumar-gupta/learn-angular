# Observables
An Observable can be thought of as a data source.
Such as : (User Input) Events, Http Requests, Triggered in Code etc.

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