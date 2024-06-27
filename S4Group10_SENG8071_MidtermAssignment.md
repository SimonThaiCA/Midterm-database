# Online Bookstore

## ID & Name
|     ID        |    Name        |
| ------------- |:-------------: |
|  8853954      | XinmengTai     |
|  0000000      | Gurpreet Singh |
|  0000000      | Himanshu       |

## Tables Required
1. Customers: Stores information about the customers.
* Attributes: CustomerID (Integer), Name (String), TotalSpent (DECIMAL)
2. Books: Stores information about the books.
* Attributes: BookID (Integer), Title (String), Genre (String), Format (String), Price (DECIMAL), AuthorName (VARCHAR), Publish_Date（Date）， Rating (DECIMAL), NumReviews (Integer)
3. Reviews: Stores the reviews given by customers.
* Attributes: ReviewID (Integer), CustomerID (Integer), BookID (Integer), Rating (DECIMAL), ReviewText (String), ReviewDate (Date)
4. Sales: Stores information about the sales.
* Attributes: SaleID (Integer), CustomerID (Integer), BookID (Integer), SaleDate (Date), Amount (DECIMAL)

## DDL & DML
### Creat Table
```sql
-- Custmers Table
    CREATE TABLE Customers (
        customer_ID INT PRIMARY KEY,
        Name VARCHAR(100),
        Email VARCHAR(100),
        join_date DATE NOT NULL,
        TotalSpent DECIMAL(10, 2)
    );

-- Books Table
    CREATE TABLE Books (
        book_ID INT PRIMARY KEY,
        Title VARCHAR(100) NOT NULL,
        Genre VARCHAR(100) NOT NULL,
        Format VARCHAR(100) NOT NULL CHECK(Format IN('physical books', 'e-books', 'audiobooks')),
        Price DECIMAL(10, 2) NOT NULL,
        AuthorName VARCHAR(100) NOT NULL,
        Publish_Date DATE NOT NULL,
        Rating DECIMAL(2, 1) Check(Rating >= 1.0 AND Rating <= 5.0>),
        NumReviews INT,
    );

-- Reviews Table
    CREATE TABLE Reviews (
        ReviewID INT PRIMARY KEY,
        CustomerID INT,
        BookID INT,
        Rating DECIMAL(2, 1) Check(Rating >= 1.0 AND Rating <= 5.0>),
        ReviewText TEXT,
        ReviewDate DATE,
        FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
        FOREIGN KEY (BookID) REFERENCES Books(BookID)
    );

-- Sales Table
    CREATE TABLE Sales (
        SaleID INT PRIMARY KEY,
        CustomerID INT,
        BookID INT,
        SaleDate DATE NOT NULL,
        Amount DECIMAL(10, 2) NOT NULL,
        FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID),
        FOREIGN KEY (BookID) REFERENCES Books(BookID)
    );
```
### Insert mock data
```sql
INSERT INTO Customers (customer_ID, Name, Email, join_date, TotalSpent) VALUES (1, 'Tim Sakamoto', 'sakamoto@jp.com', '2022-01-01', 0.0);

INSERT INTO Books (book_ID, Title, Genre, Format, Price, AuthorName, Publish_Date, Rating, NumReviews) VALUES (1, 'Book Title', 'Genre', 'e-books', 10.0, 'Author Name', '2022-01-01', 4.5, 100);

INSERT INTO Reviews (ReviewID, CustomerID, BookID, Rating, ReviewText, ReviewDate) VALUES (1, 1, 1, 4.5, 'Great book!', '2022-01-01');

INSERT INTO Sales (SaleID, CustomerID, BookID, SaleDate, Amount) VALUES (1, 1, 1, '2022-01-01', 10.0);
```
### Read
```sql
SELECT * FROM Customers WHERE customer_ID = 1;

SELECT * FROM Books WHERE book_ID = 1;

SELECT * FROM Reviews WHERE ReviewID = 1;

SELECT * FROM Sales WHERE SaleID = 1;
```

