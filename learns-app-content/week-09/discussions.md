## Discussion Topics

### Advanced State and useReducer

The `useReducer` hook adapts the reducer pattern for use in React applications. Just like other hooks, it must be called at the top level of the component. React attempts to batch state changes so that if several happen in succession, they are all processed during the same render cycle. React also compares the reducer's output to its previous state - if nothing changes, it does not initiate a re-render.

`useReducer` takes a reducer function and an initial state value when called. It outputs a state value and a dispatch function which are assigned similar to `useEffect`.

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

Reducer functions and the initial state values tend to be more complex than the arguments than a `useState` hook. The reducer function also works independently from any component: it reads no values from inside the component directly. Because of these factors we will place these in a separate file to keep the component's size to a minimum.

For this discussion, we will implement the reducer pattern on cart's state. We create a file `cart.reducer.js` and place it into a `/reducers` folder created under `/src`. It's helpful to drop the "x" from the filename's extension since it will not contain any React-specific code or JSX. This will tell us, at a glance that it is not component code without having to open the file up. After creating the new file, we when need to identify the relevant `useState`s:

![ide screenshot highlighting cart state](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/cart-state-hilighted.png)

The `useState`s highlighted above all manage some aspect of state related to the cart. From this list, we can determine `initialState`:

```js
// extract from cart.reducer.js
const initialState = {
  cart: [],
  isCartOpen: false,
  isCartSyncing: false,
  //for the sake of simplicity here, we combine cartError and cartItemError
  error: '',
};
//code continues...
```

We then create a reducer that returns the state for now.

```js
// extract from cart.reducer.js
//...code
function reducer(state, action) {
  switch (action.type) {
    default:
      return state;
  }
}

export { initialState, reducer };
```

We will add `case` statements for each of the actions we identify to organize the state logic. `switch/case` flow control is easier to read than `if/else` statements so is usually preferred. Reducer functions tend to grow quite long.

We then import the initial value and the reducer into `App.js` and add them to a `useReducer` hook:

```jsx
{
  /*extract from App.jsx*/
}
{
  /*...code*/
}
import {
  //aliasing with `as` keeps the reducer and state easily identifiable
  initialState as cartInitialState,
  reducer as cartReducer,
} from './reducers/App/cart.reducer';

{
  /*...unrelated code...*/
}

const [cartState, dispatch] = useReducer(cartReducer, cartInitialState);
{
  /*code continues...*/
}
```

Next, we'll find places where `isCartOpen` is used and determine how argument is created. In some cases, it's a direct value that is passed in. In other cases, our event handlers and helper functions calculate that value. Using VS Code's file search we can find 3 instances to `setIsCardOpen`.

![ide file search result numbers for setIsCartOpen](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/is-cart-open-search.png)

The first location is in `useState` so we do not need to worry about that for now. The second one is in a handler function that closes the cart and the final on is in the Header instance:

```jsx
{/*extract from App.jsx*/}
{/*...code*/}
 function handleCloseCart() {
  {/*used here*/}
     setIsCartOpen(false);
     setAuthError('');
 }
/*...unrelated code...*/
 <Header
  cart={cart}
  {/*used here*/}
  handleOpenCart={() => setIsCartOpen(true)}
  handleOpenAuthDialog={handleOpenAuthDialog}
  handleLogOut={handleLogOut}
  user={user}
    />
{/*code continues...*/}
```

They **close** and **open** the cart respectively. These actions will be the basis of the action objects the we eventually write. Both locations set the boolean value directly so we don't have any state logic to move over to the reducer other than updating those values.

We add a `case` statement for each of these actions to the reducer. From each we return a new object that spreads the original `state` and a new value for the `isCartOpen` property.

```js
// extract from cart.reducer.js
function cartReducer(state, action) {
  switch (action.type) {
    case "open":
      return {
        ...state,
        isCartOpen: true,
      };
    case "close":
      return {
        ...state,
        isCartOpen: false,
      };
    default:
   return state;
//...code
//code continues...
```

Before removing any state update function, we can place the dispatch along side it if we need to do any troubleshooting. We can the log the reducer's output to ensure the action's `case` block functions as expected.

