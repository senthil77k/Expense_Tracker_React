# Expense Tracker (ReactJS)
## Date:24.05.2025

## AIM
To develop a simple Expense Tracker application using React that allows users to manage their personal finances by adding, viewing, and deleting income and expense transactions, while dynamically calculating the current balance, total income, and total expenses.

## ALGORITHM
### STEP 1: Initialize the Project
Create a new React app using:

npx create-react-app expense-tracker
or

npm create vite@latest expense-tracker --template react

Open the project in a code editor like VS Code.

### Step 2: Setup State
Define a state variable to store transactions:

Example: const [transactions, setTransactions] = useState([])

Define state variables for the form inputs:

const [text, setText] = useState("")

const [amount, setAmount] = useState("")

### Step 3: Add a New Transaction
Create a form with two inputs:

Text input for description

Number input for amount

On form submit:

Validate input

Create a new transaction object

Add the object to the transactions array using setTransactions.

### Step 4: Display Transaction List

Use map() to render each transaction in a list.

Conditionally style each item based on amount > 0 (income) or amount < 0 (expense).

Add a delete button next to each transaction.

### Step 5: Calculate and Display Summary

Use reduce() to calculate:

Total Balance: sum of all amounts

Total Income: sum of all positive amounts

Total Expenses: sum of all negative amounts

Display these values at the top of the UI.

### Step 6: Delete a Transaction

When delete is clicked:

Use filter() to remove the transaction from the array by id.

Update the state using setTransactions.

### Step 7: Style the Application

Use basic CSS to style:

Balance summary

Income/expense totals

Form inputs

Transaction list (with color coding)

## PROGRAM

##app.jx
```
import React, { useState } from 'react';
import './App.css';

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState('');
  const [amount, setAmount] = useState('');

  const addTransaction = (e) => {
    e.preventDefault();
    if (!description || !amount) return;
    
    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount)
    };
    
    setTransactions([...transactions, newTransaction]);
    setDescription('');
    setAmount('');
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter(transaction => transaction.id !== id));
  };

  const balance = transactions.reduce((acc, transaction) => acc + transaction.amount, 0);
  const income = transactions
    .filter(transaction => transaction.amount > 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);
  const expenses = transactions
    .filter(transaction => transaction.amount < 0)
    .reduce((acc, transaction) => acc + transaction.amount, 0);

  return (
    <div className="app">
      <header className="header">
        <h1>Expense Tracker</h1>
        <div className="balance-box">
          <h3>Your Balance</h3>
          <h2>${balance.toFixed(2)}</h2>
        </div>
        <div className="summary">
          <div className="income">
            <h4>INCOME</h4>
            <p className="money plus">+${income.toFixed(2)}</p>
          </div>
          <div className="expense">
            <h4>EXPENSE</h4>
            <p className="money minus">-${Math.abs(expenses).toFixed(2)}</p>
          </div>
        </div>
      </header>

      <div className="transaction-list">
        <h3>Transactions</h3>
        <ul>
          {transactions.map(transaction => (
            <li key={transaction.id} className={transaction.amount > 0 ? 'plus' : 'minus'}>
              <span>{transaction.description}</span>
              <span>{transaction.amount > 0 ? '+' : ''}{transaction.amount.toFixed(2)}</span>
              <button 
                onClick={() => deleteTransaction(transaction.id)}
                className="delete-btn"
              >
                ×
              </button>
            </li>
          ))}
        </ul>
      </div>

      <form onSubmit={addTransaction} className="transaction-form">
        <h3>Add New Transaction</h3>
        <div className="form-control">
          <label htmlFor="description">Description</label>
          <input
            type="text"
            id="description"
            value={description}
            onChange={(e) => setDescription(e.target.value)}
            placeholder="Enter description..."
          />
        </div>
        <div className="form-control">
          <label htmlFor="amount">Amount</label>
          <input
            type="number"
            id="amount"
            value={amount}
            onChange={(e) => setAmount(e.target.value)}
            placeholder="Enter amount..."
            step="0.01"
          />
          <small>(Positive for income, negative for expense)</small>
        </div>
        <button type="submit" className="btn">Add Transaction</button>
      </form>
    </div>
  );
}

export default App;
```
## css
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Poppins', sans-serif;
}

