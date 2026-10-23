# allowReorder demo

Live demo for the `allowReorder` prop proposed in the `feature/allow-reorder` branch:
https://milansimek.github.io/svelte-tags-input/

The root of this branch is the built page. `playground-source/` holds its source.

To run it locally, check out `feature/allow-reorder`, copy `playground-source/` to `playground/`
in the repository root (`App.svelte` imports `../src/Tags.svelte`), then:

```bash
npm install
npx vite playground            # dev server
npx vite build playground      # static build to playground/dist
```