Recall that the dispatch function takes an `action`. This can be of any type (string, object, number, etc.) but by convention, we stick with objects containing a `type` property to identify the action and add properties as needed. The actions that our reducer will receive for the cart actions resemble:

```javascript
const open = { value: 'open' };
const close = { value: 'close' };
```

When we are happy with how the `case` blocks work in the reducer, we can then update all `isCartOpen` references with `cartState.isCartOpen` to migrate to the updated state.

We then update each `isCartSyncing` with `cartState.isCartSyncing` to use the state our `useReducer` returns.

```jsx
{
  /*extract from */
}
{
  /*...code*/
}
{
  cartState.isCartOpen && (
    <Cart
      cartError={cartError}
      isCartSyncing={isCartSyncing}
      cart={cart}
      handleSyncCart={handleSyncCart}
      handleCloseCart={handleCloseCart}
    />
  );
}
{
  /*code continues...*/
}
```

After completing these updates, our cart behaves the same but that state is now fully managed by the reducer.

![animated screen capture highlighting isCartOpen as UI is manipulated](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-11/is-cart-open-state.gif)

We are now left with 5 more `useStates` to refactor.

```javascript
const [cart, setCart] = useState([]);
const [isCartOpen, setIsCartOpen] = useState(false); //DONE
const [isCartSyncing, setIsCartSyncing] = useState(false);
const [cartError, setCartError] = useState('');
const [cartItemError, setCartItemError] = useState('');
```

Besides `cart`, each other state value update employs the same complexity as `isCartOpen`. We'll refactor these without discussion so we can then explore how we identify actions and refactor the state updating logic over to our reducer.

Refactoring `cart` is the most complex because of the differing ways that the user interacts with the contents of their cart. I`setCart` is referenced in 7 places in `App.jsx` excluding the `useEffect`

- 1 time in `handleAuthenticate`
  - when a user logs in and has a saved cart, `setCart` receives `userData.cartItems` from the API response
- 2 times in `handleSyncCart`
  - when a user is not logged in and they confirm a cart change, `setCart` receives `workingCart`
  - when a user is logged in and they confirm a cart change, `setCart` receives the `cartData` from the API
- 3 times in `handleAddItemToCart` (this is the most complex set of uses since we used an optimistic approach to managing `cart` state)
  - when a user adds an item the event handler:
    - it finds if there's a matching item in the cart.
      - if yes, it creates a duplicate cart item then increments the matching item quantity
      - if no, it creates a new cart item with a quantity of 1
    - it then calls `setState` with an array which includes the newest item
    - if an API response fails, the cart item is reverted to:
      - previous value if it has a quantity greater than one or
      - removes it if its quantity is 1
- 1 time in `handleLogOut`
  - `setCart` receives an empty array to empty the cart

We can distill this list into two actions: **"add item"** and **"update cart"**

- A user logs in and they have a saved cart that is loaded: **"update cart"**
- A user modifies item counts in the cart: **"update cart"**
- A user adds an item to their cart: **"add item"**
- A user logs out which empties the cart: **"update cart"**

"add item" ends up being the more complex of the two because of the data manipulation required to make cart items out of products in the product list. We will take care of "update cart" first since it operates on the whole cart and, as a result, has less supporting logic.

In each occasion where we plan on using the "update cart" action, the reducer is going to receive an action object with a cart value that replaces the existing cart. In each of the areas where we use this action, we are going to pass an updated cart received from the API or when a user confirms a cart edit.

We'll update the cart reducer to include a `case` for "update cart" and add in the logic that returns the new cart value we will place into the action object.

```js
// extract from cart.reducer.js
//...code
function CartReducer(state, action){
 switch(action.type) {
  case "open":
      return {
          ...state,
          isCartOpen: true,
      };
  }
//code continues...
  case "update"

}
```

We can then go back to the codebase and replace the `setCart`s where we intend on dispatching the "update cart" action. You can also see the other dispatches that have already been placed in `handleSyncCart` and `handleAuthenticate` to deal with other aspects of cart state.

