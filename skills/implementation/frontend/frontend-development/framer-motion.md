# Skill: Framer Motion Animation

## Context
You are a **UI Interaction Designer** using React. You use Framer Motion not just for "fading in" elements, but for complex **Layout Animations**, **Gestures**, and **Orchestration**.

## Core Principles
1.  **Declarative Animation:** Describe *what* the state should be (`animate`), not *how* to get there.
2.  **Layout Projection:** The magic of Framer Motion is `layout`. It morphs elements between DOM positions seamlessly.
3.  **Exit Animations:** `AnimatePresence` is required to animate components *leave* the DOM.

## Technical Rules & Patterns

### 1. Layout Animations
**Rule:** Add the `layout` prop to automatically animate changes in size or position caused by re-renders (e.g., list reordering, expanding cards).
```jsx
<motion.div layout class="card">
  {isOpen && <Content />}
</motion.div>
```

### 2. AnimatePresence (Exit)
**Rule:** Direct children of `AnimatePresence` must have a unique `key`.
```jsx
<AnimatePresence mode="wait">
  {isVisible && (
    <motion.div
      key="modal"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      exit={{ opacity: 0 }}
    />
  )}
</AnimatePresence>
```

### 3. Variants (Orchestration)
**Rule:** Use `variants` to propagate animations from parent to children (`staggerChildren`).
```jsx
const list = {
  hidden: { opacity: 0 },
  visible: { 
    opacity: 1, 
    transition: { staggerChildren: 0.1 } 
  }
};
const item = { hidden: { x: -10, opacity: 0 }, visible: { x: 0, opacity: 1 } };

<motion.ul variants={list} initial="hidden" animate="visible">
  <motion.li variants={item} />
  <motion.li variants={item} />
</motion.ul>
```

### 4. Performance (Will Change)
**Rule:** For heavy animations, use the `will-change` CSS property or `transformTemplate` if needed, but Framer Motion handles hardware acceleration well by default (animating transform/opacity). avoid animating layout properties (width/height) unless using the `layout` prop (which uses scale transforms under the hood).

## Best Practices Checklist
- [ ] **Lazy Motion:** Reduce bundle size by using `LazyMotion` and loading specific features (`domAnimation` vs `m` component) if you don't need the full engine.
- [ ] **Accessibility:** Respect `prefers-reduced-motion`.
```jsx
const shouldReduce = useReducedMotion();
const animate = shouldReduce ? { opacity: 1 } : { x: 100, opacity: 1 };
```
- [ ] **Drag:** Use `drag`, `dragConstraints`, and `dragElastic` for natural physics-based gestures.
