# Directives

In Angular, a directive is a building block used to extend the functionality of HTML elements or create reusable components. Directives allow you to attach specific behaviors or manipulate the DOM (Document Object Model) within your Angular application.

There are three types of directives in Angular:

Component Directives: Components are directives with a template. They consist of a view, which is the template HTML, and a class, which defines the behavior and properties of the component. Components are the most commonly used directive type in Angular.

Attribute Directives: Attribute directives are used to change the appearance or behavior of an existing element or component. They are applied by using an attribute on the HTML element. Examples of attribute directives in Angular include ngStyle and ngClass.

Structural Directives: Structural directives are used to change the structure of the DOM by adding or removing elements. They are denoted by an asterisk (*) preceding the directive attribute in the HTML markup. Examples of structural directives in Angular include *ngIf, *ngFor, and *ngSwitch.

## ng-container

ng-container is a logical directive that allows you to group elements in a template but doesn’t itself get rendered in the DOM. One common use case of `<ng-container>` is alongside the `*ngIf` structural directive. By using the special element we can produce very clean templates easy to understand and work with.

Multiple structural directives cannot be used on the same element; if you need to take advantage of more than one structural directive, it is advised to use an `<ng-container>` per structural directive.

## ng-template

If you add a ng-template tag to your template, it and everything inside it will be replaced by a comment. It can be used to define an `else` case:

```html
<div>
  Hello word!
  <div *ngIf="false else content">Shouldnt be displayed</div>
</div>

<ng-template #content> Should be displayed </ng-template>
```

The `<ng-template>` element defines a block of content that a component can render based on its own logic. A component can get a reference to this template content, or TemplateRef, by using either the @ContentChild or @ContentChildren decorators.
