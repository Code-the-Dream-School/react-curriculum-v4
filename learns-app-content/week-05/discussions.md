## Discussion Topics

### Conditional Rendering

When we left off with CTD Swag, we had a shopping cart icon and a counter on the upper-right portion of the screen. This gave the user a count of their cart contents but left them without the means to see everything they've selected. A detailed shopping cart takes up a lot of screen real estate and gets in the way of the shopping experience. Components like these are commonly developed so they can be toggled open or closed by some user action.

Conditional rendering allows us to develop an interface that adapts to different states, user interactions, or data inputs. Developers can employ different techniques to hide and show elements, change styles, and update visual content to create interactive and personalized user interfaces. Before we create a shopping cart that we can show/hide, let's look some common techniques we can employ.

#### Ternary Operator

`condition? expressionIfTrue : expressionIfFalse`.

The ternary operator is a useful tool that evaluates a condition and then executes one of two possible expressions. If true, the expression on the left executes. False causes the right-hand expression to execute. These expressions can be written in JSX which results in compact code.

```jsx
//ternary operator that shows 1 of 2 components
const [isTruthy, setIsTruthy] = useState(true);
return(
 <>
  {isTruthy ? <TrueComponent /> : <FalseComponent /> }
  <button type="button" onClick=(()=> setIstTruthy(!isTruthy))>Toggle Value</button>
 </>
);
```

React allows us to use `null` in place of an expression so we can show or hide an element.

```jsx
//ternary operator to conditionally show a component
const [isRendered, setIsRendered] = useState(true);
return(
 <>
  {isTruthy ? <SpecialComponent /> : null }
  <button type="button" onClick=(()=> setIsRendered(!isRendered))>Show/Hide Element</button>
 </>
);
```

#### Logical && Operator

We can take advantage of the way JavaScripts logical `&&` operator works to show or hide an element. Remember that JavaScript evaluates the left side (operand) first before continuing onto the right hand side. When the first operand is truthy, the second operand is evaluated - that is where we return our element. If that first operand is false, JavaScript stops the entire evaluation which prevents the item from being rendered.

```jsx
//logical && operator to conditionally show a component
const [isRendered, setIsRendered] = useState(true);
return(
 <>
  {isTruthy && <SpecialComponent /> }
  <button type="button" onClick=(()=> setIsRendered(!isRendered))>Show/Hide Element</button>
 </>
);
```

#### Display Logic in a Function

In cases where we have more than two options or our rendering logic is complex, we can define functions inside the component and use their return values. What's nice about this approach is that we can return JSX. We'll talk about some performance implications in [[Code The Dream/Intro to React V3/Curriculum/Week-09|Week-09]] that may arise from functions inside components but we will not be writing anything that will cause any performance problems during this course.

In the example below, the interface renders a button that calls `cycleLight`. Each time it does, it cycles through traffic light colors (green->yellow->red->green->…and so on). Each time `trafficLightColor` is updated, the component is re-rendered, calling `renderLight`. `renderLight` uses a switch-case statement to determine the correct component to render.

```jsx
const [trafficLightColor, setTrafficLightColor] = useState('green');

function cycleLight() {
  switch (trafficLightColor) {
    case 'green':
      setTrafficLightColor('yellow');
      return;
    case 'yellow':
      setTrafficLightColor('red');
      return;
    default:
      setTrafficLightColor('green');
      return;
  }
}

function renderLight() {
  switch (trafficLightColor) {
    case 'green':
      return <YellowLight />;
    case 'yellow':
      return <RedLight />;
    default:
      return <GreenLight />;
  }
}

return (
  <>
    {renderLight()}
    <button type="button" onClick={cycleLight}>
      Cycle Light
    </button>
  </>
);
```

#### Putting Conditional Rendering into Action

