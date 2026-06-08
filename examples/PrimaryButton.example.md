# Example — PrimaryButton (workflow output)

This is an illustrative example of what the workflow produces: a single component
built to the `component-builder` contract, using only CLAUDE.md tokens, with every
interactive state present. It shows the *output* of the methodology, not new rules.

## Spec applied (from CLAUDE.md)
- Variant `brand` -> teal `#00B8B2` fill, near-black text (marketing CTAs)
- Variant `app` -> white `#FFFFFF` fill, near-black text (in-app / auth)
- Radius `--r-md` (12px); icon+label gap 8px
- Hover darkens; focus-visible = 2px teal ring at 2px offset; active scale 0.99
- Disabled = 40% opacity + not-allowed + aria-disabled; loading = spinner + aria-busy
- One primary per screen

## Reference implementation

```tsx
interface PrimaryButtonProps {
  label: string;
  onClick: () => void;
  variant?: "brand" | "app";
  loading?: boolean;
  disabled?: boolean;
}

export function PrimaryButton({
  label,
  onClick,
  variant = "brand",
  loading = false,
  disabled = false,
}: PrimaryButtonProps) {
  const base =
    "inline-flex items-center justify-center gap-2 rounded-[12px] px-5 py-2.5 " +
    "text-[16px] font-semibold transition-[background-color,opacity,transform] " +
    "duration-150 ease-out focus-visible:outline-none focus-visible:ring-2 " +
    "focus-visible:ring-[#00B8B2] focus-visible:ring-offset-2 " +
    "focus-visible:ring-offset-[#0D0D0D] active:scale-[0.99] " +
    "disabled:opacity-40 disabled:cursor-not-allowed";

  const variants = {
    brand: "bg-[#00B8B2] text-[#0D0D0D] hover:brightness-90",
    app: "bg-white text-[#0D0D0D] hover:bg-neutral-200",
  };

  return (
    <button
      type="button"
      onClick={onClick}
      disabled={disabled || loading}
      aria-busy={loading}
      aria-disabled={disabled || loading}
      className={`${base} ${variants[variant]}`}
    >
      {loading ? <Spinner /> : label}
    </button>
  );
}
```

## Gate result (design-review)
```
DESIGN REVIEW — PrimaryButton
1. Tokens & values ......... PASS
2. Color & platform ........ PASS
3. Primary action .......... PASS
4. Interactive states ...... PASS
5. Responsiveness .......... PASS  (44px min touch target, fluid)
6. Domain rules ............ N/A   (no product data)
7. Component & a11y ........ PASS  (typed props, semantic button, keyboard-operable)
8. Commit readiness ........ PASS

RESULT: PASS — cleared.
```
