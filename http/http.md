# Http Resquests 
To use the http you need to import `Http` from the `@angular/http`

```typescript
import { Http } from '@angular/http';
```

to store use http the following code is used

```typescript
const headers = new Headers({'Content-Type': 'application/json'});

return this.http.put('https://address', 
                              servers, 
                              {headers: headers} );
```

the header is optional. 

to get data from a http service you can use the get function

