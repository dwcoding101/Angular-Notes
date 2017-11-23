# Passing parameters to routes

```typescript
const appRoutes: Routes = [
    { path: 'users' : component: UsersComponent },
    { path: 'users/1' : component: UserComponent },
    { path: 'users/2' : component: UserComponent },
];
```
is not optimal

instead use

```typescript
const appRoutes: Routes = [
    { path: 'users' : component: UsersComponent },
    { path: 'users/:id' : component: UserComponent },
];
```
this will enable the router to refrence that part of the address path as a variable. it mportant path is the `:` part.

## Fetching Route Parameters
Within the `UserComponent` class the following coding is used to retrieve the `:id` parameter

```typescript
...
id: number;
...
constructor(private router: Router,
            private route: ActivatedRoute){}
...
ngOnInit() {
  this.id = +this.route.snapshot.params['id']

  this.route.params.subscribe (
     (params)=>{
          this.id = + params['id'];
  });
}
```
the + at the start is use to convert the returned string to a number, the route from `this.route.snapshot.params['id']`
returns a string.

also the subscribe part is used to update the id if the id chages at some point in the future

remember to import the ActivatedRouter
```typescript
import { ActivatedRoute } from '@angular/router'
```