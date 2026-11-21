# FinTrack: Queries Used

Database: Feintrack (MongoDB Atlas)
Collections: Users, Accounts, Transactions, Budgets

All queries below were run on the `Transactions` collection in MongoDB Atlas Data Explorer.

---

## Q1: All expenses (Read)
```
{ "type": "expense" }
```
Purpose: lists every expense transaction. Result: 6 documents.

---

## Q2: Food expenses above 400 (Read with condition)
```
{ "category": "Food", "amount": { "$gt": 400 } }
```
Purpose: finds food expenses greater than 400. Result: T102 and T105.

---

## Q3: Sort by amount, highest first (Sort)
```
{ "amount": -1 }
```
Purpose: shows the biggest transactions first. This is entered in the Sort box under Options.

---

## Q4: Insert a new transaction (Create)
```
{
  "txn_id": "T109",
  "account_id": "A1",
  "date": "2026-11-15",
  "type": "expense",
  "category": "Entertainment",
  "amount": 600,
  "note": "Movie"
}
```
Purpose: adds a new transaction record. The document count increases from 8 to 9.

---

## Q5: Update a transaction (Update)
Filter:
```
{ "txn_id": "T109" }
```
Change: `"amount": 600` to `"amount": 750`

Purpose: updates the amount of an existing transaction.

---

## Q6: Delete a transaction (Delete)
Filter:
```
{ "txn_id": "T109" }
```
Purpose: removes the test transaction. The document count returns to 8.

---

## Q7: Total spending per category (Aggregation)
```
[
  { "$match": { "type": "expense" } },
  { "$group": { "_id": "$category", "total_spent": { "$sum": "$amount" } } },
  { "$sort": { "total_spent": -1 } }
]
```
Purpose: calculates how much was spent in each category.
Result: Shopping 5600, Bills 2200, Food 1550, Travel 1200.

---

## Q8: Total income and expense (Aggregation)
```
[
  { "$group": { "_id": "$type", "total": { "$sum": "$amount" } } }
]
```
Purpose: gives the total income and total expense.
Result: income 42000, expense 10550.

---

## Q9: Join Transactions with Accounts ($lookup)
```
[
  {
    "$lookup": {
      "from": "Accounts",
      "localField": "account_id",
      "foreignField": "account_id",
      "as": "account_info"
    }
  },
  { "$limit": 3 }
]
```
Purpose: combines each transaction with its account details, which shows the relationship between the two collections.

---

## Operations covered
Create, Read, Update, Delete (CRUD), Sorting, Aggregation ($match, $group, $sort) and Join ($lookup).