### Update
```sql
UPDATE Customers SET TotalSpent = 50.0 WHERE customer_ID = 1;

UPDATE Books SET Price = 20.0 WHERE book_ID = 1;

UPDATE Reviews SET Rating = 5.0 WHERE ReviewID = 1;

UPDATE Sales SET Amount = 20.0 WHERE SaleID = 1;
```
### Delete
```sql
DELETE FROM Customers WHERE customer_ID = 1;

DELETE FROM Books WHERE book_ID = 1;

DELETE FROM Reviews WHERE ReviewID = 1;

DELETE FROM Sales WHERE SaleID = 1;
```

### Typescript interface
```
interface Customer {
  customer_ID: number;
  Name: string;
  Email: string;
  join_date: Date;
  TotalSpent: number;
}

interface Book {
  book_ID: number;
  Title: string;
  Genre: string;
  Format: string;
  Price: number;
  AuthorName: string;
  Rating: number;
  NumReviews: number;
}

interface Review {
  ReviewID: number;
  CustomerID: number;
  BookID: number;
  Rating: number;
  ReviewText: string;
  ReviewDate: Date;
}

interface Sale {
  SaleID: number;
  CustomerID: number;
  BookID: number;
  SaleDate: Date;
  Amount: number;
}
```
```
import pgPromise from 'pg-promise';

const pgp = pgPromise();
const db = pgp('postgres://username:password@localhost:3006/database');

// Customers Table
async function createCustomer(customer: Customer) {
  await db.none('INSERT INTO Customers VALUES($1, $2, $3, $4, $5)', [customer.customer_ID, customer.Name, customer.Email, customer.join_date, customer.TotalSpent]);
}

async function readCustomer(customer_ID: number) {
  return await db.one('SELECT * FROM Customers WHERE customer_ID = $1', [customer_ID]);
}

async function updateCustomer(customer: Customer) {
  await db.none('UPDATE Customers SET TotalSpent = $2 WHERE customer_ID = $1', [customer.customer_ID, customer.TotalSpent]);
}

async function deleteCustomer(customer_ID: number) {
  await db.none('DELETE FROM Customers WHERE customer_ID = $1', [customer_ID]);
}

// Books Table
async function createBook(book: Book) {
  await db.none('INSERT INTO Books VALUES($1, $2, $3, $4, $5, $6, $7, $8, $9)', [book.book_ID, book.Title, book.Genre, book.Format, book.Price, book.AuthorName, book.Publish_Date, book.Rating, book.NumReviews]);
}

async function readBook(book_ID: number) {
  return await db.one('SELECT * FROM Books WHERE book_ID = $1', [book_ID]);
}

async function updateBook(book: Book) {
  await db.none('UPDATE Books SET Price = $2 WHERE book_ID = $1', [book.book_ID, book.Price]);
}

async function deleteBook(book_ID: number) {
  await db.none('DELETE FROM Books WHERE book_ID = $1', [book_ID]);
}

// Reviews Table
async function createReview(review: Review) {
  await db.none('INSERT INTO Reviews VALUES($1, $2, $3, $4, $5, $6)', [review.ReviewID, review.CustomerID, review.BookID, review.Rating, review.ReviewText, review.ReviewDate]);
}

async function readReview(ReviewID: number) {
  return await db.one('SELECT * FROM Reviews WHERE ReviewID = $1', [ReviewID]);
}

async function updateReview(review: Review) {
  await db.none('UPDATE Reviews SET Rating = $2 WHERE ReviewID = $1', [review.ReviewID, review.Rating]);
}

async function deleteReview(ReviewID: number) {
  await db.none('DELETE FROM Reviews WHERE ReviewID = $1', [ReviewID]);
}

// Sales Table
async function createSale(sale: Sale) {
  await db.none('INSERT INTO Sales VALUES($1, $2, $3, $4, $5)', [sale.SaleID, sale.CustomerID, sale.BookID, sale.SaleDate, sale.Amount]);
}

async function readSale(SaleID: number) {
  return await db.one('SELECT * FROM Sales WHERE SaleID = $1', [SaleID]);
}

async function updateSale(sale: Sale) {
  await db.none('UPDATE Sales SET Amount = $2 WHERE SaleID = $1', [sale.SaleID, sale.Amount]);
}

async function deleteSale(SaleID: number) {
  await db.none('DELETE FROM Sales WHERE SaleID = $1', [SaleID]);
}

```