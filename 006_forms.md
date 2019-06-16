Angular gives use the JSON representation of the HTML form.

Here we have two approaches to handle forms:
- **Template Driven Approach** (Angular infers the FORM object from the DOM)
- **Reactive Approach** (Form is created programatically and synchronized with the DOM)

**NOTE:** We don't use "**action**" on the form here as we are not going to directly submit it to the server.
Rather we will handle it using angular and sent it to server via Angular's HTTP methods.

# Template Drive Approach
-------
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
Angular automatically creates the Javascript respresentation of the form when it detects **<form>** selector in your code.
So you can think of that form element serving as a selector for some angular directive which then creates the JS representation for you. Also, angular won't automatically detect your "<input>" and other elements of your form.

