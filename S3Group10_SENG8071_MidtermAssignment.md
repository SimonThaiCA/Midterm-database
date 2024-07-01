# Online Bookstore

## ID & Name
|     ID        |    Name        |
| ------------- |:-------------: |
|  8853954      | Xinmeng Tai    |
|  8975528      | Gurpreet Singh Bhathal|
|  8956003      | Himanshu Deshwal |

github address:https://github.com/SimonThaiCA/Midterm-database

## Tables Required
1. Customers: Stores information about the customers.
* Attributes: customer_ID (Integer), Name (String), TotalSpent (DECIMAL)
2. Books: Stores information about the books.
* Attributes: book_ID (Integer), Title (String), Genre (String), Format (String), Price (DECIMAL), AuthorName (VARCHAR), Publish_Date（Date）， Rating (DECIMAL), NumReviews (Integer)
3. Reviews: Stores the reviews given by customers.
* Attributes: review_ID (Integer), customer_ID (Integer), book_ID (Integer), Rating (DECIMAL), ReviewText (String), ReviewDate (Date)
4. Sales: Stores information about the sales.
* Attributes: sale_ID (Integer), customer_ID (Integer), book_ID (Integer), SaleDate (Date), Amount (DECIMAL)

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
        review_ID INT PRIMARY KEY,
        customer_ID INT,
        book_ID INT,
        Rating DECIMAL(2, 1) Check(Rating >= 1.0 AND Rating <= 5.0>),
        ReviewText TEXT,
        ReviewDate DATE,
        FOREIGN KEY (customer_ID) REFERENCES Customers(customer_ID),
        FOREIGN KEY (book_ID) REFERENCES Books(book_ID)
    );

-- Sales Table
    CREATE TABLE Sales (
        sale_ID INT PRIMARY KEY,
        customer_ID INT,
        book_ID INT,
        SaleDate DATE NOT NULL,
        Amount DECIMAL(10, 2) NOT NULL,
        FOREIGN KEY (customer_ID) REFERENCES Customers(customer_ID),
        FOREIGN KEY (book_ID) REFERENCES Books(book_ID)
    );
```
### Insert mock data
```sql
INSERT INTO Customers (customer_ID, Name, Email, join_date, TotalSpent) VALUES (1, 'Tim Sakamoto', 'sakamoto@jp.com', '2022-01-01', 0.0);

INSERT INTO Books (book_ID, Title, Genre, Format, Price, AuthorName, Publish_Date, Rating, NumReviews) VALUES (1, 'Book Title', 'Genre', 'e-books', 10.0, 'Author Name', '2022-01-01', 4.5, 100);

INSERT INTO Reviews (review_ID, customer_ID, book_ID, Rating, ReviewText, ReviewDate) VALUES (1, 1, 1, 4.5, 'Great book!', '2022-01-01');

INSERT INTO Sales (sale_ID, customer_ID, book_ID, SaleDate, Amount) VALUES (1, 1, 1, '2022-01-01', 10.0);
```
### Read
```sql
SELECT * FROM Customers WHERE customer_ID = 1;

SELECT * FROM Books WHERE book_ID = 1;

SELECT * FROM Reviews WHERE review_ID = 1;

SELECT * FROM Sales WHERE sale_ID = 1;
```

### Update
```sql
UPDATE Customers SET TotalSpent = 50.0 WHERE customer_ID = 1;

UPDATE Books SET Price = 20.0 WHERE book_ID = 1;

UPDATE Reviews SET Rating = 5.0 WHERE review_ID = 1;

UPDATE Sales SET Amount = 20.0 WHERE sale_ID = 1;
```
### Delete
```sql
DELETE FROM Customers WHERE customer_ID = 1;

DELETE FROM Books WHERE book_ID = 1;

DELETE FROM Reviews WHERE review_ID = 1;

DELETE FROM Sales WHERE sale_ID = 1;
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
  review_ID: number;
  customer_ID: number;
  book_ID: number;
  Rating: number;
  ReviewText: string;
  ReviewDate: Date;
}

interface Sale {
  sale_ID: number;
  customer_ID: number;
  book_ID: number;
  SaleDate: Date;
  Amount: number;
}
```
```
import pgPromise from 'pg-promise';

const pgp = pgPromise();
const db = pgp('postgres://username:password@localhost:3006/database');

// Customers Table --Xinmeng Tai (8853954)
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

// Books Table -- Gurpreet Singh Bhathal(8975528)
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

// Reviews Table -- Himanshu Deshwal(8956003)
async function createReview(review: Review) {
  await db.none('INSERT INTO Reviews VALUES($1, $2, $3, $4, $5, $6)', [review.review_ID, review.customer_ID, review.book_ID, review.Rating, review.ReviewText, review.ReviewDate]);
}

async function readReview(review_ID: number) {
  return await db.one('SELECT * FROM Reviews WHERE review_ID = $1', [review_ID]);
}

async function updateReview(review: Review) {
  await db.none('UPDATE Reviews SET Rating = $2 WHERE review_ID = $1', [review.review_ID, review.Rating]);
}

async function deleteReview(review_ID: number) {
  await db.none('DELETE FROM Reviews WHERE review_ID = $1', [review_ID]);
}

// Sales Table -- Xinmeng Tai (8853954), Gurpreet Singh Bhathal(8975528), Himanshu Deshwal(8956003)
async function createSale(sale: Sale) {
  await db.none('INSERT INTO Sales VALUES($1, $2, $3, $4, $5)', [sale.sale_ID, sale.customer_ID, sale.book_ID, sale.SaleDate, sale.Amount]);
}

async function readSale(sale_ID: number) {
  return await db.one('SELECT * FROM Sales WHERE sale_ID = $1', [sale_ID]);
}

async function updateSale(sale: Sale) {
  await db.none('UPDATE Sales SET Amount = $2 WHERE sale_ID = $1', [sale.sale_ID, sale.Amount]);
}

async function deleteSale(sale_ID: number) {
  await db.none('DELETE FROM Sales WHERE sale_ID = $1', [sale_ID]);
}

```
