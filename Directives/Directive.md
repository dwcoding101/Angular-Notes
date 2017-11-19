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
   constructor(private elementRef: ElementRef, private renderer: Renderer2) { }
 ```

@HostBinding('style.backgroundColor') backgroundColor: string = 'transparent';

  constructor(private elementRef: ElementRef, private renderer: Renderer2) { }

  ngOnInit() {
    // this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue');
  }

  @HostListener('mouseenter') mouseOver(eventData: Event) {
    //this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'blue');
    this.backgroundColor = 'blue';
  }

  @HostListener('mouseleave') mouseLeave(eventData: Event) {
    // this.renderer.setStyle(this.elRef.nativeElement, 'background-color', 'transparent');
    this.backgroundColor = 'transparent';
  }
