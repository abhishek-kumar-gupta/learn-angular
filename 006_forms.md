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
