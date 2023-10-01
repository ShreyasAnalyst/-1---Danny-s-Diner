<h1>#1 - Danny's Diner🍽️</h1>
<img width="500" alt="Coding" src="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/cover%20page.png">
<h1>Contents</h1>
<ul>
  <li><a href="#introduction">Introduction</a></li>
  <li><a href="#problem-statement">Problem Statement</a></li>
  <li><a href="#entity-relationship">Entity Relationship Diagram</a></li>
  <li><a href="#case-study">Case Study Questions & Solutions</a></li>
  <li><a href="#bonus-question">Bonus Questions & Solutions</a></li>
  <li><a href="#insights">Key Insights</a></li>
</ul>

<h1><a id="introduction">Introduction</a></h1>

<p>Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favorite foods: sushi, curry and ramen.</p>
<p>Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.</p>

<h1><a id="problem-statement">Problem Statement</a></h1>
<p>Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favorite. Having this deeper connection with his customers will help him deliver a better and more personalized experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

- sales
- menu
- members

You can inspect the entity relationship diagram and example data below.<p>

<h1><a id="entity-relationship">Entity Relationship Diagram</a></h1>
<img width="500" alt="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/Schema_diagram.png">


<h1><a id="case-study">Case Study Questions & Solutions</a></h1>

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

<li><h5>How many days has each customer visited the restaurant?</h5></li>

```sql
SELECT
	customer_id,
	COUNT(DISTINCT order_date) days_visited
FROM
	sales
GROUP BY 1
ORDER BY 2 DESC;
```
<ul>
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

<ul>
  <li>The query selects data from the <code>"sales"</code> table (aliased as <code>'s'</code>), the <code>"members"</code> table (aliased as <code>'mm'</code>), and the <code>"menu"</code> table (aliased as <code>'m'</code>).</li>
  <li>It uses a <code>JOIN</code> clause to combine data from these three tables, matching records based on the <code>"customer_id"</code> in both the <code>"sales"</code> and <code>"members"</code> tables and the <code>"product_id"</code> in the <code>"menu"</code> table.</li>
  <li>The <code>WHERE</code> clause filters the results to include only records where the join date of the customer is after the order date. This ensures that only purchases made before customers became members are considered.</li>
  <li>The query then calculates two aggregates for each member:
    - <code>COUNT(s.product_id)</code>: This counts the total number of items purchased by each member before becoming a member.
    - <code>SUM(m.price)</code>: This calculates the total amount spent by each member on those items.</li>
  <li>The results are grouped by <code>"customer_id"</code> (<code>GROUP BY 1</code>), ensuring that the aggregates are calculated for each unique customer.</li>
</ul>

<h5><li>If each $1 spendt equates to 10 points and sushi has a 2x points multiplier how many points would each customer have?</li></h5>

```sql
SELECT 
	s.customer_id,
	SUM(
		CASE 
			WHEN product_name = 'shushi' THEN price * 10 * 2
			ELSE price * 10 		
			END
	) points
FROM
	menu m 
		JOIN sales s ON m.product_id = s.product_id
GROUP BY 1
ORDER BY points DESC;
```

<img width="500" alt="Coding" src="">

<li><h5>In the first week after a customer joins the program (including their join date) they earn 2x points on all items, not just sushi how many points do customer A and B have at the end of January?</h5></li>

```sql
SELECT 
    s.customer_id,
    SUM(
        CASE 
            WHEN s.order_date BETWEEN mm.join_date AND (mm.join_date + INTERVAL '6 days') THEN m.price * 10 * 2
            WHEN m.product_name = 'sushi' THEN m.price * 10 * 2
            ELSE m.price * 10
        END
    ) AS points
FROM
    menu m 
    JOIN sales s ON m.product_id = s.product_id
    JOIN members mm ON s.customer_id = mm.customer_id
WHERE
    DATE_TRUNC('month', s.order_date) = DATE '2021-01-01'
GROUP BY s.customer_id
ORDER BY points DESC;
```

<img width="500" alt="Coding" src="">

