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

## Hooking into the Submit Button
the `ngSubmit` directive is used to listern to the submit button. So to run a function once the submit button is pushed event bind to the `ngSubmit` event

```html
<form (ngSubmit)="onSubmit()">
</form>
```
