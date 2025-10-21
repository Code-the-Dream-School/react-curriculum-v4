# Helper Functions List

This document provides a comprehensive list of all helper functions used throughout the CTD Swag application, organized by file location and purpose.

## **Utility Functions** (src/utils/)

### **sortByBaseName.js**

- `sortByBaseName({ productItems, isSortAscending })` - Sorts product items alphabetically by base name

### **sortByPrice.js**

- `sortByPrice({ productItems, isSortAscending })` - Sorts product items by price (ascending or descending)

### **filterByQuery.js**

- `filterByQuery({ productItems, searchTerm })` - Filters products based on search term matching name, description, or variant description

### **convertInventoryToProducts.js**

- `convertInventoryToProducts(inventory)` - Converts flat inventory array into grouped products with variants

## **State Management Functions** (src/reducers/)

### **cart.reducer.js**

- `cartReducer(state, action)` - Main reducer function for cart state management

## **Component Helper Functions**

### **Main Application** (src/App.jsx)

- `syncCartWithServer(workingCart, userToken)` - Syncs cart data with backend server (async)
- `handleAuthenticate(credentials)` - Handles user login authentication (async)
- `handleRegister(user)` - Handles new user registration (async)
- `handleAddItemToCart(id)` - Adds item to shopping cart (async)
- `handleCloseCart()` - Closes the cart modal
- `handleLogOut()` - Logs out user and resets cart
- `handleOpenAuthDialog(option)` - Opens authentication dialog (login/register)
- `handleCloseDialog()` - Closes dialog and dismisses cart errors

### **Shopping Cart** (src/features/Cart/Cart.jsx)

- `getCartPrice()` - Calculates total price of items in cart
- `handleUpdateField({ event, id })` - Updates quantity of cart item
- `handleCancel(e)` - Cancels cart updates and reverts changes
- `handleCheckout(e)` - Navigates to checkout page
- `removeEmptyItems(cart)` - Filters out items with zero quantity
- `handleConfirm(e)` - Confirms cart updates

### **Authentication Forms**

#### **LoginForm.jsx** (src/features/Auth/LoginForm.jsx)

- `handleSubmit(e)` - Handles login form submission

#### **RegisterForm.jsx** (src/features/Auth/RegisterForm.jsx)

- `handleSubmit(e)` - Handles registration form submission

### **Product Filtering** (src/features/ProductListFilter/ProductListFilter.jsx)

- `handleSortDirectionChange(e)` - Handles sort direction change (ascending/descending)
- `handleClearTerm(event)` - Clears search term filter

### **Checkout Page** (src/pages/Checkout/Checkout.jsx)

- `getTotal()` - Calculates total price for checkout

### **Test Helper Functions** (src/features/ProductList/ProductCard.test.jsx)

- `setup(jsx)` - Test utility function for setting up user events in tests

## **Event Handlers by Category**

### **Cart Management**

- `handleAddItemToCart()` - Add items to cart
- `handleUpdateField()` - Update cart item quantities
- `handleCloseCart()` - Close cart modal
- `handleConfirm()` - Confirm cart changes
- `handleCancel()` - Cancel cart changes
- `removeEmptyItems()` - Clean up cart

### **Authentication**

- `handleAuthenticate()` - Login process
- `handleRegister()` - Registration process
- `handleLogOut()` - Logout process
- `handleOpenAuthDialog()` - Open auth modals
- `handleCloseDialog()` - Close dialogs

### **Product Operations**

- `sortByBaseName()` - Sort products by name
- `sortByPrice()` - Sort products by price
- `filterByQuery()` - Filter products by search
- `convertInventoryToProducts()` - Transform inventory data

### **Form Handling**

- `handleSubmit()` - Form submission (login/register)
- `handleSortDirectionChange()` - Sort direction updates
- `handleClearTerm()` - Clear search filters

### **Calculation Functions**

- `getTotal()` - Calculate checkout total
- `getCartPrice()` - Calculate cart total

## **Async Functions**

Functions that perform asynchronous operations:

- `syncCartWithServer()` - Server synchronization
- `handleAuthenticate()` - API authentication
- `handleRegister()` - API registration
- `handleAddItemToCart()` - Cart state updates

## **Summary**

**Total: 25+ helper functions** distributed across:

- **8 utility/pure functions** - Data transformation and calculations
- **8 main app functions** - Core application logic
- **6 cart management functions** - Shopping cart operations
- **4 authentication functions** - User management
- **3 form handling functions** - User input processing
- **2 calculation functions** - Price/total computations
- **1 state reducer function** - State management
- **1 test utility function** - Testing support

The helper functions follow clear naming conventions with prefixes like `handle*` for event handlers, `get*` for calculations, and descriptive names for utility functions. Most functions are pure or have minimal side effects, making the codebase maintainable and testable.
