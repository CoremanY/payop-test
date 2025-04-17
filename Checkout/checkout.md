* [Back to contents](../Readme.md#contents)

# Checkout

### **Bearer Authentication**


#### **Purpose:**

* **This document explains how authentication works in the Payop API using JWT-based Bearer Authentication.**
* **Authentication is required for accessing protected API endpoints.**

#### **How It Works:**

* **Clients must send a JWT token in the <code>Authorization</code> header with each API request.**
* **Example of required headers:**


```
Content-Type: application/json  
Authorization: Bearer YOUR_JWT_TOKEN
```

* <strong>Tokens can be generated in the Payop dashboard and should be securely stored.</strong> 

**📺 Watch the video "How to create a JWT token?":**  

[![Watch the video "How to create a JWT token?](https://img.youtube.com/vi/Q7vlfEpJMRA/0.jpg)](https://www.youtube.com/watch?v=Q7vlfEpJMRA)

<a href="https://www.youtube.com/watch?v=Q7vlfEpJMRA" target="_top">
  <img src="https://img.youtube.com/vi/Q7vlfEpJMRA/0.jpg" alt='Watch the video "How to create a JWT token?"' />
</a>

> **Deprecated: Using tokens in the header directly is discouraged, and this method may be removed in future API versions.**


---


## **Checkout Section**


### **Check invoice status**


#### **Purpose:**

* **This endpoint allows users to check the current status of an invoice after initiating a payment.**
* **Useful for tracking payments and determining if further action is required.**

#### **How It Works:**


* **A <code>GET</code> request is sent with the <code>invoiceID</code> parameter.**
* **The response contains one of the following statuses:**
    * **Pending: The transaction is still being processed.**
    * **Success: The transaction is completed successfully.**
    * **Fail: The payment attempt failed.**


#### **Example Usage:**

**Request:**


```shell
GET https://api.payop.com/v1/checkout/check-invoice-status/{invoiceID}
```


** **

**Response Examples:**

**Pending (Waiting for further action):**


```shell
{
 "data": {
   "isSuccess": true,
   "status": "pending",
   "url": ""
 },
 "status": 1
}
```


** **

**Redirect  (POST request required):**


```shell
{
 "data": {
   "isSuccess": true,
   "status": "pending",
   "form": {
     "method": "POST",
     "url": "https://acs.anybank.com/",
     "fields": {
       "PaReq": "encrypted_data",
       "MD": "unique_id",
       "TermUrl": "https://payop.com/v1/url"
     }
   }
 },

 "status": 1
}
```


** **


### **Create Checkout**


#### **Purpose:**



* **This endpoint creates a checkout transaction linked to an invoice.**
* **Required when a customer wants to make a payment.**


#### **How It Works:**



* **A <code>POST</code> request is sent with the invoice details, payment method, and customer information.**
* **The response returns a unique transaction ID.**


#### **Example Usage:**

**Request:**


```shell
POST https://api.payop.com/v1/checkout/create
```


** **

**Key Parameters:**

* **<code>invoiceIdentifier</code>: The invoice ID.**
* **<code>customer</code>: Customer details (name, email, IP address).**
* **<code>paymentMethod</code>: The payment method ID.**
* **<code>checkStatusUrl</code>: The URL to check the payment status.**

**Response Example (Success):**


```shell
{
 "data": {
   "isSuccess": true,
   "message": "",
   "txid": "transaction_unique_id"
 },
 "status": 1
}
```


---


### **Get Transaction**


#### **Purpose:**



* **Retrieves detailed information about a specific transaction.**
* **Can be used for monitoring payments and identifying errors.**


#### **How It Works:**



* **A <code>GET</code> request is sent with the transaction ID.**
* **The response includes transaction status, amount, error messages (if applicable), and payment method details.**


#### **Example Usage:**

**Request:**


```shell
GET https://api.payop.com/v2/transactions/{transactionID}
```



### **Headers:**


```shell
Content-Type: application/
Authorization: Bearer YOUR_JWT_TOKEN
```


**Response Example (Success):**


```shell
{
 "data": {
   "identifier": "transaction_id",
   "amount": 100,
   "currency": "USD",
   "state": 5,
   "error": "error message",
   "createdAt": 1567402240,
   "orderId": "134666",
   "resultUrl": "https://your.site/result"
 }

}
```

---


### **IPN**


#### **Purpose:**

* **Describes how Instant Payment Notification (IPN) works.**
* **IPNs inform merchants in real-time about payment updates.**


#### **How It Works:**

* **Once a transaction is completed, Payop sends an IPN request to the merchant’s pre-configured IPN URL.**
* **Merchants should validate the IPN and update their order statuses accordingly.**
* **IPNs may be sent multiple times until a confirmation (HTTP 200) is received.**


#### **Example Usage:**

**IPN Request Format:**


```shell
{
 "invoice": {
   "id": "invoice_id",
   "status": 1,
   "txid": "transaction_id"

 },

 "transaction": {
   "id": "transaction_id",
   "state": 2,
   "order": { "id": "ORDER_ID" },
   "error": {
     "message": "Error message",
     "code": ""
   }
 }
}
```



#### **Handling IPNs:**



* **Validate the IPN request source (only accept from Payop IPs).**
* **Ensure transactions are updated based on received status.**
* **Prevent duplicate processing if the same IPN is received multiple times.**


---


### **Void Transaction**


#### **Purpose:**

* **Cancels (voids) a transaction  in status “Pending”.**
* **Used when the merchant wants to cancel an ongoing transaction and initiate a new one with a fresh invoice**

#### **How It Works:**


* **A <code>POST</code> request is sent with the invoice identifier.**
* **The response confirms whether the void was successful.**

#### **Example Usage:**

**Request:**


```shell
POST https://api.payop.com/v1/checkout/void
```


**Key Parameter:**



* **<code>invoiceIdentifier</code>: The unique invoice ID.**

**Response Example (Success):**


```shell
{
 "data": {
   "isSuccess": true,
   "message": "",
   "txid": "canceled_transaction_id"
 },
 "status": 1
}
```
