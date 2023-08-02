# Content Projection

## Single slot

Use `<ng-content>` to project content into another component. Example:

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-zippy-basic",
  template: `
    <h2>Single-slot content projection</h2>
    <ng-content></ng-content>
  `,
})
export class ZippyBasicComponent {}
```

users of this component can project their own content

```html
<app-zippy-basic>
  <p>Is content projection cool?</p>
</app-zippy-basic>
```

## Multi-slot

You can add multiple contents slots and use a CSS selector to determine which ng-content tag to project your content into.

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "app-zippy-multislot",
  template: `
    <h2>Multi-slot content projection</h2>

    Default:
    <ng-content></ng-content>

    Question:
    <ng-content select="[question]"></ng-content>
  `,
})
export class ZippyMultislotComponent {}
```

and in consumer of the component:

```html
<app-zippy-multislot>
  <p question>Is content projection cool?</p>
  <p>Let's learn about content projection!</p>
</app-zippy-multislot>
```

## Conditional content projection

If your component needs to conditionally render content, or render content multiple times, you should configure that component to accept an `<ng-template>` element that contains the content you want to conditionally render.

Using an `<ng-content>` element in these cases is not recommended, because when the consumer of a component supplies the content, that content is always initialized, even if the component does not define an `<ng-content>` element or if that `<ng-content>` element is inside of an ngIf statement.

You can use `ngTemplateOutlet` directive to render a given `<ng-template>` element.

Template component:

```html
<div *ngIf="expanded" [id]="contentId">
  <ng-container [ngTemplateOutlet]="content.templateRef"></ng-container>
</div>
```

Content component:

```html
<ng-template appExampleZippyContent>
  It depends on what you do with it.
</ng-template>
```

Logic Angular will use when it encouters the `appExampleZippyContent`. This tells Angular to instantiate a template reference.

```typescript
@Directive({
  selector: "[appExampleZippyContent]",
})
export class ZippyContentDirective {
  constructor(public templateRef: TemplateRef<unknown>) {}
}
```
