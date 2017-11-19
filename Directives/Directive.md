# Directives
There are two main type of directive.
* ## Attribute Directives 

  Attribute directives look like normal HTML attributes. They only effect the element they are attached to.
* ## Structural Directives
  
  Structural directives look like normal HTML attributes but with an a leading *. They will also effect a whole area of a DOM.

## Attribute Directives
to create a directive use the angular cli comand 

```terminal
ng generate directive directiveName
```

or just 

```terminal
ng g d directiveName
```

which will generate [this file](directive-name.directive.ts)

## Points of note 
* ### The import
  
  The import of the directive decorator

  ```typescript
  import { Directive } from '@angular/core';
  ```
* ### The Decoractor

  The decorator infront of the class also the attribute selector [] as a selector.
  ```typescript
  @Directive({
    selector: '[appDirectiveName]'
  })
  export class DirectiveNameDirective {
  ```
## Ways to effect the element that the directive is atached to.
 the first method to effect the element that a directive is attached to
 is to use dependancy injection. This is achieved by injecting the Element reference into the
 constructor. This is done by the follwing command
 ```typescript
   import { ElementRef } from '@angular/core';
 ```
 ```typescript
   constructor(private elementRef: ElementRef){}
 ```
 then within the ngOnInit function you could use elementRef to 
 ```typescript
   this.elementRef.nativeElement.style.backgroundColor = 'green'
 ```

 however this is poor coding practice, it should have used another depency injected property namely
 renderer2. which can then be used to effect the atrached element
 ```typescript
   import { ElementRef, Renderer2 } from '@angular/core';
 ```
  ```typescript
   constructor(private elementRef: ElementRef, private renderer: Renderer2) { }
 ```
  ```typescript
   this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue');
 ```
 Renderer2 has many functions that can be used to safely change the attached hmtl element information about the Renderer2 class can be found [here](https://angular.io/api/core/Renderer2).

 ## Host listener
  Another useful way to interact with the host element is to use an host listener decorator. This decorator is used to listern for any event on the attahed element, including custom events implemented in your own angular code. Just add the decorator to a function in the directive class. as shown in the following code.

 ```typescript
    import { HostListener } from '@angular/core';
 ```

 ```typescript
     @HostListener('mouseenter') mouseOver(eventData: Event) {
     }
 ```
  the function is run every time the event is fired.

## Host binding
  Another useful way to interact with the host element is to use an host binder decorator. This decorator bindings a property of the Host to the directive. It is attachived by using the following code.

```typescript
import { HostBinding } from '@angular/core';
```

```typescript
@HostBinding('style.backgroundColor') backgroundColor: string = 'transparent';
```
  this decorator allows you to bind directly any of the hosts properties. Include custom proteries that you may have created in angular.

## Binding to Directive properties 
Using an @Input decorator you can add custom properties to the Host element. These properties can be set in the Host element and passed to the directive. Here is the code.

```typescript
import { Input } from '@angular/core';
}
 ```

```typescript
@Input() defaultColor: string = 'red';
```

this would allow you to add a property to the host element as so

```html
<host directiveName [defaultColor]="'red'"></host>
```