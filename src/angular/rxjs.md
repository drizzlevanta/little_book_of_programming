# Use of RxJS in Angular

There are various ways of using observables in Angular. Below are some of the patterns I've used:

- rxjs/observables
  - three types of observables/subscription model:
    - Entity/state observable, directly subscribe in async pipe
    - Side effect observable, put side effects in the tap operator, and subscribe all at once:
    ```typescript
    scheduled(
      [this.enabledPanelId$, this.onStatus$, this.refreshTime$],
      asyncScheduler
    )
      .pipe(mergeAll(), takeUntil(this.stop$))
      .subscribe();
    ```
    - UI event emitter and handler, use a combination of subject and async pipe
    - subscription in service
- If accessing the store from the component constructor, consider adding:

  ```typescript
  this.settingsMode$ = this.uiQuery?.settingsMode$.pipe(
    tap({
      next: (appMode) => {
        this.isPanelEditMode =
          appMode === SettingsModes.EDIT || appMode === SettingsModes.ADDNEW;
      },
    })
  );
  ```

- Use dedicated view subscription variable:

  ```html
  *ngIf="{ changeTheme: onChangeTheme$ | async, onSave: onSave$ | async, } as
  obs"
  ```
