Download Link: https://assignmentchef.com/product/solved-cs564-project-2-sql
<br>
<h1>DBMS</h1>

Use of postgres is highly recommended. Other DBMS’s may be used for development but the answers must be able to run against postgres server in the lab.

<h1>Part A: Querying the Sales Database [50%]</h1>

The <strong>Sales</strong> database tables are available under “<strong>hw2</strong>” schema on the lab postgres system (under cs564instr database).  You are encouraged to create views in your own schema to make it easier to access the hw2 tables.

[The tables can also be created using the script available in ~shatdal/lab/Sales.sql. If you are using postgres on your personal machine, to load the database, simply type “psql -f Sales.sql” on the command prompt or “i Sales.sql” inside psql. It would create a schema <strong>hw2</strong> in your postgres DBMS.]

The hw2 schema has the following 4 tables. The key of each table is underlined and the foreign keys are also mentioned:

<ul>

 <li><strong>Holidays</strong>(<u>WeekDate</u>, IsHoliday)</li>

 <li><strong>Stores</strong>(<u>Store</u>, Type, Size)</li>

 <li><strong>TemporalData</strong> (<u>Store, WeekDate</u>, Temperature, FuelPrice, CPI, UnemploymentRate) Store is a foreign key referencing Stores (Store).</li>

 <li>WeekDate is a foreign key referencing Holidays (WeekDate).</li>

 <li><strong>Sales</strong>(<u>Store, Dept, WeekDate</u>, WeeklySales)</li>

 <li>Store is a foreign key referencing Stores (Store).</li>

 <li>WeekDate is a foreign key referencing Holidays (WeekDate).</li>

 <li>(Store, WeekDate) is a foreign key referencing TemporalData (Store, WeekDate)</li>

</ul>

Write SQL queries over the given schema that obtain the answers to the following questions:

<ol>

 <li>Which stores had the largest and smallest overall sales during holiday weeks?</li>

 <li>Get the stores at locations where the unemployment rate exceeded 10% at least once but the fuel price never exceeded 4.</li>

 <li>How many non-holiday weeks had larger sales than the overall average sales during holiday weeks?</li>

 <li>Get the total sales per month, and its contribution to total sales (across months) overall for each type of store.</li>

 <li>Which stores have had sales in every department in that store for every month of at least one calendar year among 2010, 2011, and 2012?</li>

 <li>For each of the 4 numeric attributes in TemporalData, are they positively or negatively correlated with sales and how strong is the correlation? Your SQL query should output an instance with the following schema with 4 rows:</li>

</ol>

Output6 (AttributeName VARCHAR(20), CorrSign char(1), CorrValue (float))

e.g., (Temperature, -, -0.5) In your query, the values of AttributeName can be hardcoded string literals, but the other values must be computed automatically using SQL queries over the given database instance.

<ol start="7">

 <li>Which departments contribute to at least 5% of store sales across for at least 3 stores? List the departments and their average contribution to sales across the stores.</li>

 <li>Get the top 10 departments overall ranked by total sales normalized by the size of the store where the sales were recorded.</li>

 <li>For the top 10 departments (in above query, #8), find the 3-monthly moving average and cumulative total of sales.</li>

 <li>The accounting department has asked for the following report. Write a SQL Query that would most closely produce the needed report. Quarters are defined traditionally, with Jan,Feb,March being Q1, etc.</li>

</ol>

<table width="624">

 <tbody>

  <tr>

   <td width="90"><strong>Year</strong></td>

   <td width="35"></td>

   <td width="125"><strong>Quarters</strong></td>

   <td width="125"><strong>Store Type A Sales</strong></td>

   <td width="125"><strong>Store Type B Sales</strong></td>

   <td width="125"><strong>Store Type C Sales</strong></td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2010</td>

   <td width="125">Q1</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2010</td>

   <td width="125">Q2</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2010</td>

   <td width="125">Q3</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2010</td>

   <td width="125">Q4</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2010</td>

   <td width="125">—</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

  <tr>

   <td width="90">…</td>

   <td width="35"></td>

   <td width="125"></td>

   <td width="125"></td>

   <td width="125"></td>

   <td width="125"></td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2012</td>

   <td width="125">Q4</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

  <tr>

   <td width="90"></td>

   <td width="35">2012</td>

   <td width="125">—</td>

   <td width="125">…</td>

   <td width="125">…</td>

   <td width="125">…</td>

  </tr>

 </tbody>

</table>

Note: use of LIMIT N; feature in postgres is not allowed to constrain number of rows returned.

<h1>PART B: Sampling Application [50%]</h1>

Since postgres (and most DBMS’s) don’t allow sampling, the goal is to create a JDBC application that would let user create samples of data in the DBMS. The Sales data from Part A could be used for developing/testing the application. You would use sampling without replacement.

The application should:

<ol>

 <li>The JDBC connection should be to the lab postgres server. Instructions are posted on the webpage as to how to connect to lab postgres server. You may optionally also connect to your own hosted postgres for development/testing. Note that lab postgres can only be connected to from lab linux machines because of security protocols.</li>

 <li>Accept a table name or a query (prompts can distinguish the two if needed)</li>

 <li>Ask for how many sample rows are desired</li>

 <li>Ask if the user wants a table created for the sampled rows (instead of being returned). [These tables could be sampled in next iteration.]</li>

 <li>Fetch/insert exactly that number of random samples from the table or query result</li>

 <li>Allow the user to reset the seed of the random number generator</li>

 <li>Errors may be given for any syntax error in queries or invalid table entries</li>

 <li>If number of samples requested is greater than rows in the table/query, all rows should be returned (or inserted in the sample table) and a message should be given noting that fact.</li>

 <li>Ask if user wants more samples (or quit)</li>

</ol>

The application MAY NOT fetch the entire table or query result and do the sampling work in the application itself. (That is, it is assumed that the original table is too large to fit in memory.)

You may create additional tables (and insert rows) in the database (under your schema).

You may execute any query you like in the database.

Hint: One way to number all rows in a table is to use the “row_number()” ordered-analytic function.

The algorithm for random sampling of N rows (from Knuth: Art of computer programming, volume 2: semi-numerical algorithms) is on the last page.

<h1>Deliverables</h1>

You are required to submit a zipped folder with the following contents:

<ol>

 <li>A “.sql” file per question. Name your files as “query&lt;number&gt;.sql”, e.g., the file “query2.sql” is for question 2 (of part A). These would contain the SQL and the result rows.</li>

 <li>The JDBC app (both java file and executable). Show the result of sampling 10 rows from each of the tables in part A. Additional testing/validation would be done for grade.</li>

 <li>txt with your group members’ name, CS logins and Wisc ids. and describe any assumptions etc made in the application that are not in the documentation of the code.</li>

</ol>


