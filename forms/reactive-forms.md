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
      <form [formGroup]="signupForm" 
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

## Submitting the form
Use event binding to reaction to form submitions `(ngSubmit)="onSubmit()"` and uses the formGroup variable
to interact with the data.

```typescript
  onSubmit(){
    console.log(this.signupForm);
  }
  
```
## Adding Validation 
To add control validation add `Validators` as the second parameter of the `FormControl` class.

```typescript
import { FormControl, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  genders = ['male', 'female'];
  signupForm: FormGroup;

  ngOnInit() {
    this.signupForm = new FormGroup({
      'username': new FormControl(null, Validators.required),
      'email': new FormControl(null, [Validators.required, Validators.email]),
      'gender': new FormControl('male')
    });
  }
```  

## Getting access to the controls
use the `FormGroup` variable and `getValue()` to access the `FromControls`
```html
<span class="help-block" *ngIf="!signupForm.valid && signupForm.touched">Please enter a valid data</span>
<span class="help-block" *ngIf="!signupForm.getValue('email').valid && signupForm.getValue('email').touched">Please enter a email data</span>
```

## Grouping controls

Staring with the froms code add another layer of `FormGroup`

```typescript
  ngOnInit() {
    this.signupForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, Validators.required),
        'email': new FormControl(null, [Validators.required, Validators.email]),  
      }),
      'gender': new FormControl('male')
    });
  }
```
then with the forms html add `formGroupName` to a `<div>` that encapsulates the form controls

```html
<div formGroupName="userData">
  <div class="form-group">
    <label for="username">Username</label>
    <input type="text" id="username" formControlName="username" class="form-control">
    <span class="help-block" *ngIf="!signupForm.get('userData.username').valid && signupForm.get('userData.username').touched">Please enter a valid username!</span>
  </div>
  <div class="form-group">
    <label for="email">email</label>
    <input type="text" id="email" formControlName="email" class="form-control">
    <span class="help-block" *ngIf="!signupForm.get('userData.email').valid && signupForm.get('userData.email').touched">Please enter a valid email!</span>
  </div>
</div>
```

## Array of Form Controls (FormArray)

```html
<div formArrayName="hobbies">
  <h4>your hobbies</h4>
  <button class="btn btn-default" type="button" (click)="onAddHobby()">Add Hobby</button>
  <div *ngFor="let hobbyControls of signupForm.get('hobbies').controls; let i = index" class="form-group">
    <input type="text" class="form-control" [formControlName]="i">
  </div>
</div>
```        

```typescript
  ngOnInit() {
    this.signupForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, Validators.required),
        'email': new FormControl(null, [Validators.required, Validators.email]),  
      }),
      'gender': new FormControl('male'),
      'hobbies': new FormArray([])
    });
  }

  onAddHobby(){
    const control = new FormControl(null,Validators.required);
    (<FormArray>this.signupForm.get('hobbies')).push(control);
  }
```

## Creating Custom Validators
```typescript
  signupForm: FormGroup;
  forbiddenUsernames = ['Chris', 'Anna'];


  ngOnInit() {
    this.signupForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this) ]),
        'email': new FormControl(null, [Validators.required, Validators.email]),  
      }),
      'gender': new FormControl('male'),
      'hobbies': new FormArray([])
    });
  }
  
  forbiddenNames(control: FormControl): {[s: string]:boolean} {
    if(this.forbiddenUsernames.indexOf(control.value) != -1) {
      return {'nameIsForbidden': true};
    }
    return null;
  }
```

## Using Error Codes

```html
<span>
  <span *ngIf="signupForm.get('userData.username').errors['nameIsForbidden']">This Name is Invalid</span>
  <span *ngIf="signupForm.get('userData.username').errors['required']">This field is required</span>
</span>
```

## Custom Async Validators

```typescript
  ngOnInit() {
    this.signupForm = new FormGroup({
      'userData': new FormGroup({
        'username': new FormControl(null, [Validators.required, this.forbiddenNames.bind(this) ]),
        'email': new FormControl(null, [Validators.required, Validators.email], this.forbiddenEmails),  
      }),
      'gender': new FormControl('male'),
      'hobbies': new FormArray([])
    });
  }
  ...
  forbiddenEmails(control: FormControl): Promise<any> | Observable<any> {
    const promise = new Promise<any>((resolve, reject) => {
      setTimeout(()=>{
        if (control.value === 'test@test.com') {
          resolve({'emailIsForbidden': true}); // error pass the error back.
        } else {
          resolve(null); // no errors
        }
      },1500);
    });
    return promise;
  }
```

## Reacting to Status or Values Changes
```typescript
this.signupForm.valueChanges.subscribe(
  (value) => {
    console.log(value);
  }
);

this.signupForm.statusChanges.subscribe(
  (status) => {
    console.log(status);
  }
);
```

## Update the form

you can use the `reset()` , `setValue({})` and `patchValue({})` as in the template driven approach?