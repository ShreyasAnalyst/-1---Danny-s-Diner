<h1>#1 - Danny's Diner🍽️</h1>
<img width="500" alt="Coding" src="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/cover%20page.png">
<h1>Contents</h1>
<ul>
  <li><a href="INTRODUCTION">Introduction</a></li>
  <li><a href="PROBLEM STATEMENT">Problem Statement</a></li>
  <li><a href="ENTITY RELATIONSHIP DIAGRAM">Entity Relationship Diagram</a></li>
  <li><a href="CASE STUDY QUESTIONS WITH SOLUTIONS">Case Study Questions & Solutions</a></li>
  <li><a href="BONUS QUESTIONS WITH SOLUTIONS">Bonus Questions & Solutions</a></li>
  <li><a href="INSIGHTS FOUND">Key Insights</a></li>
</ul>

<h1><a id="INTRODUCTION">Introduction</a></h1>
<p>Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favorite foods: sushi, curry and ramen.
  
Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.
</p>

<h1><a id="PROBLEM STATEMENT">Problem Statement</a></h1>
<p>Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favorite. Having this deeper connection with his customers will help him deliver a better and more personalized experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

- sales
- menu
- members

You can inspect the entity relationship diagram and example data below.<p>

<h1><a id="ENTITY RELATIONSHIP DIAGRAM">Problem Statement</a></h1>
<img width="500" alt="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/Schema_diagram.png">


<h1><a id="CASE STUDY QUESTIONS WITH SOLUTIONS">Problem Statement</a></h1>

<ol>
  <li><h5>What is the total amount each customer spent at the restaurant?</h5></li>
	
```sql
SELECT 
	s.customer_id, 
	SUM(m.price) amount_spent
FROM 
	sales s
		JOIN menu m ON s.product_id = m.product_id
GROUP BY 1
ORDER BY 2 DESC;
```

<h6>Solution:</h6>
<img width="500" alt="Coding" src="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/1.png">
<ul>
  <li>The query begins by selecting data from the <code>"sales"</code> table (aliased as <code>'s'</code>) and the <code>"menu"</code> table (aliased as <code>'m'</code>).</li>
  <li>It uses a <code>JOIN</code> clause to combine data from these two tables, matching records based on the <code>"product_id"</code> column in both tables.</li>
  <li>The <code>SUM(m.price)</code> function is used to calculate the total amount spent by each customer. It sums up the <code>"price"</code> column from the <code>"menu"</code> table for each customer's purchases.</li>
  <li>The result set is grouped by the <code>"customer_id"</code> (<code>GROUP BY 1</code>), which means that it calculates the total amount spent for each unique customer.</li>
  <li>The result is then ordered in descending order based on the <code>"amount_spent"</code> column (<code>ORDER BY 2 DESC</code>), so the customers who spent the most appear at the top of the result set.</li>
  <li>The query begins by selecting data from the <code>"sales"</code> table.</li>
  <li>It uses the <code>COUNT(DISTINCT order_date)</code> function to count the number of distinct order dates for each customer. This essentially counts how many different days each customer has visited the restaurant.</li>
  <li>The result set is grouped by the <code>"customer_id"</code> (<code>GROUP BY 1</code>), which means that it calculates the count of distinct order dates for each unique customer.</li>
  <li>The result is then ordered in descending order based on the <code>"days_visited"</code> column (<code>ORDER BY 2 DESC</code>), so customers who have visited the most days appear at the top of the result set.</li>
</ul>

<li><h5>What was the first item from the menu purchased by each customer?</h5></li>

```sql
WITH CTE AS (
  SELECT 
    s.customer_id, 
    s.order_date, 
    m.product_name, 
    ROW_NUMBER() OVER(PARTITION BY s.customer_id ORDER BY order_date) rn 
  FROM 
    sales s 
    JOIN menu m ON s.product_id = m.product_id
) 
SELECT 
  customer_id, 
  product_name 
FROM 
  CTE 
WHERE 
  rn = 1;
```
<img width="500" alt="Coding" src="">

<ul>
  <li>The query utilizes a Common Table Expression (CTE) named <code>"CTE"</code> to simplify the process.</li>
  <li>Within the CTE, it selects data from the <code>"sales"</code> table (aliased as '<code>s</code>') and the <code>"menu"</code> table (aliased as '<code>m</code>') and assigns a row number to each purchase for each customer. The <code>ROW_NUMBER()</code> function is used for this purpose, and it partitions the result set by customer ID and orders the rows by order date.</li>
  <li>The main query selects the <code>"customer_id"</code> and <code>"product_name"</code> columns from the CTE.</li>
  <li>It filters the results to only include rows where the row number (<code>rn</code>) is equal to 1. This ensures that only the first item purchased by each customer is included in the result.</li>
</ul>

<li><h5>What is the most purchased item on the menu and how many times was it purchased by all customers?</h5></li>

```sql
SELECT
	m.product_name,
	COUNT(s.product_id) order_times
FROM 
	sales s
		JOIN menu m ON s.product_id = m.product_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```
<img width="500" alt="Coding" src="">

<ul>
  <li>The query begins by selecting data from the <code>"sales"</code> table (aliased as <code>'s'</code>) and the <code>"menu"</code> table (aliased as <code>'m'</code>).</li>
  <li>It uses a <code>JOIN</code> clause to combine data from these two tables, matching records based on the <code>"product_id"</code> column in both tables.</li>
  <li>The query then groups the results by the <code>"product_name"</code> (<code>GROUP BY 1</code>), which means that it calculates the count of each product's purchases.</li>
  <li>The <code>COUNT(s.product_id)</code> function is used to count the number of times each product was purchased. This effectively calculates how many times each menu item was ordered.</li>
  <li>The result set is ordered in descending order based on the <code>"order_times"</code> column (<code>ORDER BY 2 DESC</code>), so the most purchased item appears at the top of the result set.</li>
  <li>The <code>LIMIT 1</code> clause ensures that only the top result (most purchased item) is returned.</li>
