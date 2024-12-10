# angular-directive-custom
angular-directive [ predefined and customs ]

### Angular Custom Directive: A Step-by-Step Guide

In this guide, we will create an Angular custom directive that changes the background color of an element when the mouse hovers over it. This example uses `Renderer2` and `HostBinding`/`HostListener` to handle DOM manipulations in a safe and efficient way. We'll also provide step-by-step instructions for generating and using the directive in your Angular application.

---

### Step 1: **Generate a Custom Directive**

To begin, we need to generate a custom directive using Angular CLI. Run the following command in your terminal:

```bash
ng generate directive customdirective
```

This will generate a new directive called `CustomdirectiveDirective` in the `src/app` directory.

### Step 2: **Create the Custom Directive**

Now, modify the `CustomdirectiveDirective` to handle mouse events (`mouseenter` and `mouseleave`) and change the background color of an element.

#### **Code: `customdirective.directive.ts`**

```typescript
import { Directive, ElementRef, HostBinding, HostListener, OnInit, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appCustomdirective]',  // The selector used in the template
  standalone: true                   // This is a standalone directive (not tied to a module)
})
export class CustomdirectiveDirective implements OnInit {

  constructor(private elref: ElementRef, private render: Renderer2) { }

  private backgroundColor: string = "transparent";  // Default background color

  ngOnInit() {
    // You can initialize some styles here if needed (optional)
    /* element Ref example
     this.elref.nativeElement.style.backgroundColor = 'teal';
    */
    /*
    Renderer2 example
     this.render.setStyle(this.elref.nativeElement, 'background-color', 'red');
    */
  }

  // HostBinding allows binding of properties to the host element (the element the directive is applied to)
  @HostBinding('style.backgroundColor') private bgColor = "";

  // Mouse enter event (hover)
  @HostListener('mouseenter') mouseover(eventData: Event) {
    this.bgColor = 'cyan';  // Change the background color when mouse enters the element
  }

  // Mouse leave event (when mouse leaves the element)
  @HostListener('mouseleave') mouseLeave(eventData: Event) {
    this.bgColor = 'transparent';  // Reset the background color when mouse leaves the element
  }
}
```

### **Explanation:**

1. **`@HostBinding('style.backgroundColor')`**: This binds the `backgroundColor` property of the host element to the directive's `bgColor` variable. When `bgColor` changes, the background color of the host element automatically updates.

2. **`@HostListener('mouseenter')`**: This listens for the `mouseenter` event (when the mouse enters the element) and changes the `bgColor` to `cyan`.

3. **`@HostListener('mouseleave')`**: This listens for the `mouseleave` event (when the mouse leaves the element) and resets the `bgColor` to `transparent`.

### Step 3: **Declare the Directive in the Module**

You need to declare this directive in your `app.module.ts` or any other module where you plan to use it. Since this directive is standalone, it doesn't need to be added to the `declarations` array of the main module, but you must import it where necessary.

### Step 4: **Create the Component to Test the Directive**

Let's create a component to use this directive.

#### **Step 4.1: Generate a Component**

You can generate a component to test the directive:

```bash
ng generate component directive-test
```

#### **Step 4.2: Use the Custom Directive in the Component Template**

Inside the `directive-test.component.html` file, you can use the directive on various HTML elements like `div`, `p`, `span`, etc.

```html
<!-- Element Ref and Renderer2 Example -->
<div>
  <p appCustomdirective>Element Ref Example</p>
  <span appCustomdirective>Renderer2 Example</span>
</div>

<!-- HostListener Example -->
<h2 style="color:red">Mouse over the content to see the custom directive implemented</h2>
<p appCustomdirective>Paragraph Example (Element Ref)</p>
<span appCustomdirective>Span Example (Renderer2)</span>
```

#### **Step 4.3: Update the Component Class**

Make sure to import and declare the `CustomdirectiveDirective` in the component.

```typescript
import { Component } from '@angular/core';
import { CustomdirectiveDirective } from '../customdirective.directive';  // Import the directive

@Component({
  selector: 'app-directive-test',
  standalone: true,
  imports: [CustomdirectiveDirective],  // Add directive to the imports array
  templateUrl: './directive-test.component.html',
  styleUrls: ['./directive-test.component.css']
})
export class DirectiveTestComponent { }
```

### Step 5: **Modify the Root Component**

Now, let's use the newly created `DirectiveTestComponent` in the `app.component.html`.

#### **Code: `app.component.html`**

```html
<app-directive-test></app-directive-test>  <!-- Use the directive test component -->
<router-outlet></router-outlet>  <!-- For routing -->
```

### Step 6: **Running the Application**

Run your application with the following command:

```bash
ng serve
```

Then, navigate to `http://localhost:4200` in your browser. When you hover over the elements (`<p>` or `<span>`) with the `appCustomdirective` directive applied, the background color will change to cyan, and when you move your mouse away, it will revert to transparent.

---

### **README.md**

```markdown
# Angular Custom Directive with HostBinding and HostListener

This is a simple Angular application demonstrating the use of custom directives to manipulate DOM elements' styles. The directive listens for mouse events (`mouseenter` and `mouseleave`) to change the background color of the element.

## Features

- **`@HostBinding`**: Binds an element's style to a property in the directive.
- **`@HostListener`**: Listens for DOM events and changes the element's style dynamically.
- **Renderer2**: Manipulates DOM in a safe and platform-independent way.
  
## Step-by-Step Implementation

### 1. **Generate the Custom Directive**

To generate the custom directive, run the following Angular CLI command:

```bash
ng generate directive customdirective
```

This creates a new directive named `CustomdirectiveDirective`.

### 2. **Directive Code**

In the directive (`customdirective.directive.ts`), we define the following:

```typescript
import { Directive, ElementRef, HostBinding, HostListener } from '@angular/core';

@Directive({
  selector: '[appCustomdirective]'
})
export class CustomdirectiveDirective {
  @HostBinding('style.backgroundColor') private bgColor = '';

  @HostListener('mouseenter') mouseover() {
    this.bgColor = 'cyan';  // Change background color when mouse enters
  }

  @HostListener('mouseleave') mouseLeave() {
    this.bgColor = 'transparent';  // Reset background color when mouse leaves
  }
}
```

### 3. **Component for Testing**

Create a test component (`directive-test.component.ts`) to apply the directive:

```html
<div>
  <p appCustomdirective>Element Ref Example</p>
  <span appCustomdirective>Renderer2 Example</span>
</div>

<h2 style="color:red">Mouse over the content to see the custom directive implemented</h2>
<p appCustomdirective>Paragraph Example (Element Ref)</p>
<span appCustomdirective>Span Example (Renderer2)</span>
```

### 4. **Use the Directive in the Application**

In `app.component.html`, use the test component to display the directive in action:

```html
<app-directive-test></app-directive-test>
```

### 5. **Run the Application**

After implementing the directive and components, run the app:

```bash
ng serve
```
```

---

This guide should help you understand how to create a custom directive in Angular, use `HostBinding` and `HostListener`, and test it with a component. The README file provides an easy-to-follow overview of the steps involved.
