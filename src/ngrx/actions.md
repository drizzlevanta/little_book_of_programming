# Actions

- Event-Driven - capture events not commands as you are separating the description of an event and the handling of that event.
- Action describes the event, reducer handles the events
- Upfront - write actions before developing features to understand and gain a shared knowledge of the feature being implemented.
- Actions are inexpensive to write, so the more actions you write, the better you express flows in your application.
- The createActionGroup function returns a dictionary of action creators where the name of each action creator is created by camel-casing the event name, and the action type is created using the "[Source] Event Name" pattern.
- Define actions by automatically using a camel case event name, instead of string of event names.

  ```typescript
  import { createActionGroup, props } from "@ngrx/store";

  import { Product } from "./product.model";

  export const ProductsApiActions = createActionGroup({
    source: "Products API",
    events: {
      productsLoadedSuccess: props<{ products: Product[] }>(),
      productsLoadedFailure: props<{ errorMsg: string }>(),
    },
  });
  ```
