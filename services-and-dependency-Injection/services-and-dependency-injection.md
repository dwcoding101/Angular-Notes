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
