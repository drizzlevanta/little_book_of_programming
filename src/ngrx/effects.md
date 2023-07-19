# Effects

- Effects isolate side effects from components, allowing for more pure components that select state and dispatch actions.
- Effects are long-running services that listen to an observable of every action dispatched from the Store.
- Effects filter those actions based on the type of action they are interested in. This is done by using an operator.
- Effects perform tasks, which are synchronous or asynchronous and return a new action.
- In a traditional service based application, the component has to use service to perform a side-effect (reaching out to an external API to fetch movies), change the state of the movies within the component.
- Effects when used along with Store, decrease the responsibility of the component.
- Effects handle external data and interactions, allowing your services to be less stateful and only perform tasks related to external interactions, react to store state changes,
- Effects can listen to not only actions, but any RxJs stream. This can handle periodic things like (refreshing of an auth token, uploading of logs), or reacting to user interaction.
- Effects contains these parts: an action$ observable stream, ofType operator to filter which action to listen to,
- Example:

  ```typescript
  login$ = createEffect(() =>
    this.actions$.pipe(
      ofType(LoginPageActions.login),
      map((action) => action.credentials),
      exhaustMap((auth: Credentials) =>
        this.authService.login(auth).pipe(
          map((user) => AuthApiActions.loginSuccess({ user })),
          catchError((error) => of(AuthApiActions.loginFailure({ error })))
        )
      )
    )
  );
  ```

- You can create a functional effect outside a class, using the `functional: true` flag. If the `dispatch: false` flag is set, the effect doesn't return actions.

  ```typescript
  import { inject } from "@angular/core";
  import { catchError, exhaustMap, map, of, tap } from "rxjs";
  import { Actions, createEffect, ofType } from "@ngrx/effects";

  import { ActorsService } from "./actors.service";
  import { ActorsPageActions } from "./actors-page.actions";
  import { ActorsApiActions } from "./actors-api.actions";

  export const loadActors = createEffect(
    (actions$ = inject(Actions), actorsService = inject(ActorsService)) => {
      return actions$.pipe(
        ofType(ActorsPageActions.opened),
        exhaustMap(() =>
          actorsService.getAll().pipe(
            map((actors) => ActorsApiActions.actorsLoadedSuccess({ actors })),
            catchError((error: { message: string }) =>
              of(
                ActorsApiActions.actorsLoadedFailure({
                  errorMsg: error.message,
                })
              )
            )
          )
        )
      );
    },
    { functional: true }
  );

  export const displayErrorAlert = createEffect(
    () => {
      return inject(Actions).pipe(
        ofType(ActorsApiActions.actorsLoadedFailure),
        tap(({ errorMsg }) => alert(errorMsg))
      );
    },
    { functional: true, dispatch: false }
  );
  ```

- Effects start running immediately after instantiation to ensure they are listening for all relevant actions as soon as possible.
- If additional metadata is needed to perform an effect besides the initiating action's type, we should rely on passed metadata from an action creator's props method.
- However, there may be cases when the required metadata is only accessible from state. When state is needed, the RxJS withLatestFrom or the @ngrx/effects concatLatestFrom operators can be used to provide it. Example:
  ```typescript
  addBookToCollectionSuccess$ = createEffect(
    () =>
      this.actions$.pipe(
        ofType(CollectionApiActions.addBookSuccess),
        concatLatestFrom((action) =>
          this.store.select(fromBooks.getCollectionBookIds)
        ),
        tap(([action, bookCollection]) => {
          if (bookCollection.length === 1) {
            window.alert("Congrats on adding your first book!");
          } else {
            window.alert("You have added book number " + bookCollection.length);
          }
        })
      ),
    { dispatch: false }
  );
  ```
- Using other observable sources for effects. Example: creating an effect that listens to the document click event:
  ```typescript
  trackUserActivity$ = createEffect(
    () =>
      fromEvent(document, "click").pipe(
        concatMap((event) => this.userActivityService.trackUserActivity(event))
      ),
    { dispatch: false }
  );
  ```
