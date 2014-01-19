/*CREATE DATABASE Library
USE Library

CREATE TABLE book (
	book_id int IDENTITY (1,1), 
	title varchar(50) NOT NULL, 
	price decimal(9,2) NOT NULL, 
	year_published int NOT NULL, 
	ISBN varchar(20) NOT NULL, 
	CONSTRAINT PK_book PRIMARY KEY (book_id) 
)
CREATE TABLE autors (
	autor_id int IDENTITY (1,1), 
	autor_fname varchar(20) NOT NULL, 
	autor_lname varchar(30) NOT NULL, 
	CONSTRAINT PK_autor PRIMARY KEY (autor_id) 
)
CREATE TABLE customer (
	customer_egn int NOT NULL, 
	customer_fname varchar(20) NOT NULL, 
	customer_lname varchar(30) NOT NULL, 
	phone varchar (20) NULL, 
	customer_adress varchar(100) NOT NULL, 
	CONSTRAINT PK_customer PRIMARY KEY (customer_egn) 
)

CREATE TABLE book_autors (
	book_id int, 
	autor_id int 
) 

ALTER TABLE book_autors 
	ADD CONSTRAINT FK_ba_book FOREIGN KEY (book_id) REFERENCES book (book_id) 

ALTER TABLE book_autors 
	ADD CONSTRAINT FK_ba_autors FOREIGN KEY (autor_id) REFERENCES autors (autor_id)



CREATE TABLE book_customer (
	book_id int NOT NULL, 
	customer_egn int NOT NULL 
) 

ALTER TABLE book_customer 
	ADD CONSTRAINT FK_bc_book FOREIGN KEY (book_id) REFERENCES book (book_id) 

ALTER TABLE book_customer 
	ADD CONSTRAINT FK_bc_customer FOREIGN KEY (customer_egn) REFERENCES customer (customer_egn)


SET IDENTITY_INSERT book on

INSERT INTO book (book_id, title, price, year_published, ISBN)
	VALUES (1, 'Научете сами SQL', 20, 2000, '123A-3')
INSERT INTO book (book_id, title, price, year_published, ISBN)
	VALUES (2, 'Java', 25, 2010, '254A-6')
INSERT INTO book (book_id, title, price, year_published, ISBN)
	VALUES (3, 'Macromedia Flash', 22, 2011, '654K-4')
	


INSERT INTO autors (autor_fname, autor_lname)
	VALUES ('Иван', 'Иванов')
INSERT INTO autors (autor_fname, autor_lname)
	VALUES ('Петър', 'Петров')
INSERT INTO autors (autor_fname, autor_lname)
	VALUES ('Стоян', 'Стоянов')



INSERT INTO customer (customer_egn, customer_fname, customer_lname, phone, customer_adress)
	VALUES (881212445, 'Ася', 'Тоскова', '0888 324 456', 'Пловдив, "Бугариево" 31')
INSERT INTO customer (customer_egn, customer_fname, customer_lname, phone, customer_adress)
	VALUES (901114658, 'Христо', 'Петров', '0889 273 406', 'Пловдив, "Марица" 22')
INSERT INTO customer (customer_egn, customer_fname, customer_lname, phone, customer_adress)
	VALUES (661204475, 'Иван', 'Петров', '0888 649 204', 'София, "Круша" 31')



INSERT INTO book_autors (book_id, autor_id)
	VALUES (1, 1)
INSERT INTO book_autors (book_id, autor_id)
	VALUES (1, 2)
INSERT INTO book_autors (book_id, autor_id)
	VALUES (2, 1)
INSERT INTO book_autors (book_id, autor_id)
	VALUES (3, 2)
INSERT INTO book_autors (book_id, autor_id)
	VALUES (3, 3)



INSERT INTO book_customer (book_id, customer_egn)
	VALUES (1, 661204475)
INSERT INTO book_customer (book_id, customer_egn)
	VALUES (2, 661204475)
INSERT INTO book_customer (book_id, customer_egn)
	VALUES (3, 901114658)
INSERT INTO book_customer (book_id, customer_egn)
	VALUES (3, 881212445)


UPDATE autors
	SET autor_fname = 'Ивайло', 
		autor_lname = 'Илиев'
	WHERE autor_id = 1
	

UPDATE book_customer
	SET book_id = 3 
	WHERE customer_egn = 661204475 AND book_id = 1 


--DELETE FROM book
--WHERE book_id = 1

SELECT title, price FROM book
SELECT * FROM book ORDER BY price DESC
SELECT MAX(price) FROM book 
SELECT SUM (price) FROM book
SELECT ISBN, title FROM book WHERE price < 24
SELECT * FROM book WHERE price BETWEEN 20 AND 50
SELECT title FROM book WHERE year_published IN ('1999', '2010', '2013')
SELECT * FROM customer WHERE phone LIKE'%3%'
SELECT customer_egn FROM book_customer WHERE book_id IS NOT NULL
SELECT * FROM book
SELECT * FROM book WHERE title LIKE 'Научете%' AND price BETWEEN 20 AND 50
SELECT * FROM book ORDER BY title ASC, year_published ASC
SELECT * FROM book ORDER BY price DESC
SELECT SUM (price) FROM book WHERE year_published >'1998'
SELECT * FROM autors WHERE autor_fname = 'Иван'
SELECT * FROM autors WHERE autor_fname LIKE '%f'
SELECT * FROM customer WHERE phone IS NUll 
SELECT customer_fname, customer_lname FROM customer WHERE phone IS NUll ORDER BY customer_fname ASC
SELECT title FROM book WHERE price BETWEEN 20 AND 50
SELECT title FROM book WHERE title LIKE 'на%' AND price BETWEEN 20 AND 50

SELECT * FROM book
SELECT SUM (price) FROM book GROUP BY year_published
SELECT COUNT (customer_fname) FROM customer where customer_fname = 'Ivan'
SELECT sum (price) FROM book where year_published > '2001' group by year_published

SELECT year_published, sum (price) as Suma
FROM book 
where year_published > '2001' 
group by year_published
having sum (price) > 20
order by year_published ASC



SELECT min (price)
FROM book 
having min (price) < 23



SELECT year_published, count (book_id) as broi
FROM book 
group by year_published
having count (book_id) > 1 --poveche ot edna izdadena kniga na godina




INSERT INTO autors 
	VALUES ('Misho', 'Petrov')

INSERT INTO book 
	VALUES ('Macromedia Flash2', 15, 2014, '677K-4')


select title, autor_fname, autor_fname
from book_autors 
Join book on book_autors.book_id = book.book_id
Join autors on book_autors.autor_id = autors.autor_id

-- tova e ednakvo sas sledvastoto



select book_id, autor_fname, autor_fname
from book_autors, autors 
where book_autors.autor_id = autors.autor_id


select customer_fname, customer_lname, book_id
from customer, book_customer 
where customer.customer_egn = book_customer.customer_egn


--1.
select autor_fname, autor_lname
from book_autors 
Join autors on book_autors.autor_id = autors.autor_id
where book_autors.book_id = 1

--2.
select *
from book_customer Join customer on book_customer.customer_egn = customer.customer_egn
				   Join book on book_customer.book_id = book.book_id
				where book.title = 'Научете сами SQL'


--3.
select customer.*, book.*
from book_customer Join customer on book_customer.customer_egn = customer.customer_egn
				   Join book on book_customer.book_id = book.book_id

			
--4.
select customer.customer_fname, customer.customer_lname, book.*
from book_customer Join customer on book_customer.customer_egn = customer.customer_egn
				   Join book on book_customer.book_id = book.book_id
				   where customer.customer_fname = 'Иван' AND customer.customer_lname = 'Петров'
					
--5.
select a.autor_fname, a.autor_lname, b.title
from  book_autors ba Right Join  autors a on ba.autor_id = a.autor_id 
				  Left Join book b on ba.book_id = b.book_id

--6.
select customer.*, book.*
from book_customer Join customer on book_customer.customer_egn = customer.customer_egn
				   Join book on book_customer.book_id = book.book_id
				   ORDER by customer.customer_fname
				  



select *
from book 
where price = (select Max(price) from book)



--8.
select distinct autors.autor_fname, autors.autor_lname, book.price
from  book_autors Full Join autors on book_autors.autor_id = autors.autor_id 
				  Full Join book on book_autors.book_id = book.book_id
where price = (select Max(price) from book) or price = (select Min(price) from book)


ALTER TABLE book_customer
ADD get_data varchar(10), return_data varchar(10) 



--10.
create view book_A 
as
select autors.autor_fname, autors.autor_lname, book.title
from  book_autors Full Join autors on book_autors.autor_id = autors.autor_id 
				  Full Join book on book_autors.book_id = book.book_id


select * from book_A 


create view client_B 
as
select customer.customer_fname, customer.customer_lname, book.title
from  book_customer Full Join customer on book_customer.customer_egn = customer.customer_egn 
				  Full Join book on book_customer.book_id = book.book_id

select * from client_B


ALTER view client_B 
as
select customer.customer_fname + '' + customer.customer_lname as customer, book.title, book_customer.get_data
from  book_customer Full Join customer on book_customer.customer_egn = customer.customer_egn 
				  Full Join book on book_customer.book_id = book.book_id


--12.
create view c_broi 
as
select customer.customer_fname + '' + customer.customer_lname as customer
from  book_customer Full Join customer on book_customer.customer_egn = customer.customer_egn 
				  Full Join book on book_customer.book_id = book.book_id
				  where book.book_id =  (Select COUNT (book.book_id) from book)
	
ALTER view c_broi 
as
select customer.customer_fname + ' ' + customer.customer_lname as customer, COUNT (book_customer.book_id) as broi
from  book_customer Full Join customer on book_customer.customer_egn = customer.customer_egn 
group by customer.customer_fname, customer.customer_lname				  
		

--13.
create view c_notreturn 
as
select customer.customer_fname + '' + customer.customer_lname as customer, book.title
from  book_customer Full Join customer on book_customer.customer_egn = customer.customer_egn 
				  Full Join book on book_customer.book_id = book.book_id
				  where book_customer.return_data is NULL 
				
--13a.= 13.				  
create view c_notreturn 
as
select customer.customer_fname + '' + customer.customer_lname as customer, book.title
from  book_customer, customer, book
where book_customer.customer_egn = customer.customer_egn 
and book on book_customer.book_id = book.book_id
and book_customer.return_data is NULL   	 
				  	 
*/