> [!tip]
> When we need to see dynamic elements in our layout but are not ready to code them, we can add placeholders in the JSX. These can be plain text substitutions for values like a cart's total price or placeholder images similar to the one found on each product's card. This helps us determine our layout and anticipate any tasks that we are going to have to complete. As we continue to build the feature we can replace these substitutions with live code.

##### Combining `baseItems`

Now that we know how to show and hide components, we can start making improvements to the store. The first task is to update cards to let users choose from product variations. This will allow us to combine items of different colors or models into a single card that the user opens to select which variant they want to add to their cart.

To achieve this, we have to change the way ProductList handles inventory items. Rather than mapping them to a `ProductCard` component, we make state variable, `products`, that is updated when `inventory` props changes. Inside this `useEffect` we create an empty array containing `workingProducts` and then iterate over the inventory to compose the each product in `workingProducts` before using that array to set the `products` variable.

`forEach` basically looks at each item's `baseName` property to see if it matches any object already in `workingProducts`. If a match is found, that item is pushed into a `variants` array. If there is no match, a new object is composed for a product and then pushed into `workingProducts`.

```jsx
//extract from ProductList.jsx
//...component code
const [products, setProducts] = useState([]);

useEffect(() => {
  const workingProducts = [];
  inventory.forEach((item) => {
    if (!item.inStock) {
      return;
    }
    if (
      !workingProducts.find(
        (productItem) => productItem.baseName === item.baseName,
      )
    ) {
      workingProducts.push({
        baseName: item.baseName,
        price: item.price,
        baseDescription: item.baseDescription,
        variants: [{ ...item }],
      });
    } else {
      const index = workingProducts.findIndex(
        (productItem) => productItem.baseName === item.baseName,
      );
      workingProducts[index].variants.push({ ...item });
    }
  });
  setProducts([...workingProducts]);
}, [inventory]);
//component code...
```

Our next step is to make changes to the `ProductCard` component so it can conditionally show a "Show Options" button instead of "Add to Cart" when a card contains more than one product variant. We can do so by examining the `variants` length. Our handleAddItemToCart is also updated to grab the id off of the default variant when only one exists.

```jsx
//extract from ProductCard.jsx
<div className="productButtons">
  {product.variants.length > 1 ? (
    <button>Show Options</button>
  ) : (
    <button onClick={() => handleAddItemToCart(product.variants[0].id)}>
      Add to Cart
    </button>
  )}
</div>
```

Finally, we need to display the variants to the user so they can add them to their cart. For this, we create another component, `ProductCardVariants` that accepts an array of variants, a handler function to close the display, and a handler function to add an item to the cart.

```jsx
//ProductCardVariants.jsx

function ProductCardVariants({ variants, closeVariants, handleAddItemToCart }) {
  return (
    <div className="productVariantsWrapper">
      <ul>
        {variants.map((variant) => {
          return (
            <li key={variant.id} className="productVariant">
              <div className="variantPreview">
                <img
                  src={`/public/product-images/${variant.image}`}
                  alt={variant.variantDescription}
                />
                <p>${variant.price}</p>
              </div>
              <div className="variantDetails">
                <h3>{variant.variantName}</h3>
                <p>{variant.variantDescription}</p>
                <button
                  onClick={() => {
                    handleAddItemToCart(variant.id);
                    closeVariants();
                  }}>
                  Add to cart
                </button>
              </div>
            </li>
          );
        })}
      </ul>
      <button
        className="variantsCloseButton"
        type="button"
        onClick={closeVariants}>
        Close
      </button>
    </div>
  );
}

export default ProductCardVariants;
```

That component is imported into ProductCard, and its visibility will depend on a state variable `areVariantsShown` the accompanying state variable updater function is then used to show/hide the options.