</ul>

<li><h5>Which item was the most popular for each customer?</h5></li>

```sql
WITH CTE AS(
	SELECT
		m.product_name,
		s.customer_id,
		COUNT(s.product_id) order_times,
	ROW_NUMBER() OVER(PARTITION BY s.customer_id ORDER BY COUNT(s.product_id) DESC) rn
	FROM 
		sales s
			JOIN menu m ON s.product_id = m.product_id
	GROUP BY 1,2
)
SELECT 
	customer_id,
	product_name
FROM CTE
WHERE rn = 1;
```

<img width="500" alt="Coding" src="">

<ul>
  <li>The query utilizes a Common Table Expression (CTE) named <code>"CTE"</code> to simplify the process.</li>
  <li>Within the CTE, it selects data from the <code>"sales"</code> table (aliased as <code>'s'</code>) and the <code>"menu"</code> table (aliased as <code>'m'</code>). It calculates the count of each product's purchases for each customer and assigns a row number to each customer's most popular item. The <code>ROW_NUMBER()</code> function is used for this purpose, and it partitions the result set by customer ID and orders the rows by the count of product purchases in descending order.</li>
  <li>The main query selects the <code>"customer_id"</code> and <code>"product_name"</code> columns from the CTE.</li>
  <li>It filters the results to only include rows where the row number (<code>rn</code>) is equal to 1. This ensures that only the most popular item for each customer is included in the result.</li>
</ul>


<li><h5>Which item was purchased first by the customer after they became a member?</h5></li>

```sql
WITH CTE AS (
	SELECT 
		s.customer_id,
		mm.join_date,
		s.order_date,
		m.product_name,
		ROW_NUMBER() OVER(PARTITION BY s.customer_id ORDER BY s.product_id) rn
	FROM 
		sales s 
			JOIN members mm ON s.customer_id= mm.customer_id
			JOIN menu m ON s.product_id = m.product_id
	WHERE
		mm.join_date <= s.order_date
)
SELECT 
	customer_id,
	product_name
FROM CTE
WHERE 
	rn = 1;
```
<img width="500" alt="Coding" src="">

<ul>
  <li>The query utilizes a Common Table Expression (CTE) named <code>"CTE"</code> to simplify the process.</li>
  <li>Within the CTE, it joins data from the <code>"sales"</code> table (aliased as <code>'s'</code>), the <code>"members"</code> table (aliased as <code>'mm'</code>), and the <code>"menu"</code> table (aliased as <code>'m'</code>). It calculates the row number for each product purchase by customers who became members before or on the order date. The <code>ROW_NUMBER()</code> function is used for this purpose and orders the rows by the <code>"product_id."</code></li>
  <li>The <code>WHERE</code> clause ensures that only records where the join date of the customer is on or before the order date are considered.</li>
  <li>The main query selects the <code>"customer_id"</code> and <code>"product_name"</code> columns from the CTE.</li>
  <li>It filters the results to only include rows where the row number (<code>rn</code>) is equal to 1. This ensures that only the first item purchased by each customer after becoming a member is included in the result.</li>
</ul>

<li><h5>Which item was purchased just before the customer became a member?</h5></li>

```sql
WITH CTE AS (
	SELECT 
		s.customer_id,
		mm.join_date,
		s.order_date,
		m.product_name,
		ROW_NUMBER() OVER(PARTITION BY s.customer_id ORDER BY s.product_id DESC) rn
	FROM 
		sales s 
			JOIN members mm ON s.customer_id= mm.customer_id
			JOIN menu m ON s.product_id = m.product_id
	WHERE
		mm.join_date > s.order_date
)
SELECT 
	customer_id,
	product_name
FROM CTE
WHERE 
	rn = 1;
```

<img width="500" alt="Coding" src="">

<ul>
  <li>The query utilizes a Common Table Expression (CTE) named <code>"CTE"</code> to simplify the process.</li>
  <li>Within the CTE, it joins data from the <code>"sales"</code> table (aliased as <code>'s'</code>), the <code>"members"</code> table (aliased as <code>'mm'</code>), and the <code>"menu"</code> table (aliased as <code>'m'</code>). It calculates the row number for each product purchase by customers who became members before or on the order date. The <code>ROW_NUMBER()</code> function is used for this purpose and orders the rows by the <code>"product_id."</code></li>
  <li>The <code>WHERE</code> clause ensures that only records where the join date of the customer is on or before the order date are considered.</li>
  <li>The main query selects the <code>"customer_id"</code> and <code>"product_name"</code> columns from the CTE.</li>
  <li>It filters the results to only include rows where the row number (<code>rn</code>) is equal to 1. This ensures that only the first item purchased by each customer after becoming a member is included in the result.</li>
</ul>

<li><h5>What is the total items and amount spent for each member before they became a member?</h5></li>

```sql
SELECT 
	s.customer_id,
	COUNT(s.product_id),
	SUM(m.price)
FROM
	sales s 
			JOIN members mm ON s.customer_id= mm.customer_id
			JOIN menu m ON s.product_id = m.product_id
WHERE 
	mm.join_date > s.order_date
GROUP BY 1;
```

<img width="500" alt="Coding" src="">


