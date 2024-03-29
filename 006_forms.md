Angular gives use the JSON representation of the HTML form.

Here we have two approaches to handle forms:
- **[Template Driven Approach](#Template_Driven_Approach)** (Angular infers the FORM object from the DOM)
- **[Reactive Approach](#Reactive_Approach)** (Form is created programatically and synchronized with the DOM)

**NOTE:** We don't use "**action**" on the form here as we are not going to directly submit it to the server.
Rather we will handle it using angular and sent it to server via Angular's HTTP methods.

# Template_Driven_Approach
To start make sure you have imported the **"Forms Module"** in app.module.ts (in imports array).
##### app.module.ts
```
import { FormsModule } from "@angular/forms";
@NgModule({
    ...
    imports:[
    ... ,
    FormsModule
    ]
})
```
Angular automatically creates the Javascript respresentation of the form when it detects **form** selector in your code.
So you can think of that form element serving as a selector for some angular directive which then creates the JS representation for you. Also, angular won't automatically detect your **input** and other elements of your form.
So, we need to add form controls manually.

### 1. Creating Form
```
<form>
    <input ngModel  name="username">
    <!-- ngModel is the directive(made available by forms module).
    It tells angular that it's a form control.
    Now for this to work we need to give a name so that it could be identified. -->
</form>
```

### 2. Submitting Form
##### component.html
```
<form (ngSubmit)="onSubmit(f)" #f="ngForm">
    <!-- This event gets triggered when tis form get's submitted.
    We pass local reference to access this form data 
    We need to assing this local reference to **ngForm** ,
    else we won't be able to access the form properly(as JSON { f['value'] object})-->
    ...
</form>
```
##### component.ts
```
import { NgForm } from '@angular/forms';
// create a function that'll be called on submit
    onSubmit(form:NgFrom){
        console.log("Submitted");
    }
```

## Form State
Each form control comes attached with certain properties, and same on the overall form.
#### Some Properties
- **dirty** :  false intially, true when we change something in the form.
- **disabled** : true if form was disabled
- **invalid** : true if some invalid values are passed into the fields (checked by Validators)
- **touched** : true if we have clicked in some field.

## Accessing Form using @ViewChild
We could use **@ViewChild** to access the form data without needing it to be passed as a parameter.
##### component.ts
```
@ViewChild('f') formData:NgForm;
Onsubmit(){
    console.log(this.formData);
}
```
So we don't have to pass it to on submit here.
And it allows form data to be accessed even before submitting (Eg: in case of Validators)


## Validations
Since we are using Template driven approach so we can use the HTML validators.
##### Example
- required
- email
** Look for HTML validators for more **
Now it not only updates the values of **valid** & **invalid** properties.
But also it added some classes to the form control based on that.
##### These Classes are:
- ng-dirty
- ng-valid
- ng-invalid
- ng-touched


## Using the Form State
##### component.html
```
<form #f="ngForm" (onSubmit)="OnSubmit()">
    ...
    <button type=submit [disabled]="f.invalid">Submit</button>
    // disables button if form is not valid
</form>
```
##### component.css
```
input.ng-invalid.ng-touched{
    border : 1px solid red;
}
```
We could also show some error msgs using *ngIf.

## Validation Error Messages
We could get access to control using local reference on that form control.
##### component.html
```
<input #username="ngModel">
<span *ngIf="username.invalid && username.touched">Please enter a valid username</span>
```

## Default values with ngModel Property Binding
##### component.html
```
<input [ngModel]="variable">
<input [ngModel]=" 'default value as string' ">
<!-- -->
```

## Two way binding (NgModel)
##### component.html
```
<textarea [(ngModel)]="answer">
</textarea>
```
##### component.ts
```
answer="";
```

## Grouping Form Control
Used in case if we have a big form and we want to group certain data so to create a structure.
##### component.html
```
<div ngModelGroup="userData">
    <!-- form controls here -->
</div>
```
We could also get access to this form controls Java Script object using the NgModel as follows:
```
<form>
    <div #userData="ngModelGroup">
        ...
    </div>
</form>
```

## Handling Radio Buttons
##### component.ts
```
genders = [ 'male', 'female' ];
```
##### component.html
```
<div *ngFor="let gender of genders">
    <input type="radio" name="gender"> ngModel [value]="gender">{{ gender }}
</div>
```

## Setting and Patching Values
##### Approach 1
This approach overwrites the whole content.
##### component.ts
```
@ViewChild('f') signUpForm:NgForm;
OnNgInit(){
    this.signUpForm.setValue({
    // whole form values as key:value pairs
    username:'root',
    email:''
    ...
    })
}
```

##### Approach 2
Best when we want to overwrite specific values.
##### component.ts
```
@ViewChild('f') signUpForm:NgForm;
OnNgInit(){
    this.signUpForm.form.patchValue({
        userdata:{
            // user group
            username:'root'
        }
    })
}
```

## Resetting Form
It now just clears out the data but also all the settings are reset.
##### component.ts
```
@ViewChild('f') signupForm:NgForm;

//method 1
this.signupForm.reset();

//method 2
this.signupForm.setValue({});
```

# Reactive_Approach
To start make sure you have imported the **"Reactive Forms Module"** in app.module.ts (in imports array).
We don't need **"Forms Module"** here.
##### app.module.ts
```
import { ReactiveFormsModule } from "@angular/forms";
@NgModule({
    ...
    imports:[
    ... ,
    ReactiveFormsModule
    ]
})
```

## 1. Creating Form
We need to initalize it before the template is rendered.
FormGroup is the object that holds the entire form.
FormControls are the key value pairs.
##### component.ts
```
import { FormsGroup,FormControl } from '@angular/forms';
signupForm:FormGroup;
ngOnInit(){
    this.signupForm = new FormGroup({
        'username' : new FormControl(null)
        'email' : new FormContrl(null)
        // 1st arg -->> Intial state/Intial Value
        // 2nd arg -->> Single Validator or Array of Validators
        // 3rd arg -->> Potential Async Validator
    });
}
```

## 2. Syncing HTML and Form
##### component.html
```
<form [formGroup]="signupForm">
 // form synced
 <input formControlName="username">
 <input formControlName="email">
 <!-- use property binding while passing data, In case of strings use without square brackets -->
</form>
```

## 3. Submitting Form
##### component.html
```
<form [formGroup]="signupForm" (ngSubmit)="OnSubmit()">
    <!-- here we don't need reference as we created the form own our own. -->
    ...
</form>
```
##### component.ts
```
OnSubmit(){
    console.log(this.signupForm);
}
```

## 4. Adding Validations
Keep in mind we are just syncing our Form Controls with the HTML form.
We aren't configuring it in HTML, so we can't use HTML validations as we used in Template Driven Appoach.
##### component.ts
```
import { Validators } from '@angular/forms';
ngOnInit(){
    this.signupForm = new FormGroup({
        'username' : new FormControl(null, Validators.required)
        <!-- we need to pass the reference of Validators here we don't have to call it,
        angular will execute them whenever the form data will change -->
        'email': new FormControl(null,[Validators.required,Validators.email])
        });  
    })
}
```

## Getting Access of Form Control Data
##### component.html
```
<span *ngIf="!signupForm.get('username').valid">
// get takes path to that element
Error Occurred
</span>
```

## Reactive Grouping Control
##### component.ts
```
ngOnInit(){
    this.signupForm = new FormGroup({
        'userData' : new FormGroup({
            'username' : new FormControl(null, Validators.required),
            'email' : new FormControl(null, [Validators.required, Validators.email])
        })
        'password' : new FormControl(null,Validators.required)
    });
}
```
##### component.html
```
<form [formGroup]="signupForm">
    <div formGroupName="userData">
        <input formControlName="username">
        <span *ngIf="!signupForm.get('userData.username')"> Error Occured </span>
        <input formControlName="email">
    </div>
    <input formControlName="password">
</form>
```

## Form Array
For scenarios like adding Form Controls dynamically.
##### component.html
```
<div formArrayName="hobbies" >
    <h4> Add your Hobbies</h4>
    <button (click)="onAddHobby()">Add Hobby</button>
    <div *ngFor="let hobbyControl of signupForm.get('hobbies').controls;let i=index">
        <input [formControlName]="i" >
</div>
```
##### component.ts
```
ngOnInit(){
    ...
    'hobbies' : new FormArray([])
    // FormArray holds an array of Controls
}
onAddHobby(){
    const control = new FormControl(null);
    (<FormArray>this.signupForm.get('hobbies')).push(control)
}
```

## Custom Validators
##### component.ts
```
forbiddenUsername = ['root','superuser','user'];
ngOnInit(){
    this.signupForm(){
        'username' : new FormControl(null,[Validators.required,this.forbiddenNames.bind(this)])
    }
}

//custom validator
forbiddenNames(controls:FormControl):{[s:string]:boolean}{
    // so it should return something like {'root':false}
    if (this.forbiddenUsernames.indexOf(control.value)){
        return {'nameIsForbidden',true};
    }
    // now if the validators is sucessful you have to pass **nothing** or **null**
    // {'nameIsForbidden':false} won't work
    return null;
}
```
We need to bind "this" here as we are not calling the function.
But rather angular is doing so therefore we need to bind the classe's "this".


## Using Error Codes
Every control has a attached Error property.
We could use this error codes to output Error messages.
Error codes could be found via console.log();
##### component.html
```
<span *ngIf="!signupForm.get('userData.username').errors['nameIsForbidden']">
Error Occurred
</span>
```

## Custom Async Validators
##### component.ts
```
import { Observables } from 'rxjs/Observables';

ngOnInit(){
    this.signupForm = new FormGroup({
        'username' : new FormControl(null,Validators.this.forbiddenEmails)
    });
}

forbiddenEmails(control:FormControl) : Promise<any> | Observable<any>{
    const promise = new Promise<any>((resolve,reject)=>{
        setTimeout(()=>{
            if true
                resolve("Email is Forbidden");
            else
                resolve(null);
        },1500)
    }
}
```

## Status Changes and Value Changes
statusChanges and valueChanges are two observables.
##### component.ts
```
this.signupForm.valueChanges.subscribe(
    (value) => console.log(value);
    // executed every time value changes 
);   

this.signupForm.statusChanges.subscribe(
    (status) => console.log(status);
);
```

## setValue, patchValue and reset Form
##### component.ts
```
this.signupForm.setValue({
    // data as key:value here...
});

this.signupForm.patchValue({
    'userData':{
        'username' : 'root'
    }
})

this.signupForm.reset();
// note we could pass an object to reset to prevent some fields from getting cleared.
```
