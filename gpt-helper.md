 # NimBull GPT Proxy Helper
 
 This guide explains how to use the `/proxy` endpoint to interact with the Bullhorn API via the NimBull GPT proxy.
 
 ## 🧠 Supported Operations
 
 Use the `type` query parameter to specify the Bullhorn operation type:
 
 | Type   | Description                                  |
 |--------|----------------------------------------------|
 | query  | Filtered entity queries using `where`        |
 | search | Full-text search using `query`               |
 | entity | Fetch a single record by ID                  |
 | meta   | Get metadata for a Bullhorn entity           |
 
 Both GET and POST methods are supported. Use POST for large or complex queries.
 
 ## 💡 Example Calls
 
 ### Get enabled skills
 ```http
 GET /proxy?type=query&entity=Skill
 ```
 _Defaults to `where=enabled=true` and `fields=id,name,enabled`_
 
 ### Search for candidates by keyword
 ```http
 GET /proxy?type=search&entity=Candidate&query=developer
 ```
 
 ### Get a specific job order
 ```http
 GET /proxy?type=entity&entity=JobOrder&id=12345
 ```
 
 ### Get metadata for an entity
 ```http
 GET /proxy?type=meta&entity=Candidate
 ```
 
 ### Count submitted CVs
 ```http
 GET /proxy?type=query&entity=JobSubmission&where=jobOrder.id=12345
 ```
 _Get submissions for a specific job order_
 
 ```http
 GET /proxy?type=query&entity=JobSubmission&where=jobOrder.id=12345 AND status!='Deleted'
 ```
 _With status filter to exclude deleted submissions_
 
 ### Use POST to bypass URL limits
 ```http
 POST /proxy
 Content-Type: application/json
 {
   "type": "query",
   "entity": "Candidate",
   "where": "isDeleted=false AND firstName='John'",
   "fields": "id,firstName,lastName,email"
 }
 ```
 
 ## ⚡ Optimized Count Queries
 
 If you're only interested in how many records exist (e.g. number of JobSubmissions), the proxy will automatically optimize the request when using:
 
 ```http
 GET /proxy?type=query&entity=JobSubmission&fields=id&count=1&start=0
 ```
 
 Instead of fetching data, this returns a response like:
 
 ```json
 {
   "total": 2431
 }
 ```
 
 This works for all entities and avoids slow pagination.
 
 ## 🛡️ Smart Defaults
 
 The following entities include default fields and filters automatically:
 
 | Entity         | Default `fields`                                | Default `where`                          |
 |----------------|-------------------------------------------------|------------------------------------------|
 | Skill          | `id,name,enabled`                               | `enabled=true`                           |
 | Candidate      | `id,firstName,lastName,email`                  | `isDeleted=false`                        |
 | JobOrder       | `id,title,clientCorporation`                   | `isDeleted=false AND isOpen=true`        |
 | ClientContact  | `id,firstName,lastName,email`                  | `isDeleted=false`                        |
 | JobSubmission  | `id,candidate,jobOrder,status,dateAdded`       | `isDeleted=false`                        |
 
 ## 📦 Response Format
 
 All responses follow this format:
 ```json
 {
   "data": [
     { ... },
     { ... }
   ]
 }
 ```
 
 For `meta` requests, you'll receive field information:
 ```json
 {
   "fields": [
     {
       "name": "firstName",
       "type": "String",
       "required": false,
       "searchable": true
     }
   ]
 }
 ```
 
 ## 🔄 Caching
 
 - `meta` results are cached in memory for 15 minutes per entity.
 - Recent query results are cached in memory for 10 minutes to improve performance.
 - This reduces redundant API calls and improves response speed.
 
 ## 🚀 Best Practices
 
 - Be specific with `JobSubmission` queries by using a `where` clause like `jobOrder.id=12345` to avoid slow full-dataset scans.
 - For fast record counts, use `fields=id&count=1&start=0`. The proxy will return only the `total` value without fetching records.
 - Recent query results may be cached in memory for up to 10 minutes to reduce load and improve speed.
 - Let GPT use the proxy without requiring Bullhorn-specific knowledge.
 - Use `meta` to inspect available fields for custom entities.
 - Prefer POST when dealing with complex queries or large parameter sets.
 - Keep prompts natural; GPT will handle the technical details.
 
 ---
 _Nimbl-Recruitment.com 2025 all rights reserved_