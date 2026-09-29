# React Testing Library Interview Questions (2025–2026)

### Table of Contents
1. [render](#1-render)
2. [screen](#2-screen)
3. [expect](#3-expect)
4. [getByRole](#4-getbyrole)
5. [getByPlaceholderText](#5-getbyplaceholdertext)
6. [getByLabelText](#6-getbylabeltext)
7. [getByText](#7-getbytext)
8. [getByTestId](#8-getbytestid)
9. [Button Click](#9-button-click)
10. [getBy vs queryBy vs findBy](#10-getby-vs-queryby-vs-findby)


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

## 4. **getByRole**

### Example Button:
```jsx
function Login() {
  return (
    <button>Login</button>
  );
}
```

### Test Button:
```jsx
test("login button is displayed", () => {
  render(<Login />);

  expect(
    screen.getByRole("button", { name: "Login" })
  ).toBeInTheDocument();
});
```

### Example Checkbox:
```jsx
// Component
<label>
  <input type="checkbox" /> Accept terms and conditions
</label>

// Test
const checkbox = screen.getByRole('checkbox', { name: /accept terms/i });
expect(checkbox).not.toBeChecked();
```

### Example Link:
```jsx
// Component
<a href="/dashboard">Go to Dashboard</a>

// Test
const link = screen.getByRole('link', { name: /go to dashboard/i });
expect(link).toHaveAttribute('href', '/dashboard');
```

### Example Dropdown:
```jsx
// Component
<div>
  <label htmlFor="country">Country</label>
  <select id="country">
    <option value="us">United States</option>
    <option value="ca">Canada</option>
  </select>
</div>

// Test
// Note: <select> maps to the 'combobox' role in accessibility trees
const select = screen.getByRole('combobox', { name: /country/i });
expect(select).toBeInTheDocument();
```

### Example Alert / Status Message
```jsx
// Component
<div role="alert">Error: Invalid credentials</div>

// Test
const alertMessage = screen.getByRole('alert', { name: /error/i });
expect(alertMessage).toHaveTextContent(/invalid credentials/i);
```

## 5. **getByPlaceholderText**
You can also find an input using its placeholder.
```jsx
// Component
export default function SearchBar() {
  return <input type="text" placeholder="Search products..." />;
}

// Test
import { render, screen } from '@testing-library/react';
import SearchBar from './SearchBar';

test('renders input with placeholder', () => {
  render(<SearchBar />);

  // Find the input using its placeholder text (case-insensitive regex)
  const searchInput = screen.getByPlaceholderText(/search products/i);
  
  expect(searchInput).toBeInTheDocument();
});
```

## 6. **getByLabelText**

```jsx
// Component
export default function SignupForm() {
  return (
    <div>
      <label htmlFor="username">Username</label>
      <input id="username" type="text" />
    </div>
  );
}

// Test
import { render, screen } from '@testing-library/react';
import SignupForm from './SignupForm';

test('renders input associated with its label', () => {
  render(<SignupForm />);

  // Find the input using the text of its associated label
  const usernameInput = screen.getByLabelText(/username/i);
  
  expect(usernameInput).toBeInTheDocument();
});
```

## 7. **getByText**

```jsx
// Component
export default function UserCard({ name, role }) {
  return (
    <div className="card">
      <h3>{name}</h3>
      <p>Role: {role}</p>
      <span>Status: Active</span>
    </div>
  );
}

// Test
import { render, screen } from '@testing-library/react';
import UserCard from './UserCard';

test('renders user profile details correctly', () => {
  render(<UserCard name="Jane Doe" role="Frontend Developer" />);

  // Find elements using their text content (case-insensitive regex is recommended)
  const nameElement = screen.getByText(/jane doe/i);
  const roleElement = screen.getByText(/frontend developer/i);
  const statusElement = screen.getByText(/status: active/i);

  expect(nameElement).toBeInTheDocument();
  expect(roleElement).toBeInTheDocument();
  expect(statusElement).toBeInTheDocument();
});
```

## 8. **getByTestId**

```jsx
// Component
export default function CustomLoader() {
  return (
    <div data-testid="loading-spinner" className="spinner-container">
      <span>Loading...</span>
    </div>
  );
}

// Test
import { render, screen } from '@testing-library/react';
import CustomLoader from './CustomLoader';

test('renders the spinner using a test id', () => {
  render(<CustomLoader />);

  // Find the element using the data-testid value
  const spinner = screen.getByTestId('loading-spinner');
  
  expect(spinner).toBeInTheDocument();
});
```

## 9. **Button Click**

### Example:
```jsx
function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

### Test:
```jsx
import { render, screen } from "@testing-library/react";
import userEvent from "@testing-library/user-event";
import { test, expect } from "vitest";
import Counter from "./Counter";

test("increments count", async () => {
  const user = userEvent.setup();

  render(<Counter />);

  const button = screen.getByRole("button", {
    name: "Increment",
  });

  await user.click(button);

  expect(screen.getByText("1")).toBeInTheDocument();
});
```
## 10. **getBy vs queryBy vs findBy**

### getBy
Use when the element should already exist.

```jsx
screen.getByText("Product List");
```
If it doesn't exist → test fails immediately.

### queryBy
Use when you want to check that something does NOT exist.

```jsx
expect(
  screen.queryByText("Error")
).not.toBeInTheDocument();
```
queryBy returns null if nothing is found.

### findBy
Use when something appears asynchronously.

```jsx
const product = await screen.findByText("iPhone 15");
```

findBy waits for the element to appear.


### Example:
```jsx
import { useState, useEffect } from 'react';

export default function DataFetcher() {
  const [data, setData] = useState(null);

  useEffect(() => {
    const timer = setTimeout(() => {
      setData('Loaded Data!');
    }, 100);
    return () => clearTimeout(timer);
  }, []);

  return (
    <div>
      {!data ? <p>Loading...</p> : <h1>{data}</h1>}
    </div>
  );
}
```
### Test:
```jsx
import { render, screen } from '@testing-library/react';
import DataFetcher from './DataFetcher';

test('demonstrates getBy, queryBy, and findBy', async () => {
  render(<DataFetcher />);

  // 1. getBy: Expect the loading text to be there IMMEDIATELY
  expect(screen.getByText(/loading.../i)).toBeInTheDocument();

  // 2. queryBy: Expect the loaded data NOT to be there yet (returns null, doesn't crash)
  expect(screen.queryByText(/loaded data!/i)).not.toBeInTheDocument();

  // 3. findBy: Wait ASYNCHRONOUSLY for the data to appear
  const loadedData = await screen.findByText(/loaded data!/i);
  expect(loadedData).toBeInTheDocument();

  // Now that data is loaded, "Loading..." should be gone
  expect(screen.queryByText(/loading.../i)).not.toBeInTheDocument();
});
```
