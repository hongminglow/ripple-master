# Ripple Playground for React Devs

> ⚠️ **Not production ready. Expect breaking changes at any time.** Use this repo purely for learning and experimenting.

- Purpose: a tutorial playground to help React developers get comfortable with Ripple syntax.
- Current Ripple version: `0.2.184` (npm tag `latest`).
- Stack: Ripple + TypeScript + Vite + Tailwind.

## React vs Ripple (state and effect)

React familiar pattern:

```tsx
import { useEffect, useState } from 'react';

export function Counter() {
	const [count, setCount] = useState(0);

	useEffect(() => {
		document.title = `Count: ${count}`;
	}, [count]);

	return <button onClick={() => setCount((c) => c + 1)}>Count: {count}</button>;
}
```

Ripple equivalent:

```jsx
import { effect, track } from 'ripple';

export component Counter() {
    let count = track(0);

    effect(() => {
        document.title = `Count: ${@count}`;
    });

    <button onClick={() => @count++}>{'Count: '}{@count}</button>
}
```

Key differences:

- `track` creates reactive state; use `@state` to read/update it without setters.
- `effect` runs whenever tracked values inside it change (no dependency arrays).
- Components are declared with `component` and render directly—no `return` wrapper needed.

## Getting Started

1.  Install dependencies:

        ```bash
        npm install # or pnpm or yarn
        ```

2.  Start the development server:

        ```bash
        npm run dev
        ```

3.  Build the demo bundle (still experimental):

        ```bash
        npm run build
        ```

## Project layout

- `src/App.ripple` – main tutorial component using Ripple state and events.
- `src/index.ts` – mounts the Ripple app to `#root`.
- `src/assets/` – static assets (logo, etc.).
- `tailwind.config.ts` – Tailwind v4 config consumed by Vite.

## Code Formatting

Prettier is configured with the Ripple plugin for `.ripple` files.

Available commands:

- `npm run format` – format all files.
- `npm run format:check` – check formatting without writing.

VS Code extensions recommended:

- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Ripple](https://marketplace.visualstudio.com/items?itemName=ripple-ts.vscode-plugin)

## More resources

- [Ripple Docs](https://www.ripplejs.com/docs)
- [Ripple Playground](https://www.ripplejs.com/playground)
- [Vite Docs](https://vitejs.dev/)
