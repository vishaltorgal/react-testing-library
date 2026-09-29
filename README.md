# React Testing Library Interview Questions (2025–2026)

### Table of Contents
1. [render](#1-render)
2. [screen](#2-screen)
3. [expect](#3-expect)


## 1. **render**
Renders your React component for testing.

```jsx
render(<Welcome />);
```

## 2. **screen**
Used to find elements on the screen.

```jsx
screen.getByText("Hello Vishal");
```

## 3. **expect**
Checks whether something is correct.

```jsx
expect(screen.getByText("Hello Vishal")).toBeInTheDocument();
```
