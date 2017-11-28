# Reactive Forms

## The setup 
To use reactive forms you must first import the `ReactiveFormsModule` into the app module. Like so.

```Typescript
import { ReactiveFormsModule } from '@angular/forms';
...
@NgModule({
  declarations: [
    ...
  ],
  imports: [
    ...
    ReactiveFormsModule
    ...
  ],
  ...
})
export class AppModule { }
```

also you need a `FormGroup` variable in your component that is implementing the from.
```Typescript
import { FormGroup } from '@angular/forms';
...
@Component({
  selector: 'appFormSignup'
})
export class AppModule {

    signupForm: FormGroup
    
}
```

## Creating a form in code 
The next stage is to create a form in code using the `FormGroup` variable all ready initalised. with the use of `FormControl`,
this should be done before the template in rendered, ideally in the `ngOnInit()`

```typescript
import { FormControl, FormGroup } from '@angular/forms';
...
  signupForm: FormGroup;

  ngOnInit() {
    this.signupForm = new FormGroup({
      'username': new FormControl(null),
      'email': new FormControl(null),
      'gender': new FormControl('male')
    });
  }
 ...
```

## Syncing Html and Form
To Sync Html with form code, add property bind`[formGroup]="signupForm"` to the `<form>` element and in the form controls
property bind with `formControlName="username"`

<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <form [formGroup]="signupForm">
        <div class="form-group">
          <label for="username">Username</label>
          <input
            type="text"
            id="username"
            formControlName="username"
            class="form-control">
        </div>
        <div class="form-group">
          <label for="email">email</label>
          <input
            type="text"
            id="email"
            formControlName="email"
            class="form-control">
        </div>
        <div class="radio" *ngFor="let gender of genders">
          <label>
            <input
              type="radio"
              formControlName="gender"
              [value]="gender">{{ gender }}
          </label>
        </div>
        <button class="btn btn-primary" type="submit">Submit</button>
      </form>
    </div>
  </div>
</div>