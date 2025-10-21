# React Components List

This document provides a comprehensive list of all React components used in the CTD Swag application.

## **Main Application**

- `App` - Main application component (src/App.jsx)

## **Layout Components**

- `Header` - Navigation header with logo, auth buttons, and cart (src/layout/Header.jsx)
- `Footer` - Site footer with copyright info (src/layout/Footer.jsx)

## **Page Components**

- `Shop` - Main shopping page that combines product list and filters (src/pages/Shop/Shop.jsx)
- `Account` - User account page displaying user info (src/pages/Account/Account.jsx)
- `Checkout` - Checkout page with cart items and totals (src/pages/Checkout/Checkout.jsx)
- `ProductDetail` - Individual product details page (src/pages/Product/ProductDetails.jsx)
- `NotFound` - 404 error page (src/pages/404/404.jsx)

## **Feature Components**

### **Authentication**

- `AuthDialog` - Authentication modal wrapper (src/features/Auth/AuthDialog.jsx)
- `LoginForm` - Login form component (src/features/Auth/LoginForm.jsx)
- `RegisterForm` - User registration form (src/features/Auth/RegisterForm.jsx)

### **Shopping Cart**

- `Cart` - Shopping cart sidebar/modal (src/features/Cart/Cart.jsx)
- `CartItem` - Individual cart item with quantity controls (src/features/Cart/CartItem.jsx)

### **Product Display**

- `ProductList` - Grid/list of product cards (src/features/ProductList/ProductList.jsx)
- `ProductCard` - Individual product card in the list (src/features/ProductList/ProductCard.jsx)
- `ProductCardVariants` - Product variant selection overlay (src/features/ProductList/ProductCardVariants.jsx)
- `ProductDetailsCard` - Product card used on detail pages (src/pages/Product/ProductDetailsCard.jsx)

### **Filtering & Search**

- `ProductListFilter` - Sort and search controls (src/features/ProductListFilter/ProductListFilter.jsx)

### **Checkout**

- `CheckOutItem` - Individual item display on checkout page (src/pages/Checkout/CheckoutItem.jsx)

## **Shared/Common Components**

- `Dialog` - Reusable dialog/modal component for alerts (src/shared/Dialog.jsx)

## **Styled Components**

The application also uses several styled-components within `ProductCard.jsx`:

- `Card` - Styled list item for product cards
- `Preview` - Styled product image container
- `Copy` - Styled text content area
- `Details` - Styled product details text
- `ButtonWrapper` - Styled button container

## **Summary**

**Total: 21 React components** across different feature areas including authentication, shopping cart, product display, user management, and shared utilities. The application follows a well-organized component structure with clear separation between pages, features, layout, and shared components.

## **Component Architecture**

The components are organized into the following directory structure:

- `src/` - Root source directory
  - `features/` - Feature-specific components (Auth, Cart, ProductList, etc.)
  - `layout/` - Layout components (Header, Footer)
  - `pages/` - Page-level components (Shop, Account, Checkout, etc.)
  - `shared/` - Reusable utility components
  - `utils/` - Utility functions (not components)
  - `reducers/` - State management logic
