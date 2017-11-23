# Route Registering

for simple routes you can register them directly in the `app.module.ts`, this can be done by 
creating a `const` variable containing the routing infomation.

```typescript
const appRoutes: Routes = [
    { path: 'pathforRoute' : component: ComponentName },
    { path: 'pathforRoute1' : component: ComponentName1 },
    { path: 'pathforRoute2' : component: ComponentName2 }
];
```

remember to add the imports of Routes like so

```typescript
import { Routes } from '@angular/router'
```

then add `RouterModule.forRoot(appRoutes)` to the imports of the `@NgModules` like so

```typescript
...
imports: [
    RouterModule.forRoot(appRoutes)
],
```

remember to add the imports of RouterModule like so

```typescript
import { RouterModule } from '@angular/router'
```
## Where to load the routed page?
used the directive in your html code to inform the router where to plave the routed page.
```html
<router-outlet></router-outlet>
```