# Route Registering

for simple routes you can register them directly in the `app.module.ts`, this can be done by 
creating a `const` variable containing the routing infomation.

```typescript
const appRoutes: Routes = [
    { path: 'pathforRoute' : component: ComponentName }
];
```

remember to add the imports of Routes like so

```typescript
import { Routes } from '@angular/router'
```