# eCommerce Order System for React

| #    | Version | Change Log    |
| ---- | ------- | ------------- |
| 1    | 1.0     | Initial Draft |

This document covers software specifications and design contraints that needs to be followed while developing the module

------

### Proposal

To develop a set of modules (for Ark-React, Ark-Express and Ark-React-Native) that enables a system to safely and reliably add products / services to Cart and / or Wishlist.

Users must be able to reliably place orders. The module must support for external integration to third party payment providers. 

#### Abstract

FreePizza eCommerce module aims at providing flexible order processing functionalities to an existing / new Ark project. The projects does not necessarily be an eCommerce centric one.

The module must be able to communicate with a suitable inventory management system to check availability of stock before creating order.

#### Proposed Features

1. Cart View
   1. Add to Cart
   2. Update Quantity
   3. Remove Item
   4. Add / Delete Coupon Code
   5. Checkout
      1. Validate Cart (Availability & Remaning stock)
2. Wishlist
   1. Add to Wishlist
   2. Remove Items
3. Order System
   1. Create Order from Cart
   2. Create Order Manually (using Product / Service)
   3. Order Modifier Steps
      1. Billing Address
      2. Delivery Options
   4. Select Payment Option [Core Modifier]
   5. Thank You Page
4. My Orders
   1. See all orders
   2. See order details > items > total amount > payment option > delivery / billing address / other data collected from order modifiers
   3. Cancel order

#### Customisability Evaluation

The cart, wishlist and order processing should be seperated into three different views. In such a way that, developers can use any one of these pages without relying or having to include other pages.

Developers should be able to add / edit / customise Order Modifier Steps. Core Modifiers should be written in a separate file.

System should support easy adoption and integration of new payment providers. However, this scope is limited to Cash-On-Delivery option only.

#### Design Constraints

The UI / UX should follow our general module design guidelines. Additionally, user should feel secure and confident from the point a user adds items to cart until receipt and managing placed orders

------

### Architecture

FreePizza modules are developed to be used with Ark Framework

#### User Flow

> Empty

#### Module State (Redux)

```typescript
type CartItem = {
  _id: string
  itemType: 'cart' | 'wishlist'
  cartId: string
  title: string
  sku: string
  productId: string
  variantId: string
  imageUrl: string
  isAvailable: boolean
  effectiveQuantity: number
  quantity: number
  totalAmount: number
}

type Address = {
  name: string
  companyName: string
  addressLine1: string
  addressLine2?: string
  city: string
  postalCode: string
  state: string
  country: string
}

type OrderActivity = {
  orderId: string
  userId?: string
  changeType: string | 'status_update' | 'payment_status_update' | 'other'
  title: string
  description: string
  providerId?: string
  providerPayload?: any
  createdAt: string
}

type Order = {
 	_id: string
  userId: string
  emailAddress: string
  orderNumber: string
  items: Array<CartItem>
 	billingAddress: Address
  deliveryAddress?: Address
  // Any modifier info can be added here (like delivery option)
  deliveryCharge: number
  deliveryNarration: string
  currencyCode: 'USD' | 'INR' | string
  status: 'placed' | 'confirmed' | 'dispatched' | 'delivered'
  paymentStatus: 'paid' | 'unpaid' | 'partial'
  activities: Array<OrderActivity>
  grossAmount: number // Without delivery charge
  serviceCharge: number
  netAmount: number // gross + delivery + service charge
  amountPaid: number
  balanceAmount: number // netAmount - amountPaid
  createdAt: string
}

type State = {
  items: Array<CartItem>
  wishlistItems: Array<CartItem>
  orders: Array<Order>
  itemsPerLoad: number
}
```



#### Views

1. Cart.View

2. Wishlist.View

3. MyOrders.View

   > The page should support pagination (load only certain number of order at a time)

4. Checkout.View

#### Controller Specification

```typescript
type Controller = {
  addToCart: (type: 'cart' | 'wishlist', itemPayload: CartItem) => void
  updateCartQuantity: (cartId: number, cartId: number, quantity: number)
  removeItem: (type: 'cart' | 'wishlist', cartId: number)
	fetchOrders: (count: number) // count arg defaults to the one in state (itemsPerLoad)
}
```



#### Reducer Specification

> Empty



#### Service Specification (Declaration Map)

```typescript

```



