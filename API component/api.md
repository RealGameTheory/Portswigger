### API Testing Steps

#### API Reconnaissance
**Example API Request:**
GET /api/books HTTP/1.1
Host: example.com

**Techniques:**
* **Fuzzing:** We can use `/api/books/..` to search for more endpoints on the website.
* **Key Checks:**
    * The input data the API processes, including both compulsory and optional parameters.
    * The types of requests the API accepts, including supported HTTP methods and media formats.
    * Rate limits and authentication mechanisms.

---

#### API Documentation
APIs are usually documented so that developers know how to use and integrate with them. This documentation is often publicly available.

**Discovery Paths:**
To discover API documentation, check the following common locations:
* `/api`
* `/swagger/index.html`
* `/openapi.json`

> **Important:** Make sure to check the base path in API paths.

**Additional Discovery:**
* You can also use a list of common paths to find documentation using Intruder.

**Lab: Exploiting an API endpoint using documentation**
<br>
<img src="api_lab1.png" alt="Alt text" width="1000" height="900">
</br>

We can also use OpenAPI Parser BApp to get api documentation

We can use Burp Scanner to crawl the application, then manually investigate interesting attack surface using Burp's browser. Another app like JS Link Finder BApp can be used to find out more hidden api
#### Identifying supported HTTP methods
An API endpoint may support different HTTP methods. It's therefore important to test all potential methods when you're investigating API endpoints. This may enable you to identify additional endpoint functionality, opening up more attack surface.
For example, the endpoint /api/tasks may support the following methods:
* GET /api/tasks - Retrieves a list of tasks.
* POST /api/tasks - Creates a new task.
* DELETE /api/tasks/1 - Deletes a task.


#### Identifying supported content types
API endpoints often expect data in a specific format. To change the content type, modify the Content-Type header, then reformat the request body accordingly. We can use the Content type converter BApp to automatically convert data submitted within requests between XML and JSON.

**Lab: Finding and exploiting an unused API endpoint**
<br>
<img src="api_lab2.png" alt="Alt text" width="1000" height="900">
</br>
After this api patch we have to go to the website and add the item to cart and then buy it.

#### Finding hidden parameters
When we're doing API recon, we may find undocumented parameters that the API supports. We can find these by:
* The Param miner BApp enables us to automatically guess up to 65,536 param names per request. Param miner automatically guesses names that are relevant to the application, based on information taken from the scope.
* Burp Intruder enables us to automatically discover hidden parameters, using a wordlist of common parameter names to replace existing parameters or add new parameters. Make sure we also include names that are relevant to the application, based on our initial recon.
* The Content discovery tool enables us to discover content that isn't linked from visible content that we can browse to, including parameters.
