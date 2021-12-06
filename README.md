# Full-stack Svelte

A little framework on top of SvelteKit that's great if you're running a node server and want tight integration between server and client code.

This framework makes it so you can do this in your `route/*.svelte` files:

```svelte
<script context="module" endpoint>
  // anything in here runs on server requests and is not included on the client
  export async function get(request) {
    return {
      body: {
        name: 'Glen'
      }
    }
  }
</script>

<!-- everything else runs on the client -->
<div>Hello {$d.name}</div>
```

Full-stack Svelte will generate all the endpoints for you in `/generated_endpoints`. `$d` is a store that contains the data returned from the server so you don't have to write a bunch of `export` statements in your component — the data returned from the endpoint is the single source of truth.

I like this scheme because it removes the SvelteKit `load` boilerplate by assuming that I want all my data to come from the server endpoint. It establishes an endpoint for every route automatically and tightly couples the server code and client code. In the app I develop there is basically no time I want a route without an associated endpoint and want the coordination between the page and endpoints to be as seamless as possible.

Full-stack Svelte also lets me make this cool form component:

```svelte
  <Form>
    <input name="name" bind:value="{$d.name}" />
    <input type="submit" value="Submit"> 
  </Form>
```

Form features:
  * Forms will be submitted with `fetch` automatically.
  * Forms will still work if the user has JavaScript disabled.
  * Form inputs are disabled automatically while waiting for a server response. No double submitting.
  * Server errors and network errors are automatically handled by default.
  * Exposes `submitting` and `serverError` so users of the component can have custom UI when those states happen.
  * Exposes a `submit` function that allows custom code to run before and after the server response.



