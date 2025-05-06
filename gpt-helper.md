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
 GET /proxy?type=query&entity=JobSubmission
 ```
 _Defaults to `status!='Deleted'`_

 ## 🛡️ Smart Defaults

 The following entities include default fields and filters automatically:

 | Entity         | Default `fields`                                | Default `where`                          |
 |----------------|-------------------------------------------------|------------------------------------------|
 | Skill          | `id,name,enabled`                               | `enabled=true`                           |
 | Candidate      | `id,firstName,lastName,email`                  | `isDeleted=false`                        |
 | JobOrder       | `id,title,clientCorporation`                   | `isDeleted=false AND isOpen=true`        |
 | ClientContact  | `id,firstName,lastName,email`                  | `isDeleted=false`                        |
 | JobSubmission  | `id,candidate,jobOrder,status,sendoutDate`     | `status!='Deleted'`                      |

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
     },
     ...
   ]
 }
 ```

 ## 🔄 Caching

 - `meta` results are cached in memory for 15 minutes per entity.
 - This reduces redundant API calls and improves response speed.

 ## 🚀 Best Practices

 - Let GPT use the proxy without requiring Bullhorn-specific knowledge.
 - Use `meta` to inspect available fields for custom entities.
 - Keep prompts natural; GPT will handle the technical details.

 ---
 _Nimbl-Recruitment.com 2025 all rights reserved_