Angular gives use the JSON representation of the HTML form.

Here we have two approaches to handle forms:
- **Template Driven Approach** (Angular infers the FORM object from the DOM)
- **Reactive Approach** (Form is created programatically and synchronized with the DOM)

**NOTE:** We don't use "**action**" on the form here as we are not going to directly submit it to the server.
Rather we will [template](#Reactive) handle it using angular and sent it to server via Angular's HTTP methods.

# Template Drive Approach
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



# Reactive
