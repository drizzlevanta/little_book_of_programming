# Server-side Rendering

## SSR-friendly

When something is SSR-friendly, it means it:

- Runs without browser-only APIs (like window, localStorage, etc.)
- Can run on the server and the client without issues
- Can hydrate correctly — meaning:
  - HTML rendered on the server matches what's rehydrated in the browser
  - No UI flickering or double-fetching

## Client hydration

Hydration is the process where the browser takes the static HTML rendered by the server and turns it into a fully interactive Angular app. Steps:

1. Server renders your Angular component into HTML.
2. That HTML is sent to the browser and displayed instantly — this is called First Contentful Paint (FCP).
3. Angular's runtime downloads the JavaScript bundle, boots up, and "hydrates" the existing HTML:
   - Connects event listeners
   - Initializes signals or reactive data
   - Makes the page interactive

## Issues with hydration

1. Mismatches between server and client
   If the HTML generated on the server doesn't match what Angular renders on the client, you get hydration errors or visual flickering.

2. Double-fetching

   If you don't use TransferState or HttpResources, the client re-fetches data that was already fetched on the server — bad for performance.

## The double-fetching problem

When you use HttpClient + RxJS without transfer state, Angular doesn't know the server already fetched the data, so it re-runs the `HttpClient` calls on the client during hydration.

This causes:

- Duplicate HTTP requests - one on the server, one again on the client.
- Flash of loading states: UI might flicker: HTML shows data → browser loads → shows "Loading..." → then shows data again.
- Wasted bandwidth/performance. Unnecessary API calls and CPU usage

## `HttpResources`

It integrates with Angular's transfer state mechanism.
When used with `provideHttpResources()`, it:

- Fetches data on the server
- Embeds that data in the HTML response
- Prevents duplicate HTTP requests when the client bootstraps

This means:

- No flash of loading state
- No duplicate fetch on the client
- Seamless hydration

### When not to use `HttpResources`

- If your fetch should only happen under very specific conditions (e.g., after user clicks a button), `HttpResources`' eagerness can become a problem.
- HttpResources is designed primarily for fetching (i.e., GET). It doesn't have built-in support for POST, PUT, DELETE with side effects, complex transaction logic, advanced request chaining, low-level control (custom headers, retries, timeout, events).
- It doesn't support cross-component or global cache coordination.
