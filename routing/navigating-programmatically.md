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

## programmatically adding queryParams and fragments 

```typescript
...
constructor(private router: Router){}
...
onNavigate(id: number) {
  this.router.navigate(['navigate','to','me',id],{queryParams: {allowEdit:'1'},fragment: 'loading'});
}
```
if you pass 5 to the function this will produce the absolute path `/navigate/to/me/5?allow=1#loading`

remember to import the Router
```typescript
import { Router } from '@angular/router'
```
## Retrieving Querry Parameters and Fragments

```typescript
...
constructor(private router: Router,
            private route: ActivatedRoute){}
...
ngOnInit() {
  console.log( this.route.snapshot.queryParams['paraName'])
  console.log( this.route.snapshot.fragment);
  
  //reactive way if theres any chance of the queryParams changing while in the component
  this.route.queryParams.subscribe(
      (params:Params)=>{
          console.log('the parameters have changed to' + params['paraName'])
      }
  )

  // and for fragment changes
  this.route.fragment.subscribe(
      (fragment: string)=>{
          console.log('the fragment has changed to' + fragment);
      }
  )
  
  this.router.navigate(['navigate','to','me'],{relativeTo: this.route});
}
```
remember to import the Router
```typescript
import { ActivatedRoute, Params } from '@angular/router'
```