```jsx
//ProductCard.jsx extract

return (
  <li className="productCard">
    <div className="productPreview">
      <img src={placeholder} alt=" " />
    </div>
    <div className="productCopy">
      <h2>{product.baseName}</h2>
      <p>{product.baseDescription}</p>
    </div>
    <div className="productButtons">
      {product.variants.length > 1 ? (
        <button onClick={() => setAreVariantsShown(true)}>Show Options</button>
      ) : (
        <button onClick={() => handleAddItemToCart(product.variants[0].id)}>
          Add to Cart
        </button>
      )}
    </div>
    {areVariantsShown && (
      <ProductCardVariants
        handleAddItemToCart={handleAddItemToCart}
        variants={product.variants}
        closeVariants={() => setAreVariantsShown(false)}
      />
    )}
  </li>
);
```

Here is the resulting change:

![show bucket hat options](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-05/show-options.gif)

##### Create Cart Component

Looking back at CTD swag, we can now determine which approaches to use that best suites our needs at each step of building out the feature.

To get started, we create a cart component which contains the contents of the shopping cart, a cart total, and a button to close the cart. We need to include props for the cart and the handler function that handles closing the cart. The inside an unordered list, items are mapped to `li.cartListItem` similar to how the products were mapped to a list of cards in the store. We can then include a cart total and a button to close the cart. We pass an `onClick` props to the button with the handler function.

```jsx
//Cart.jsx

import placeholder from './assets/placeholder.png';

// `handleCloseCart` is not made yet but we know we will need it
function Cart({ cart, handleCloseCart }) {
  return (
    <>
      <div className="cartScreen"></div>
      {/*
   .cartScreen covers the product list with
   a div that has a blur effect placed on it.
   this makes the product buttons unclickable
  */}
      <div className="cartListWrapper">
        <ul className="cartList">
          {cart.map((item) => {
            return (
              <li className="cartListItem" key={item.cartItemId}>
                <img src={placeholder} alt="" />
                <h2>{item.baseName}</h2>
                <div className="cartListItemSubtotal">
                  <p>${item.price}</p>
                </div>
              </li>
            );
          })}
        </ul>
        {/* cart total will need to be calculated */}
        <h2>Cart Total: $0.00</h2>
        <button onClick={handleCloseCart}>CloseCart</button>
      </div>
    </>
  );
}

export default Cart;
```

#### Track Cart's Display Status

We next move to the `App` component to add the `Cart` component to the UI. The logic to show and hide the cart is simple so we do not need to extract a function to perform our conditional rendering. We are left with ternary operator or the logical `&&`. Both choices are valid but implementing the feature with the logic `&&` operator will give us with the least amount of code. Let's insert the toggle-able cart underneath the `ProductList` component.

We need a boolean state value that starts out with a value that hides the cart when the SPA is first visited. `const [isCartOpen, setIsCartOpen] = useState(false);` We then conditionally render the `Cart` based on `isCartOpen`. The code below provides the `App`'s full return statement:

```jsx
// App.jsx excerpt

return (
  <>
    <Header cart={cart} handleOpenCart={handleOpenCart} />
    <main>
      <ProductList
        inventory={inventory}
        handleAddItemToCart={handleAddItemToCart}></ProductList>
      {/*`isCartOpen has to be true for the cart to be rendered*/}
      {isCartOpen && <Cart cart={cart} />}
    </main>
    <footer>
      <p>
        Made with ❤️ | &copy; {year.current}{' '}
        <a href="https://codethedream.org/">CTD </a>
      </p>
    </footer>
  </>
);
```

Let's open the React dev tools to see how things are working. We don't have our button wired up yet but we should be able to modify that state value to show/hide the cart.

![showing state change in components tab](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-05/open-cart-state.gif)

#### Opening and Closing Cart

The cart is working so now it's time to make handlers to open and close the cart.

```jsx
//App.jsx
//component code...
function handleCloseCart() {
  //prevents re-render if unchanged
  if (isCartOpen) {
    setIsCartOpen(false);
  }
}

