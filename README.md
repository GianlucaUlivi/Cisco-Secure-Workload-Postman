# Cisco Secure Workload Postman
Postman collection for interacting with the Cisco Secure Workload API (formerly know as Tetration)  
The official Cisco Postman collection [CiscoSE/tetration-postman](https://github.com/CiscoSE/tetration-postman) has not been updated in multiple years.  
This collection fixes some issues in the Pre-request authentication scripts (fix also pending pull request from [GianlucaUlivi/tetration-postman](https://github.com/GianlucaUlivi/tetration-postman) into the official collection) and adds more API calls examples.  

## Using Postman with the Cisco Secure Workload REST API

The Cisco Secure Workload REST API uses a highly secure digest authentication based on a Public and Private API keys that help generate unique headers for every API request.  
The digest is calculated leveraging the Private API key, a timestamp, and the message body, this means that the Private Key is never sent in the actual request.  
A single authorization header can both verify identity and integrity of the message, and the authorization code will never be re-used.  
Thus, even if the message is sent in the clear and intercepted by a man-in-the-middle, they would not be able to access the API.  
The generation of these custom headers is built into the Python library, but if we want to use Postman, we have to add a custom Pre-request script to our requests.
 
Specifically, the headers are as follows:
* **'Id'**: Your Tetration Public API key
* **'Timestamp'**: A UTC Timestamp in a specific format
* **'Content-type'**: 'application/json'
* **'X-Tetration-Cksum'**: This is only used in POST and PUT requests and is a hash of the body being posted with SHA256 and encoded in Hex.  This ensures that the body is not tampered with in-flight.  Will be unique to any specific post or put body.
* **'Authorization'**: An HMAC-SHA256 digest with the request method, URL, X-Tetraiton-Cksum header, Content-type header, Timestamp header and the API Secret Key.  Used in all requests and will be unique to EVERY request. [Hash-based Message Authentication Code (Wikipedia)](https://en.wikipedia.org/wiki/Hash-based_message_authentication_code)
 
To set up Postman, you will need to create a Tetration Environment with the following variables:
* **'API_KEY'**: Your API Key, visible under your profile > API Keys
* **'API_SECRET'**: Your API Secret, visible under your profile > API Keys only when creating the new API Key
* **'URL'**: Your Tetration Cloud URL (eg. example.tetrationcloud.com)
 

## Importing the Collection:

`Cisco Secure Workload GianlucaUlivi Example.postman_collection.json` includes the Pre-request authentication script fixes and the new example of the API Calls.  
`Example Environment.postman_environment.json` can be imported as an Environment Example, you can also manually recreated the variables into the Global Environment if needed (for multi-tenant it is suggested to move the 'URL' variable to a specific environment.  
`Cisco Secure Workload 3.5 - OpenAPI.postman_collection.json` is a copy of the official Cisco collection, it includes requests for the "Applications and Security Policies" related API calls.
