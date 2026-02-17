# React Redux Bank

A small learning project that demonstrates Redux state management with React.
The app models a simple bank workflow:

- Create a customer profile
- Deposit and withdraw money
- Request and pay back a loan
- Convert non-USD deposits to USD via an async thunk

## Tech Stack

- React 19
- Redux Toolkit
- React Redux
- Create React App

## How The App Works

The UI flow is controlled by customer state:

1. If no customer exists (`customer.fullName === ""`), the app shows the customer creation form.
2. After creating a customer, the app shows `Customer`, `AccountOperations`, and `BalanceDisplay`.

Account state is stored in Redux and updated through slice reducers and thunk actions.

## State Shape

### `account` slice

```js
{
  balance: 0,
  loan: 0,
  loanPurpose: "",
  isLoading: false
}
```

### `customer` slice

```js
{
  fullName: "",
  nationalID: "",
  createdAt: ""
}
```

## Key Redux Behavior

### Account actions (`src/features/accounts/accountSlice.js`)

- `deposit(amount, currency)`
- If `currency === "USD"`, dispatches sync deposit.
- If not USD, dispatches `account/convertingCurrency`, calls Frankfurter API, then dispatches converted USD amount.
- `withdraw(amount)`
- Decreases balance by amount.
- `requestLoan(amount, purpose)`
- Adds loan only when no existing loan is active.
- `payLoan()`
- Deducts current loan from balance, then clears loan data.

### Customer actions (`src/features/customers/customerSlice.js`)

- `createCustomer(fullName, nationalID)`
- Uses a prepared action payload that includes `createdAt` timestamp.
- `updateName(fullName)`
- Updates customer name.

## Project Structure

```text
src/
  App.js
  index.js
  store.js
  features/
    accounts/
      accountSlice.js
      AccountOperations.js
      BalanceDisplay.js
      accountSlice-v2.js
    customers/
      customerSlice.js
      CreateCustomer.js
      Customer.js
      customerSlice-v2.js
  store-v1.js
  store-v2.js
```

## Active vs Learning Files

Active runtime files:

- `src/store.js`
- `src/features/accounts/accountSlice.js`
- `src/features/customers/customerSlice.js`
- `src/App.js`

Learning/reference snapshots:

- `src/store-v1.js` (classic Redux example)
- `src/store-v2.js` (Redux + thunk + devtools middleware setup)
- `src/features/accounts/accountSlice-v2.js` (non-RTK reducer version)
- `src/features/customers/customerSlice-v2.js` (non-RTK reducer version)

## Setup

Install dependencies:

```bash
npm install
```

Run in development:

```bash
npm start
```

Open `http://localhost:3000`.

## Scripts

- `npm start` - Start development server
- `npm test` - Run tests in watch mode
- `npm run build` - Create production build
- `npm run eject` - Eject CRA configuration (irreversible)

## External API

Currency conversion uses:

- `https://api.frankfurter.app/latest`

Used when depositing in `EUR` or `GBP`, converted into USD before balance update.