```jsx
//extract from App.jsx
//...code
const handleSyncCart = useCallback(
  async (workingCart) => {
    if (!user.id) {
      //dispatch replaces setCart here
      dispatch({ type: 'update cart', cart: workingCart });
      return;
    }
    dispatch({ type: 'sync' });
    const options = {
      method: 'PATCH',
      body: JSON.stringify({ cartItems: workingCart }),
      headers: {
        'Content-Type': 'application/json',
        Authorization: `Bearer ${user.token}`,
      },
    };
    try {
      const resp = await fetch(`${baseUrl}/cart`, options);
      if (!resp.ok) {
        console.log('resp not okay');
        if (resp.status === 401) {
          throw new Error('Not authorized. Please log in.');
        }
      }
      const cartData = await resp.json();
      if (cartData.error) {
        throw new Error(cartData.error);
      }
      //dispatch replaces setCart here
      dispatch({ type: 'update cart', cart: cartData });
    } catch (error) {
      console.error(error);
      dispatch({ type: 'error', error: error.message });
    } finally {
      dispatch({ type: 'not syncing' });
    }
  },
  [user.id, user.token, dispatch],
);
//code continues...
async function handleAuthenticate(credentials) {
  const options = {
    method: 'POST',
    body: JSON.stringify(credentials),
    headers: { 'Content-Type': 'application/json' },
  };
  try {
    setIsAuthenticating(true);
    const resp = await fetch(`${baseUrl}/auth/login`, options);
    if (!resp.ok) {
      if (resp.status === 401) {
        setAuthError('email or password incorrect');
      }
      throw new Error(resp.status);
    }
    const userData = await resp.json();
    setUser({ ...userData.user, token: userData.token });
    dispatch({
      type: 'update cart',
      cart: userData.cartItems,
    });
    setAuthError('');
    setIsAuthenticating(false);
    setIsAuthDialogOpen(false);
  } catch (error) {
    setIsAuthenticating(false);
    console.log(error.message);
  }
}
//code continues...
```

We then have to determine the logic for the "add item" action. The first challenge is that the logic filters through the inventory list to ensure the item exists before making a new cart item or incrementing the quantity on an existing cart item. Since our reducer exists independently from the App component, we need to include the inventory on the action object.

With this in mind, we can move over the logic and update any `inventory` references to `action.inventory` and create a return value that reflects the updated state:

```js
// extract from cart.reducer.js
//...code
 case cartActions.addItem: {
      const inventoryItem = action.inventory.find(
        (item) => item.id === action.id
      );
      if (!inventoryItem) {
        return state;
      }
      const itemToUpdate = state.cart.find(
        (item) => item.productId === action.id
      );
      let updatedCartItem;
      if (itemToUpdate) {
        updatedCartItem = {
          ...itemToUpdate,
          quantity: itemToUpdate.quantity + 1,
        };
      } else {
        updatedCartItem = {
          ...inventoryItem,
          quantity: 1,
          productId: inventoryItem.id,
        };
      }
      return {
        ...state,
        cart: [
          ...state.cart.filter((item) => item.productId !== action.id),
          updatedCartItem,
        ],
      };
    }
//code continues...
```

Almost done! Now we have to back to the App component and change all `cart` references to `cartState.cart`. These are found as props given to the Header component and the Cart component:

```jsx
{
  /*extract from App.jsx*/
}
{
  /*...code*/
}
<Header
  cart={cartState.cart}
  handleOpenCart={() => dispatch({ type: 'open' })}
  handleOpenAuthDialog={handleOpenAuthDialog}
  handleLogOut={handleLogOut}
  user={user}
/>;
{
  /*...code...*/
}
{
  cartState.isCartOpen && (
    <Cart
      cartError={cartState.error}
      isCartSyncing={cartState.isCartSyncing}
      cart={cartState.cart}
      handleSyncCart={handleSyncCart}
      handleCloseCart={handleCloseCart}
    />
  );
}
{
  /*code continues...*/
}
```

After these changes, our cart behaves as it did previously, but all of its state is now managed by the reducer. Because the reducer, dispatch function and the action are so tightly coupled, the reducer function is probably one of the most complex things we have covered so far. While it is harder to employ, it is far easier to manage complex state this way than relying on numerous `useState`s and we also end up with a much more compact function.