function handleOpenCart() {
  //prevents re-render if unchanged
  if (!isCartOpen) {
    setIsCartOpen(true);
  }
}
//component code...
```

We then need to add the props for the handler function into the component definition of the `Header`. When that's been added, we go back to `App` and pass the props into the `Header` `<Header cart={cart} handleOpenCart={handleOpenCart} />`. We complete the handling for opening of the cart by wrapping the image with a button element and then adding in an `onClick` props with the handler function. We also can remove the `useEffect` since it was just to log the cart contents to the console. The `Header`'s code will now look like this:

```jsx
//Header.jsx
import { useEffect } from 'react';
import ctdLogo from './assets/icons/mono-blue-logo.svg';
import shoppingCart from './assets/icons/shoppingCart.svg';

function Header({ cart, handleOpenCart }) {
  return (
    <header>
      <div className="siteBranding">
        <img src={ctdLogo} alt="Code The Dream Logo" />
        <h1>CTD Swag</h1>
      </div>
      <div className="shoppingCart">
        <button type="button" onClick={handleOpenCart}>
          <img src={shoppingCart} alt="" />
          <p className="cartCount">{cart.length}</p>
        </button>
      </div>
    </header>
  );
}

export default Header;
```

Since we already have the `handleCartClose` in the props of the `Cart` component, it's now a matter of passing the handler as props: `{isCartOpen && <Cart cart={cart} handleCloseCart={handleCloseCart} />}`.

![cart opening and closing](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-05/open-close-cart.gif)

#### Render a Message Instead of an Empty List

It may also be nicer for the user to see a message rather than rendering an empty list. Showing a message or a list is another conditional rendering scenario to address. The logic to change between the two elements is still simple enough that we don't need to extract a function for conditional rendering. The `&&` operator will not work here either so we are left with using a ternary. We look at the `cart`'s length and then determine if it's empty or not. If `cart.length === 0` then the lefthand expression executes, showing the message. If it contain items, the righthand executes, showing the list.

```jsx
// extract from Cart.jsx
//...component code
{
  cart.length === 0 ? (
    <p>cart is empty</p>
  ) : (
    <ul className="cartList">
      {cart.map((item) => {
        return (
          <li className="cartListItem" key="{item.cartItemId}">
            <img src="{placeholder}" alt="" />
            <h2>{item.baseName}</h2>
            <div className="cartListItemSubtotal">
              <p>${item.price}</p>
            </div>
          </li>
        );
      })}
    </ul>
  );
}
//component code...
```

We are left with updating the cart total. It's not important to hide this total from the user so we'll add the cart total after the ternary block that shows the list or the empty cart message. To calculate the total, we iterate over the `cart` items to add up the price. Since the logic to do this is a little more complicated, we create a function to return the cart total for display.

```jsx
//extract from Cart.jsx

function Cart({ cart, handleCloseCart }) {
  function getCartPrice() {
    // using `.toFixed` because floating point arithmetic
    // introduces suprising rounding issues
    // eg: `console.log(.99 + .99 +.99)` will print 2.9699999999999998
    return cart.reduce((acc, item) => acc + item.price, 0).toFixed(2);
  }
  return (
    <div className="cartListWrapper">
      //...component code
      <h2>Cart Total: ${getCartPrice()}</h2>
      <button onClick={handleCloseCart}>CloseCart</button>
    </div>
  );
}

