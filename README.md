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
- Props are plain params; slots/children are passed as callable values instead of `props.children`.

## More Ripple syntax at a glance

- **Reactive reads/writes**: `@count` reads and updates tracked values inline (no setters or `.value`).
- **Effects**: `effect(() => {...})` auto-subscribes to any `@tracked` access inside; reruns only when those change.
- **Computed values**: use regular functions that read `@state` to derive values—Ripple tracks them automatically without `useMemo`.
- **Events**: handlers receive native events; you can destructure like `onClick={(e) => @count += e.shiftKey ? 10 : 1}`.
- **Props**: function parameters are props; destructure directly `export component Badge({ label, tone = 'info' }) { ... }`.
- **Slots/children**: pass JSX directly; no `props.children`—just reference the slot name you declare.
- **Class handling**: `class` works (no `className`); you can interpolate strings/expressions inline.
- **Lists**: map arrays with JSX; keys are `key=` attribute (no `key` prop warning ceremony).
- **No useEffect/useState/useMemo**: tracking and effects replace most React hooks; lifecycle is largely implicit through data flow.

## Components vs React

- Ripple uses `component` instead of `function` and does not require an explicit `return`; JSX is written directly after declarations.
- Props are positional/destructured parameters (no `props` object unless you want one).
- Slots/children are passed as callable values (`<FormFooter Title={FormHeader}>...`) instead of `props.children`.

## Collections: arrays and sets

Ripple tracks arrays and sets; reassign to trigger updates:

```jsx
let items = track(['Alpha', 'Beta']);
let tags = track(new Set(['Ready']));

const addItem = () => {
        @items = [...@items, `Item ${@items.length + 1}`];
};

const addTag = () => {
        @tags = new Set([...@tags, `Tag ${@tags.size + 1}`]);
};
```

Then render with familiar JSX:

```jsx
{@items.map((item, idx) => <li key={`item-${idx}`}>{item}</li>)}
{Array.from(@tags).map((tag) => <li key={tag}>{tag}</li>)}
```

## Live demo in this repo

- Counter with `track` and `effect` updates `document.title`.
- Derived value shows a memo-like function (`doubled()`) without `useMemo`.
- Collections panel shows tracked array/set updates.
- Public API fetch panel demonstrates loading/success/error state handling with tracked state.

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
