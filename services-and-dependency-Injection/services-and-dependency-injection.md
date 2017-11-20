# Services and dependancy injection
A service is simple a class that provides a service that can be used with in your
angular code. to create a service class you simple create a normal exported typescript class.

The name convention for a service class file is `named-service.service.ts` and the file can be 
places any where with in your project, althrough it is bes pastice to place the file in a meaning 
full place. 

To create the class the class is used.
```typescript
export class NamedServiceService {

}
```
notice the matching camelcase the the class name to filename.

the Service should have functions / properties  that can be used by users of the service.

adding in some simple functions and data for the example

```typescript
export class NamedServiceService {
    
    status: boolean = ture;

    logStatus(note: string) {
        console.log('This is a note : ' + note + ' with the status of: ' + this.status);
    }
}
```
 and there you have it a simple service.

 ## Using a service and Hyarchical Injection

 To use the service you need to inject it into the code where you wish to use it. This is done though the constructor and the use of the provider in the Class decorator. like so.

 ```typescript
 import { Component } from '@angular/core';

import { NamedServiceService } from '../named-service.service';

@Component({
  selector: 'app-component',
  providers: [NamedServiceService]
})
export class Component {
 
  constructor(private namedServiceService: NamedServiceService){}

  onSetTo(status: string) {
    this.namedServiceService.logStatus('hote');
  }
}
```
remember to import the Service with 
```typescript
import { NamedServiceService } from '../named-service.service';
```
and it into the decorator

 ```typescript
@...({
  providers: [NamedServiceService]
})
```
 Everytime that you provider class appears in the decorator you create an instances the service. Once the new service has been created it can be accessed by injecting into the component. Depending on whether that you need one or many instances of the same service. So if you require one service place it at the top of the hierarchical structure of the dom. If you require many then place it within the provider of each decorator.  The constructor injector will firstly look within it current compoents  providers and then look in its parent providers and so on up the hierarchy. This is known as a hierarchical injection.

and inject it into the classes constructor.
```typescript
  constructor(private namedServiceService: NamedServiceService){}
```

## Injecting a service into a service.

You can even inject a service into service, this is done by using a injectable decorator on the service that you are should you inject into. like so

```typescript
import { Injectable} from '@angular/core'

@Injectable()
export class NamedServiceService {
    
    status: boolean = ture;

    constructor( private newService: NewService) {}

    logStatus(note: string) {
        console.log('This is a note : ' + note + ' with the status of: ' + this.status);
    }
}
```
also remember to at the new Service to the app.module.ts  @ngModule({provider}) other wise the service will no be injected.