<ul>
  <li>The query begins by selecting data from the <code>"menu"</code> table (aliased as <code>'m'</code>), the <code>"sales"</code> table (aliased as <code>'s'</code>), and the <code>"members"</code> table (aliased as <code>'mm'</code>).</li>
  <li>It uses <code>JOIN</code> clauses to combine data from these three tables, matching records based on the <code>"product_id"</code> in the <code>"menu"</code> and <code>"sales"</code> tables and the <code>"customer_id"</code> in the <code>"sales"</code> and <code>"members"</code> tables.</li>
  <li>The query calculates the number of points for each customer using a <code>CASE</code> statement:
    - If the <code>"order_date"</code> falls within the first week after a customer joins (including their join date), it applies a 2x points multiplier to all items by multiplying the <code>"price"</code> by 10 and then by 2.
    - If the <code>"product_name"</code> is 'sushi', it also applies a 2x points multiplier.
    - For all other items, it calculates points by multiplying the <code>"price"</code> by 10.</li>
  <li>The <code>SUM()</code> function is used to add up the points for each customer.</li>
  <li>The <code>WHERE</code> clause filters the results to include only records where the <code>"order_date"</code> is in the month of January 2021.</li>
  <li>The result set is grouped by <code>"customer_id"</code> (<code>GROUP BY s.customer_id</code>), ensuring that the total points are calculated for each unique customer.</li>
  <li>The result is ordered in descending order based on the <code>"points"</code> column (<code>ORDER BY points DESC</code>), so customers with the most points appear at the top of the result set.</li>
</ul>

<h1><a id="bonus-question">BONUS QUESTIONS WITH SOLUTIONS</a></h1>

<ol>

<li>Join all the things</li>

```sql
SELECT 
	s.customer_id, 
	s.order_date,
	m.product_name,
	m.price,
	CASE 
		WHEN mm.join_date IS NULL THEN 'N'
		WHEN s.order_date < mm.join_date THEN 'N'
		ELSE 'Y'
		END as member
FROM
	sales s
	JOIN menu m ON s.product_id = m.product_id
	LEFT JOIN members mm ON s.customer_id = mm.customer_id
ORDER BY 1,2,3;
```

<img width="500" alt="Coding" src="">

<ul>
  <li>The query retrieves data from three tables: <code>"sales"</code> (aliased as <code>'s'</code>), <code>"menu"</code> (aliased as <code>'m'</code>), and <code>"members"</code> (aliased as <code>'mm'</code>).</li>
  <li>It joins these tables using different join types:
    - <code>JOIN menu m ON s.product_id = m.product_id</code>: This inner join links the <code>"sales"</code> and <code>"menu"</code> tables based on the <code>"product_id"</code> column to retrieve product-related information for each sale.
    - <code>LEFT JOIN members mm ON s.customer_id = mm.customer_id</code>: This left join connects the <code>"sales"</code> and <code>"members"</code> tables based on the <code>"customer_id"</code> column, allowing us to determine whether a customer is a member.</li>
  <li>The <code>CASE</code> statement is used to determine the membership status ("<code>member</code>" column) for each customer based on the following conditions:
    - If there is no matching join with the "<code>members</code>" table (<code>mm.join_date IS NULL</code>), the customer is marked as 'N' (not a member).
    - If the order date is earlier than the join date (<code>s.order_date < mm.join_date</code>), the customer is also marked as 'N' (not a member).
    - Otherwise, the customer is marked as 'Y' (a member).</li>
  <li>The result set includes columns for "<code>customer_id</code>," "<code>order_date</code>," "<code>product_name</code>," "<code>price</code>," and "<code>member</code>."</li>
  <li>The result is ordered by "<code>customer_id</code>," "<code>order_date</code>," and "<code>product_name</code>" to present the data in a structured manner.</li>
</ul>

<li><h5>Rank all the things</h5></li>

```sql
WITH CTE AS (
	SELECT 
		s.customer_id, 
		s.order_date,
		m.product_name,
		m.price,
		CASE 
			WHEN mm.join_date IS NULL THEN 'N'
			WHEN s.order_date < mm.join_date THEN 'N'
			ELSE 'Y'
			END as member
	FROM
		sales s
		JOIN menu m ON s.product_id = m.product_id
		LEFT JOIN members mm ON s.customer_id = mm.customer_id
	ORDER BY 1,2,3
)
SELECT 
	*,
	CASE 
		WHEN member = 'N' THEN NULL
		ELSE ROW_NUMBER() OVER(PARTITION BY customer_id, member ORDER BY order_date)
		END as ranking
FROM
	CTE;
```

<img width="500" alt="Coding" src="">

<h1><a id="insights">Key Insights</a></h1>