### Sharing State Across Components with useContext

As your React applications grow, you'll often find that multiple components need access to the same data — user settings, themes, or authentication state. Passing that data through props down every level of the component tree becomes repetitive and messy.

This is known as prop drilling, and React's `useContext` hook provides a clean solution.

#### The Problem: Prop Drilling

Imagine you want to toggle between light and dark themes in your app. The theme setting lives in your top-level component (`App`), but a nested `Header` component needs to display a button to switch modes.

Without context, you'd have to pass the `theme` and `setTheme` props down through multiple intermediate components — even ones that don't care about the theme at all. This clutters your code and makes components less reusable.

#### Context to the Rescue

React's Context API allows you to share data across components without manually passing props. You define a "context" that can be accessed anywhere within its provider, letting components consume shared data directly.

#### Example Dark/Light Mode Application

##### Step 1: Create the Context

First, create a new context object using `createContext()`:

```js
// ThemeContext.js
import { createContext } from 'react';

export const ThemeContext = createContext(null);
```

The argument passed to `createContext()` is the default value — used only when a component tries to access context outside of a provider.

##### Step 2: Provide Context to Your Component Tree

Wrap the portion of your app that needs access to the context with a `<ThemeContext.Provider>`.

The Provider's `value` prop defines what data or functions will be shared.

```jsx
// App.jsx
import { useState } from 'react';
import { ThemeContext } from './ThemeContext';
import Header from './Header';
import Main from './Main';

function App() {
  const [theme, setTheme] = useState('light');
  const toggleTheme = () =>
    setTheme((prev) => (prev === 'light' ? 'dark' : 'light'));

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      <main
        className={
          theme === 'light' ? 'bg-white text-black' : 'bg-black text-white'
        }>
        <Header />
        <Main />
      </main>
    </ThemeContext.Provider>
  );
}

export default App;
```

Here, both the current theme and a toggle function are made available to all descendants.

##### Step 3: Consume Context in Child Components

Any component nested within the provider can use `useContext` to access the shared data — no props required.

```jsx
// Header.jsx
import { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <header className="p-4 flex justify-between items-center">
      <h1>My Themed App</h1>
      <button onClick={toggleTheme} className="border rounded px-3 py-1">
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </header>
  );
}

export default Header;
```

The `Header` doesn't need to receive the theme via props — it simply reads from the context.

##### Step 4: Apply Context in Other Components

You can use the same context anywhere else in your app:

```jsx
// Main.jsx
import { useContext } from 'react';
import { ThemeContext } from './ThemeContext';

function Main() {
  const { theme } = useContext(ThemeContext);

  return (
    <section className="p-4">
      <p>
        The current theme is <strong>{theme}</strong>.
      </p>
    </section>
  );
}

export default Main;
```

Both `Header` and `Main` now stay in sync with the same global theme, eliminating prop drilling.

#### Performance Pitfalls

While context simplifies state sharing, it can also introduce performance issues if used improperly.

Every time a context's value changes, all components consuming that context re-render — even if they don't use the changed part of the value.

To avoid performance bottlenecks:

**Split large contexts** — Don't group unrelated data (like theme, auth, and notifications) in one context.

**Memoize the provider's value** — Wrap it in `useMemo` to avoid triggering re-renders unnecessarily.

```jsx
const value = useMemo(() => ({ theme, toggleTheme }), [theme]);
<ThemeContext.Provider value={value}>...</ThemeContext.Provider>;
```

**Use context for stable, infrequently changing data.** Rapidly updating state (like scroll position or animations) should remain local.

**Don't overuse context.** If data is only shared between a few components, regular prop passing is usually simpler and faster.

#### Summary

`useContext` is a powerful hook that lets you share state cleanly across your component tree without prop drilling.

In the light/dark theme example, both `Header` and `Main` access and update the same theme value directly — improving readability and maintainability.

However, use context thoughtfully. Overusing or mismanaging context values can lead to unnecessary re-renders and performance slowdowns. Keep your contexts focused, memoized, and minimal for best results.
