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