body {
  background: linear-gradient(120deg, #fdfbfb 0%, #ebedee 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
}

.app {
  background: #ffffff;
  border-radius: 20px;
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 520px;
  padding: 30px;
  margin: 20px 0;
}

.header {
  text-align: center;
  margin-bottom: 25px;
}

.header h1 {
  color: #34495e;
  font-size: 30px;
  margin-bottom: 15px;
  border-bottom: 3px solid #8e44ad;
  display: inline-block;
  padding-bottom: 5px;
}

.balance-box {
  background: linear-gradient(to right, #8e44ad, #3498db);
  color: #ffffff;
  padding: 18px;
  border-radius: 12px;
  margin-bottom: 25px;
}

.balance-box h3 {
  font-size: 16px;
  margin-bottom: 6px;
}

.balance-box h2 {
  font-size: 30px;
}

.summary {
  display: flex;
  justify-content: space-between;
  gap: 15px;
  margin-top: 25px;
}

.income, .expense {
  flex: 1;
  background-color: #f4f6f8;
  border-left: 5px solid;
  border-radius: 10px;
  padding: 15px;
  text-align: center;
}

.income {
  border-color: #27ae60;
}

.expense {
  border-color: #c0392b;
}

.money {
  font-weight: bold;
  font-size: 22px;
}

.plus {
  color: #27ae60;
}

.minus {
  color: #c0392b;
}

.transaction-list h3 {
  padding-bottom: 8px;
  margin-bottom: 18px;
  color: #2c3e50;
  border-bottom: 2px dashed #bdc3c7;
}

.transaction-list ul {
  list-style-type: none;
  padding-left: 0;
}

.transaction-list li {
  background-color: #fefefe;
  padding: 14px;
  margin-bottom: 12px;
  border-radius: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  box-shadow: 0 3px 7px rgba(0, 0, 0, 0.05);
  border-left: 5px solid;
  transition: background 0.3s;
}

.transaction-list li.plus {
  border-color: #27ae60;
}

.transaction-list li.minus {
  border-color: #c0392b;
}

.delete-btn {
  background-color: #c0392b;
  color: #fff;
  border: none;
  border-radius: 50%;
  width: 28px;
  height: 28px;
  font-size: 18px;
  line-height: 28px;
  cursor: pointer;
  opacity: 0;
  transition: opacity 0.3s ease-in-out;
}

.transaction-list li:hover .delete-btn {
  opacity: 1;
}

.transaction-form {
  margin-top: 35px;
}

.transaction-form h3 {
  margin-bottom: 18px;
  color: #34495e;
  border-bottom: 2px solid #bdc3c7;
  padding-bottom: 10px;
}

.form-control {
  margin-bottom: 20px;
}

.form-control label {
  display: block;
  margin-bottom: 8px;
  color: #34495e;
  font-weight: 600;
}

.form-control input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 16px;
  background-color: #fdfdfd;
}

.form-control small {
  color: #7f8c8d;
  font-size: 13px;
  margin-top: 4px;
  display: inline-block;
}

.btn {
  width: 100%;
  padding: 14px;
  background: linear-gradient(to right, #8e44ad, #3498db);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.btn:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 18px rgba(0, 0, 0, 0.12);
}

@media (max-width: 500px) {
  .app {
    padding: 20px;
  }

  .summary {
    flex-direction: column;
  }

  .income, .expense {
    margin: 10px 0;
  }
}

```

## OUTPUT
![image](https://github.com/user-attachments/assets/2c8164ae-43ca-4093-9683-b978aef0bfb3)


## RESULT
A fully functional React-based Expense Tracker application was successfully developed. 