export default Cart;
```

#### Updating Cart to Accept User Inputs

Our shopping cart looks pretty good but there are a few improvements that can be made. In shopping carts on other sites frequently users to make changes to their carts - they can remove items or change the quantity of items in the cart.

![updating cart at mechanicalkeyboards.com](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-05/keyboard-switch.gif)
![updating cart at louisianapantry.com](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-05/lousiana-pantry.gif)
Currently, the state value, `cart`, is just an array that lists the cart's contents. We want to update `cart` so it creates only one list item for each unique product. The product, in turn, contains data about how many times it's been added to the cart using `itemCount`. We can remove `cartItemId` property and then resume using the item's id as a key in the cart's list. An example cart item now looks like:

```js
//typical `cart` item
{
  "baseName": "Mouse Pad",
  "variantName": "Default"
  "id": "6",
  "price": 12.99,
  "baseDescription": "A pad perfect for mousing",
  "variantDescription": "",
  "image": "mouse-pad.png",
  "inStock": "TRUE",
  "itemCount": 3
}
```

We then need to update the handler that adds item to `cart`. After doing some error handling on `inventoryItem` we search the `cart` for an item that includes the product id. If one exists, the handler needs to increment `itemCount`. If not, it creates a new item to add to the `cart`.

```jsx
//extract from App.jsx

function handleAddItemToCart(id) {
  const inventoryItem = inventory.find((item) => item.id === id);
  if (!inventoryItem) {
    console.error('cart error: item not found');
    return;
  }
  const itemToUpdate = cart.find((item) => item.id === id);
  let updatedCartItem;
  if (itemToUpdate) {
    updatedCartItem = {
      ...itemToUpdate,
      itemCount: itemToUpdate.itemCount + 1,
    };
  } else {
    updatedCartItem = { ...inventoryItem, itemCount: 1 };
  }
  setCart([...cart.filter((item) => item.id !== id), updatedCartItem]);
}
```

We need to update the logic that provides the cart icon its number. Rather than retrieving the cart's length, we have to add together all of the item's `itemCount`s. We extract this to a function since `.reduce`would make the JSX difficult to read if we inline it between the paragraph tags.

```jsx
//Header.jsx

import ctdLogo from './assets/icons/mono-blue-logo.svg';
import shoppingCart from './assets/icons/shoppingCart.svg';

function Header({ cart, handleOpenCart }) {
  function getItemCount() {
    return cart.reduce((acc, item) => acc + item.itemCount, 0);
  }

  return (
    <header>
      <div className="siteBranding">
        <img src={ctdLogo} alt="Code The Dream Logo" />
        <h1>CTD Swag</h1>
      </div>
      <div className="shoppingCart">
        <button type="button" onClick={handleOpenCart}>
          <img src={shoppingCart} alt="" />
          <p className="cartCount">{getItemCount()}</p>
        </button>
      </div>
    </header>
  );
}

export default Header;
```

We can then update `Cart` component's JSX to reflect the changes we've made. We replace the item price with an item count and a price sub-total. The cart's total also needs to be recalculated.

```jsx
//Cart.jsx
import placeholder from './assets/placeholder.png';

function Cart({ cart, handleCloseCart }) {
  function getCartPrice() {
    return cart
      .reduce((acc, item) => acc + item.price * item.itemCount, 0)
      .toFixed(2);
  }

  return (
    <>
      <div className="cartScreen"></div>
      <div className="cartListWrapper">
        {cart.length === 0 ? (
          <p>cart is empty</p>
        ) : (
          <ul className="cartList">
            {cart.map((item) => {
              return (
                <li className="cartListItem" key={item.id}>
                  <img src={placeholder} alt="" />
                  <h2>{item.baseName}</h2>
                  {item.variantName !== 'Default' ? (
                    <p>{item.variantName}</p>
                  ) : null}
                  <div className="cartListItemSubtotal">
                    <p>Count: {item.itemCount}</p>
                    <p>Subtotal: ${(item.price * item.itemCount).toFixed(2)}</p>
                  </div>
                </li>
              );
            })}
          </ul>
        )}
        <h2>Cart Total: ${getCartPrice()}</h2>
        <button onClick={handleCloseCart}>CloseCart</button>
      </div>
    </>
  );
}

export default Cart;
```

With the cart ready, we can make its data updatable for users.

![cart open](https://raw.githubusercontent.com/Code-the-Dream-School/react-curriculum-v3/refs/heads/main/learns-app-content/lessons/assets/week-05/cart.png)
