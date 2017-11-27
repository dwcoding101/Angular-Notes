# Form Templates

To use template driven forms you need to import 

```typescript
import { FormsModule } from '@angular/forms'
```
and include `FormsModule` in the imports part of the `@NgModule` decorator

```typescript
@NgModule({
    ...
    imports: [
        ...
        FormsModule
        ...
    ]
})
```
`FormsModule` takkes a html template form and converts it into something that the angular can use.

## Registering Input controls
To use template driven forms you also need register the input controls. This is done by adding `ngModel` and `name` attributes to the input elements of the from

```html
<div class="form-group">
    <label for="email">Mail</label>
    <input
        type="email"
        id="email"
        class="form-control"
        ngModel
        name="email"
    >
</div>
```

## Hooking into the Submit Button and passing the form to the function
the `ngSubmit` directive is used to listern to the submit button. So to run a function once the submit button is pushed event bind to the `ngSubmit` event.

```html
<form (ngSubmit)="onSubmit(f)" #f="ngForm">
</form>
```
inside you function 

```typescript
onSubmit(form: NgForm) {
    console.log(form);
}
```
remember to import `NgForm` with 


```typescript
import { NgForm } from '@angular/forms';
```
you can also gain access to the form with a `@ViewChild` decorator. Like so



```typescript
@ViewChild('f') form:NgForm;

onSubmit(form: NgForm) {
    console.log(form);
}

```

remember to import `ViewChild` with
```typescript
import { ViewChild } from '@angular/forms';
```

## Checking for Validty

by adding `required` attribute to the html form input elements it forces the input element to be required by the form. If the input is empty then a `ng-invalid` class is added to the element. It also can be seen in the element object under valid key vale pair. 