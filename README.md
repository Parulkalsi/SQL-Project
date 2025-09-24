use sql_project;
show tables;
select * from books;
select * from customers;
select * from orders;
# 1-Retrieve all books in the "Fiction" genre
select * from books where genre ="Fiction";
# 2- Find books published after the year 1950
select * from books where Published_Year > 1950;
# 3- List all customers from the Canada
select * from customers where country="Canada";
# 4- Show orders placed in November 2023
select * from orders where Order_Date between '2023-11-01' and '2023-11-30';
# 5- Retrieve the total stock of books available
select sum(stock) as Total_Stock from books;
# 6- Find the details of the most expensive book
select * from books order by price desc limit 1;
# 7- Show all customers who ordered more than 1 quantity of a book
select * from orders where quantity	> 1;
# 8- Retrieve all orders where the total amount exceeds $20
select * from orders where Total_Amount > 20;
# 9- List all genres available in the Books table
select distinct genre from books;
# 10- Find the book with the lowest stock
select * from books order by stock limit 1;
# 11- Calculate the total revenue generated from all orders
select round(sum(Total_Amount)) as total_revenue from orders;
# 12- Retrieve the total number of books sold for each genre
select b.genre,sum(o.quantity) as Total_Books_Sold from orders o join books b on o.book_id=b.book_id group by genre;
# 13- Find the average price of books in the "Fantasy" genre
select round(avg(price)) as Average_Price from books where genre = "Fantasy";
# 14- List customers who have placed at least 2 orders
select Customer_ID,Count(Quantity) as Order_Count from orders group by customer_ID having count(Order_ID)>=2;
# 15-Find the most frequently ordered book
select book_id, count(order_id) as Most_selling from orders group by Book_ID order by Most_Selling desc limit 1;
select b.book_id,count(o.order_id) as Most_Selling,b.title from orders o join books b on o.book_id= b.book_id 
group by o.book_id order by Most_Selling desc limit 1;
# 16- Show the top 3 most expensive books of 'Fantasy' Genre
select *  from books where genre= "Fantasy" order by price desc limit 3;
# 17- Retrieve the total quantity of books sold by each author
select sum(o.Quantity) as Total_Quantity_Sold, b.author
from orders o join books b on o.book_id = b.book_id group by b.author;
# 18- List the cities where customers who spent over $30 are located
select distinct o.Total_Amount,c.city from orders o join customers c on o.customer_id = c.customer_id
where o.Total_Amount>30;
#19- Find the customer who spent the most on orders
select c.name ,sum(o.Total_Amount) as Most_Spent_Orders from orders o join customers c on o.customer_id = c.Customer_ID
group by c.name order by Most_Spent_Orders desc limit 1;
#20- Calculate the stock remaining after fulfilling all orders
select b.book_id,b.title,b.stock, coalesce(sum(o.quantity),0) as Order_Quantity, b.stock- coalesce(sum(o.quantity),0) as Remaining_Stock
from books b left join orders o on b.book_id = o.book_id group by book_id;
