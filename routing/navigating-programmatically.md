# Navigating Programatically

For this example If you create a button that runs a function within your component.

```html
 <button (click)="onNavigate"> click Me to Navigate </button>
```

and then within your component class inject the Router 


```typescript
...
constructor(private router: Router){}
...
onNavigate() {
  this.router.navigate(['navigate','to','me']);
}
```
this will produce the absolute path `/navigate/to/me

remember to import the Router
```typescript
import { Router } from '@angular/router'
```

## Using Relative Paths in Programmatic Navigation
This is very similar to above but with some variables to the navigate function

```typescript
...
constructor(private router: Router,
            private route: ActivatedRoute){}
...
onNavigate() {
  this.router.navigate(['navigate','to','me'],{relativeTo: this.route});
}
```
remember to import the Router
```typescript
import { ActivatedRoute } from '@angular/router'
```