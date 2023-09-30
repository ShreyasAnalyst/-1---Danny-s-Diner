<h1>#1 - Danny's Diner🍽️</h1>
<img width="500" alt="Coding" src="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/cover%20page.png">
<h1>Contents</h1>
<ul>
  <li><a href="#INTRODUCTION">Introduction</a></li>
  <li><a href="#PROBLEM STATEMENT">Problem Statement</a></li>
  <li><a href="#ENTITY RELATIONSHIP DIAGRAM">Entity Relationship Diagram</a></li>
  <li><a href="#CASE STUDY QUESTIONS WITH SOLUTIONS">Case Study Questions & Solutions</a></li>
  <li><a href="#BONUS QUESTIONS WITH SOLUTIONS">Bonus Questions & Solutions</a></li>
  <li><a href="#INSIGHTS FOUND">Key Insights</a></li>
</ul>

<h1><a name="INTRODUCTION">Introduction</a></h1>
<p>Danny seriously loves Japanese food so in the beginning of 2021, he decides to embark upon a risky venture and opens up a cute little restaurant that sells his 3 favorite foods: sushi, curry and ramen.
  
Danny’s Diner is in need of your assistance to help the restaurant stay afloat - the restaurant has captured some very basic data from their few months of operation but have no idea how to use their data to help them run the business.
</p>

<h1><a name="PROBLEM STATEMENT">Problem Statement</a></h1>
<p>Danny wants to use the data to answer a few simple questions about his customers, especially about their visiting patterns, how much money they’ve spent and also which menu items are their favorite. Having this deeper connection with his customers will help him deliver a better and more personalized experience for his loyal customers.

He plans on using these insights to help him decide whether he should expand the existing customer loyalty program - additionally he needs help to generate some basic datasets so his team can easily inspect the data without needing to use SQL.

Danny has provided you with a sample of his overall customer data due to privacy issues - but he hopes that these examples are enough for you to write fully functioning SQL queries to help him answer his questions!

Danny has shared with you 3 key datasets for this case study:

- sales
- menu
- members

You can inspect the entity relationship diagram and example data below.<p>

<h1><a name="ENTITY RELATIONSHIP DIAGRAM">Problem Statement</a></h1>
<img width="500" alt="https://github.com/ShreyasAnalyst/-1---Danny-s-Diner/blob/main/images/Schema_diagram.png">


<h1><a name="CASE STUDY QUESTIONS WITH SOLUTIONS">Problem Statement</a></h1>

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
</ul>



