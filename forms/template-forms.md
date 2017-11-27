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

and within our component code 

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

 and within our component code 

```typescript
 answer: string  = '';
```
 this this will update while the data is change in the textarea input and vica verca.

 ## Grouping Form Controls
you can use `ngModelGroup` in to group the controls together for better organisation.  this is done like so.

 ```html

<div 
    id="user-data" 
    ngModelGroup="userData"
    #userData="ngModelGroup">
    <div class="form-group">
        <label for="username">Username</label>
        <input type="text" id="username" class="form-control" ngModel name="username" required>
    </div>
    <button class="btn btn-default" type="button">Suggest an Username</button>
    <div class="form-group">
        <label for="email">Mail</label>
        <input type="email" id="email" class="form-control" ngModel name="email" required email #email="ngModel">
        <span class="help-block" *ngIf="!email.valid && email.touched">Please enter a valid email!</span>
    </div>
</div>
<p *ngIf="!userData.valid && userData.touched" >User Data is invald!</p>

 ```

 note that the grouped part can also be referenced for validity.

 ## Handling Radio Buttons

this is how you handle radio buttons

 ```html
         <div class="radio" *ngFor="let gender of genders" >
    <label>
        <input 
            type="radio"
            name="gender"
            [ngModel]="male"
            [value]="gender"
            required>
            {{ gender }}
    </label>
</div>
```
## Setting and Patching Form Values
there are to method that you can use to programmatically change form data 

```typescript
suggestUserName() {
    const suggestedName = 'Superuser';
     this.signUpForm.setValue({
       userData: {
         username: suggestedName,
         email: ''
       },
       secret: 'pet',
       questionAnswer: 'Squeak',
       gender: 'male'
    })
    this.signUpForm.form.patchValue({
      userData: {
        username: suggestedName
      }
    });
  }
```
the `setValue` method need a exact representation of the from data, once its run all data in the form is changed.

the `patchValue` method you only represent the form data that you want to change.

## Using form data and resetting the form

```html
  <div class="row" *ngIf="submitted" >
    <div class="col-xs-12">
      <h3>Your Data</h3>
      <p>Username: {{ user.username }}</p>
      <p>Mail: {{ user.email }} </p>
      <p>Secret Question: Your first{{ user.secretQuestion }}</p>
      <p>Answer: {{ user.answer }}</p>
      <p>Gender: {{ user.gender }}</p>
    </div>
  </div>
```

```typescript
  user = {
    username: '',
    email: '',
    secretQuestion: '',
    answer: '',
    gender: ''
  }
  submitted: boolean = false
    ...
    ...

  onSubmit() {
    this.submitted = true;
    console.log(this.signUpForm);
    this.user.username = this.signUpForm.value.userData.username;
    this.user.email = this.signUpForm.value.userData.email;
    this.user.secretQuestion = this.signUpForm.value.secret;
    this.user.answer = this.signUpForm.value.questionAnswer;
    this.user.gender = this.signUpForm.value.gender;

    this.signUpForm.reset();
  }
```
you can also use `reset()` like the `setValue()` and pass the reset function a object of the values you want the form to be resetted to.