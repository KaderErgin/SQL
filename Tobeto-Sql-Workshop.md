--1-Hangi ülkeden kaç adet sipariş aldım?

SELECT c.country, COUNT(o.order_id) as total_orders 
FROM customers c
INNER JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.country

--2-Ürün kategorilerinin ortalama ürün fiyatı nedir? 

SELECT category_id,AVG(unit_price) as "Ortalama Ürün Fiyatı" from products
group by category_id

--3-Her müşteri için toplam kaç adet sipariş verilmiş?

SELECT c.contact_name as "Müşteri adı", COUNT(o.order_id) as "verdiği sipariş adeti" 
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.contact_name

--4-Her yıl için kaç adet sipariş aldım? 

SELECT
    EXTRACT(year FROM o.order_date) AS "Yıl",COUNT(*) AS "Sipariş Adeti" FROM orders o  
GROUP BY
    EXTRACT(year FROM o.order_date)

--5-Hangi çalışanım kaç adet sipariş aldı?

SELECT c.employee_id, c.last_name, COUNT(s.order_id) AS order_count
FROM employees c
LEFT JOIN orders s ON c.employee_id = s.employee_id
GROUP BY c.employee_id, c.last_name

--6-Supplier'ları en az sipariş alandan en çok sipariş alana doğru sıralayan query nedir?

SELECT s.contact_name,COUNT(od.quantity) from order_details od
INNER JOIN products p ON  od.product_id = p.product_id
INNER JOIN suppliers s ON p.supplier_id= p.supplier_id 
group by s.contact_name,od.quantity
order by od.quantity asc

--7-Tek bir query içerisinde  sipariş detayı, kullanıcı, çalışan, kargolayacı bilgilerini içeren query'i yazınız.
	
select * from customers c
join orders o on o.customer_id = c.customer_id
join employees e on o.employee_id = e.employee_id
join shippers s on o.ship_via = s.shipper_id
join order_details od on o.order_id = od.order_id


--8 En kârlı satış yapan çalışanım hangisi?

SELECT e.first_name,SUM(od.unit_price*od.quantity) as "En karlı" from order_details od
INNER JOIN orders o ON od.order_id=o.order_id
INNER JOIN employees e ON o.employee_id=e.employee_id
group by e.first_name
order by SUM(od.unit_price*od.quantity) desc
LIMIT 1

--9-En çok sipariş veren (adet) müşterim hangisi?

SELECT customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id
ORDER BY total_orders DESC
LIMIT 1

--10-Siparişleri en geç teslim edilenden en erken teslim edilene doğru sıralayınız (OrderDate ve ShippedDate alanları)?

SELECT order_id,order_date,shipped_date, shipped_date - order_date AS teslim_süresi 
FROM orders
WHERE shipped_date IS NOT NULL
ORDER BY teslim_süresi DESC;



