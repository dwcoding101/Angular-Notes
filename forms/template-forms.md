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

by adding `required` attribute to the html form input elements it forces the input element to be required by the form. If the input is empty then a `ng-invalid` class is added to the element. It also can be seen in the element object under the valid key: value pair. 

## using CSS classes to style an invalid form
because angular adds class to the inputs elements you can tie into then to style the page

```css
input.ng-invalid.ng-touched {
  border: 1px solid red;
}
```
## Outputting Validation Error Messages

attach a local reference to the input element with `# ="ngModel` so you can reference it from within the html.

```html
 <input 
    type="email" 
    id="email" 
    class="form-control"
    ngModel
    name="email"
    required
    email
    #email="ngModel">
    <span class="help-block" *ngIf="!email.valid && email.touched" >Please enter a valid email!</span>
```

## NgModel Property binding to set default values 
```html
<div class="form-group">
    <label for="secret">Secret Questions</label>
    <select 
    id="secret" 
    class="form-control"
    [ngModel]="defaultQuestion"
    name="secret"
    >
        <option value="pet">Your first Pet?</option>
        <option value="teacher">Your first teacher?</option>
    </select>
</div>
```

and within tour component code 

```typescript
 defaultQuestion: string  = 'pet';
```
 this should select the pet value option of the select element.

 ## NgModel two way data binding

 ```html
 <div class="form-group">
    <textarea class="form-control" name="questionAnswer" rows="3" [(ngModel)]="answer"></textarea>
</div>
<p>Your reply: {{ answer }}</p>
 ```

 and within tour component code 

```typescript
 answer: string  = '';
```
 this this will update while the data is change in the textarea input and vica verca.