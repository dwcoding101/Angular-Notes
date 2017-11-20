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

 ## Using a service

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
Every time that a provider class appears in the decorator a new instance of the serves is created.
Once the new service has ben created it a be accessed by child decents of the component. So depending on 
whether you need one or many instance of the same service converns the placement of the provider call. 

and inject it into the classes constructor.
```typescript
  constructor(private namedServiceService: NamedServiceService){}
```


