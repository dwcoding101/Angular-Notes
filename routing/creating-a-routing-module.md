# Creating a Routing Module
It is good pratice to beter organise the app routing by seperating the routing into its own module.

this is done by creating a file normally `app-routing.module.ts` and this is where the routing is implemented

the following code is an example of what a app-routing.module.ts file should look like.
```typescript
import { ServerResolver } from './servers/server/server-resolver.service';
import { ErrorPageComponent } from './error-page/error-page.component';
import { CanDeactiveGuard } from './servers/edit-server/can-deactivate-guard.service';
import { NgModule } from '@angular/core';

import { PageNotFoundComponent } from './page-not-found/page-not-found.component';
import { EditServerComponent } from './servers/edit-server/edit-server.component';
import { ServerComponent } from './servers/server/server.component';
import { ServersComponent } from './servers/servers.component';
import { UserComponent } from './users/user/user.component';
import { UsersComponent } from './users/users.component';
import { Routes, RouterModule} from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AuthGuard } from './auth-guard.service';



const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent, children: [
    { path: ':id/:name', component: UserComponent },
  ]},
  { path: 'servers', 
    // canActivate: [AuthGuard], 
    canActivateChild:[AuthGuard],
    component: ServersComponent , 
    children: [
    { path: ':id', component: ServerComponent, resolve:{server: ServerResolver}},
    { path: ':id/edit', component: EditServerComponent, canDeactivate:[CanDeactiveGuard] }  
  ]},
  // {path: 'not-found', component: PageNotFoundComponent },
  {path: 'not-found', component: ErrorPageComponent, data:{errorMessage: 'Page not found!'} },
  {path: '**', redirectTo: '/not-found' }
];


@NgModule({
  imports: [
    RouterModule.forRoot(appRoutes)
  ],
  exports: [ 
    RouterModule
   ]
})
export class AppRoutingModule {}
```
the important parts of the app routing module are the appRoutes const that describe the apps routing and the decorator at the end where the module imports RouterModule remembering to include the appRoutes in the forRoot function. and the it is exported for use out side the module.

# Using the module within your App

```typescript
import { AppRoutingModule } from './app-routing.module';
..
@NgModule({
  declarations: [
  ...
  imports: [
    ...
    AppRoutingModule
   
  ]
})
export class AppModule { }
```
and because we have already add appRoutes to the forRoot() with in the module and that it exports the `RouterModule` the app will now use the appRoutes describe within the `app-routing.module.ts` file.