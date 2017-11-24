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
this will output the component specifed in the appRoutes varaible the app.

## Navigating with Router Links Directive
Use the `routerLink` directive to navigate like so 

```html
<a routerLink="/path">Link me to </a>
```
you can also use

```html
<a [routerLink]="['/path',3,'edit]">Link me to </a>
```

## Relative and absoulute
relative to is without a `/` which basicly adds the routerLink path onto the current path. 

## Styling Active Router Links

use the directive `routerLinkActive` to add a css class to the current active router path.

```html
<a routerLinkActive="activeClass">Link me to </a>
```


it anaylsis the current  loaded path and then checks which links lead to a route that uses this path. 
use  `routerLinkActiveOptions` to tailor the routerLinkActive directive
```html
<a routerLinkActive="activeClass"
   routerLinkActive="{exact: true}">Link me to </a>
```
this tells the `routerLinkActive` to only use the exact path.

## Nested Routing

```typescript
const appRoutes: Routes = [
    { path: '' : component: HomeComponent },
    { path: 'users' : component: UsersComponent, children: [
        {path:':id/:name' , component: UserComponent}
    ] },
    { path: 'servers' : component: ServersComponent, children: [
        {path:':id' , component: ServerComponent},
        {path:':id/edit' , component: EditServerComponent}
    ] }
];
```
with in `ServersComponent` there should be a `<router-outlet>` directive to indicate where the child path should be 

## Redirecting whildcards (404 error) 

```typescript
const appRoutes: Routes = [
    { path: '' : component: HomeComponent },
    { path: 'users' : component: UsersComponent, children: [
        {path:':id/:name' , component: UserComponent}
    ] },
    { path: 'servers' : component: ServersComponent, children: [
        {path:':id' , component: ServerComponent},
        {path:':id/edit' , component: EditServerComponent}
    ] },
    { path: 'not-found' : component: PageNotFoundComponent },
    { path: '**' : redirectTo: '/not-found' },
];
```
make sure that the ** path is the last route in the list;
