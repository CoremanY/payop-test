* [Back to contents](../Readme.md#contents)

# Responses  

## **✅ Success Responses**

**Successful responses follow a consistent structure and return an HTTP status code of <code>200</code> or <code>201</code>. The response body always contains a <code>data</code> field with the relevant payload and a <code>status</code> key used internally by Payop.**


### **200 OK**

> **The request was successfully processed.**

**Example:**


```json

{
 "data": {
   "id": "423131",
   "status": 1,
   "dateTime": {
     "createdAt": 1566543694,
     "updatedAt": null
   }
 },
 "status": 1
}

```


** **


### **201 Created**
> **The resource was successfully created (e.g., token, invoice, transaction).**


**Example:**


```json

{
 "data": {
   "token": "HR5qDwg9B09dJSr5SjJ/u6oVBcq6TkOjnAUR0875IcYO8nQUxRSO3KpDVN",
   "expired_at": 1644301111
 },
 "status": 1
}

```


** **


### **🔑 Success Response Fields**


<table>
  <tr>
   <td><strong>Field</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>data</code></strong>
   </td>
   <td><strong>Object/String</strong>
   </td>
   <td><strong>The returned object, array, or message string (string as <code>data</code> is a known bug).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>status</code></strong>
   </td>
   <td><strong>Number</strong>
   </td>
   <td><strong>Internal flag (<code>1</code> = success) — not used by clients.</strong>
   </td>
  </tr>
</table>



## **❌ Error Responses**

**Error responses follow REST conventions and return standard HTTP status codes such as <code>401</code>, <code>403</code>, <code>404</code>, <code>422</code>, and <code>500</code>. The response body always includes a <code>message</code> field that describes the issue.**


### **401 Unauthorized**


> **Authentication token is missing, expired, or invalid.**


```json
{ 
"message": "Full authentication is required to access this resource." 
}

```


** **


### **403 Forbidden**


> **The authenticated user does not have permission to access the requested resource.**


```json
{   "message": "Access denied." }
```


** **


### **404 Not Found**


> **The requested resource could not be found (e.g., invoice or transaction ID is incorrect).**


```json
{   "message": "Invoice not found" }
```


** **


### **422 Unprocessable Entity**


>  **The request was understood, but it failed validation or a required feature is not enabled.**


#### **🔸 Case: Payment method not enabled**


```json
{   "message": "Method must be enabled to use it" }
```



#### **🔸 Case: Validation error**


```json
{
 "message": {
   "email": [
     "This value should not be blank."
   ],
   "password": [
     "This value should not be blank."
   ]
 }
}

```



### **500 Internal Server Error**


> **A generic error occurred on the server side.**


```json

{   "message": "Something went wrong, try again or contact support." }

```


** **


### **📝 Handling Recommendations**


<table>
  <tr>
   <td><strong>Code</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Recommendation</strong>
   </td>
  </tr>
  <tr>
   <td><strong>401</strong>
   </td>
   <td><strong>Unauthorized</strong>
   </td>
   <td><strong>Check Bearer token and retry</strong>
   </td>
  </tr>
  <tr>
   <td><strong>403</strong>
   </td>
   <td><strong>Forbidden</strong>
   </td>
   <td><strong>Verify API permissions or access scope</strong>
   </td>
  </tr>
  <tr>
   <td><strong>404</strong>
   </td>
   <td><strong>Not Found</strong>
   </td>
   <td><strong>Check if resource ID is correct</strong>
   </td>
  </tr>
  <tr>
   <td><strong>422</strong>
   </td>
   <td><strong>Validation or unsupported method</strong>
   </td>
   <td><strong>Validate input or enable missing method</strong>
   </td>
  </tr>
  <tr>
   <td><strong>500</strong>
   </td>
   <td><strong>Internal error</strong>
   </td>
   <td><strong>Retry or contact support if persistent</strong>
   </td>
  </tr>
</table>
