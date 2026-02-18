Error Analysis: Slow Database Query
Error Description:
The query to fetch customer order details takes 8–10 seconds for customers with many orders. This delay causes timeout issues in the web application. The slowness comes from complex subqueries (json_agg with joins) and lack of supporting indexes.
Root Cause:
- Unindexed columns: Filtering by customer_id and order_date without indexes forces full table scans.
- Nested subqueries: Each order triggers separate queries for order_items and order_status_history, multiplying work.
- Large dataset: With hundreds of thousands of rows, inefficient joins and aggregations become costly.
- Serialization overhead: Building JSON objects inside the query adds processing time.
Solution:
- Add indexes:
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_status_order_id ON order_status_history(order_id);
- Refactor query:
- Use JOIN LATERAL instead of correlated subqueries for items and status_history.
- Example:
SELECT o.order_id, o.order_date, ...
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
LEFT JOIN addresses a ON o.shipping_address_id = a.address_id
LEFT JOIN LATERAL (
  SELECT json_agg(...) AS items
  FROM order_items oi
  JOIN products p ON oi.product_id = p.product_id
  WHERE oi.order_id = o.order_id
) items ON true
LEFT JOIN LATERAL (
  SELECT json_agg(...) AS status_history
  FROM order_status_history s
  WHERE s.order_id = o.order_id
  ORDER BY s.status_date DESC
) status_history ON true
WHERE o.customer_id = $1
  AND o.order_date BETWEEN $2 AND $3
ORDER BY o.order_date DESC;
- Pagination: Return results in chunks (e.g., 50 orders at a time) instead of all at once.
- Caching: Cache frequent queries (e.g., recent orders) in Redis.
- Profile query: Use EXPLAIN ANALYZE to confirm improvements.

Learning Points
- Indexes matter: Always index columns used in filters and joins.
- Avoid correlated subqueries: They scale poorly with large datasets.
- Use EXPLAIN ANALYZE: It reveals which parts of the query are slow.
- Paginate results: Don’t fetch thousands of rows in one request.
- Cache wisely: Reduce database load for common queries.

Reflection
- Optimization impact: Adding indexes and refactoring subqueries can reduce query time from ~10s to under 1s.
- Understanding gained: Learned how PostgreSQL query planner works and why correlated subqueries are expensive.
- Future approach: Always start with EXPLAIN ANALYZE to identify bottlenecks before rewriting queries.
- Tools to use: PostgreSQL’s pg_stat_statements, query planner visualization tools, and application-level profiling.

Would you like me to also sketch out a before vs. after performance comparison table (e.g., query time, rows scanned, indexes used) so you can clearly show the optimization results in your documentation?
