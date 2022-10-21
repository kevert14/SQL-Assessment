# SQL-Assessment 

Below are my answers to each of the SQL Assessment:

*  Question #1
 Generate a query to get the sum of the clicks of the marketing data

    SELECT sum(clicks) AS sum_clicks FROM marketing_data

*  Question #2
 Generate a query to gather the sum of revenue by store_location from the store_revenue table

    SELECT store_location, sum(revenue) AS sum_revenue FROM store_revenue
    GROUP BY store_location

*  Question #3
 Merge these two datasets so we can see impressions, clicks, and revenue together by date
and geo.
 Please ensure all records from each table are accounted for.

    SELECT b.date, b.geo, b.clicks, b.impressions, a.revenue FROM 
        ( SELECT date, RIGHT(store_location, 2) AS store_state, GROUP_CONCAT(brand_id SEPARATOR ',') AS brand_ids, SUM(revenue) AS revenue
        FROM store_revenue
        GROUP BY store_state, date) AS a

    JOIN 
        ( SELECT geo, clicks, impressions, date
        FROM marketing_data 
        WHERE geo = 'CA' OR geo = 'TX' OR geo = 'NY') AS b

    ON a.store_state = b.geo AND a.date = b.date
    ORDER BY  b.date, b.geo

* Question #4
 In your opinion, what is the most efficient store and why?

    The CA store is the most effecient. I calculated each locations click to revenue ratio (CA: 6524.71, NY: 1365.74, TX: 329.53) and impressions to revenue   ratio (CA: 45.03, NY: 11.05, TX: 2.6). For every 1 click, the CA store is averaging $6524.71 in revenue. For every 1 impression, CA is averaging $45.03     in revenue. Clicks and impressions are the work, or effort, going in to each store to improve revenue, this is why I calculated these ratios.   

* Question #5 (Challenge)
 Generate a query to rank in order the top 10 revenue producing states

    SELECT store_location, revenue
    FROM (SELECT store_location,revenue,
    DENSE_RANK() OVER (ORDER BY revenue DESC) 
    FROM store_revenue) AS new_table
    LIMIT 10;
