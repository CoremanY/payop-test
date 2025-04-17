* [Back to contents](../Readme.md#contents)

# Withdrawal

**The Withdrawal API allows merchants to process fund withdrawals from their account to external recipients, such as customers, partners, or employees. Withdrawals can be created individually or in batches (mass payouts) and support multiple methods and currencies. All operations require authentication and encrypted payloads.**


<table>
  <tr>
   <td>
<h5 style="text-align: center"><strong>#</strong></h5>


   </td>
   <td>
<h5 style="text-align: center"><strong>Endpoint</strong></h5>


   </td>
   <td>
<h5 style="text-align: center"><strong>Method</strong></h5>


   </td>
   <td>
<h5 style="text-align: center"><strong>Auth Required</strong></h5>


   </td>
   <td>
<h5 style="text-align: center"><strong>Purpose</strong></h5>


   </td>
  </tr>
  <tr>
   <td><strong>1</strong>
   </td>
   <td><strong>/v1/withdrawals/user-withdrawals?query[identifier]={withdrawalId}</strong>
   </td>
   <td><strong>GET</strong>
   </td>
   <td><strong>✅ Yes</strong>
   </td>
   <td><strong>Get detailed information about a specific withdrawal by ID.</strong>
   </td>
  </tr>
  <tr>
   <td><strong>2</strong>
   </td>
   <td><strong>/v1/withdrawals/user-withdrawals</strong>
   </td>
   <td><strong>GET</strong>
   </td>
   <td><strong>✅ Yes</strong>
   </td>
   <td><strong>Retrieve a list of all withdrawals initiated by the merchant.</strong>
   </td>
  </tr>
  <tr>
   <td><strong>3</strong>
   </td>
   <td><strong>/v1/withdrawals/create-mass</strong>
   </td>
   <td><strong>POST</strong>
   </td>
   <td><strong>✅ Yes</strong>
   </td>
   <td><strong>Submit one or more withdrawal requests as a batch (mass withdrawal).</strong>
   </td>
  </tr>
  <tr>
   <td><strong>4</strong>
   </td>
   <td><strong>/v1/instrument-settings/payment-methods/available-withdrawal-for-user</strong>
   </td>
   <td><strong>GET</strong>
   </td>
   <td><strong>✅ Yes</strong>
   </td>
   <td><strong>Get all available withdrawal payment methods for the authenticated user.</strong>
   </td>
  </tr>
  <tr>
   <td><strong>5</strong>
   </td>
   <td><strong>{Withdraw IPN URL configured in project settings}</strong>
   </td>
   <td><strong>POST</strong>
   </td>
   <td><strong>❌ No</strong>
   </td>
   <td><strong>Receive asynchronous Instant Payment Notifications (IPNs) for status updates on withdrawals.</strong>
   </td>
  </tr>
</table>



### **1. Get Withdrawal Details**


### **Purpose:**

**Fetch detailed information about a specific withdrawal using its identifier.**


### **Endpoint:**


```shell
GET https://api.payop.com/v1/withdrawals/user-withdrawals?query[identifier]={withdrawalId}
```



### **Headers:**


```shell
Content-Type: application/
Authorization: Bearer YOUR_JWT_TOKEN
```



### **Request Example:**


```shell
curl -X GET "https://api.payop.com/v1/withdrawals/user-withdrawals?query[identifier]=WITHDRAWAL_ID" \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_JWT_TOKEN"
```



### **Withdrawal Statuses:**


<table>
  <tr>
   <td>Status
   </td>
   <td>Type
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td>1, 4
   </td>
   <td>pending
   </td>
   <td>Withdrawal initiated or processing
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>accepted
   </td>
   <td>Withdrawal completed successfully
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>rejected
   </td>
   <td>Withdrawal rejected
   </td>
  </tr>
</table>



### **2. Get Withdrawals List**


### **Purpose:**

**Retrieve a list of all withdrawals initiated by the merchant.**


### **Endpoint:**


```shell
GET https://api.payop.com/v1/withdrawals/user-withdrawals
```



### **Request Example:**


```shell
curl -X GET "https://api.payop.com/v1/withdrawals/user-withdrawals" \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_JWT_TOKEN"
```



### **3. Create Mass Withdrawal**


### **Purpose:**

**Send multiple withdrawal requests in a single encrypted payload to automate payouts.**


### **Endpoint:**


```shell
POST https://api.payop.com/v1/withdrawals/create-mass
```



### **Headers:**


```shell
Content-Type: application/
Authorization: Bearer YOUR_JWT_TOKEN
idempotency-key: YOUR_UNIQUE_UUID  (Optional, recommended)
```



### **Encryption Required:**



* **All withdrawal requests must be encrypted using Sodium Sealed Box encryption.**
* **The encrypted binary data should then be Base64 encoded before sending.**
* **Certificates can be generated in your merchant account.**

    **See “Request Payload Encrypt/Decrypt” section for PHP examples.**



### **Withdrawal Method Examples:**


#### **🟦 Volet (method: 14)**


```shell
{
 "direction": "Salary for September",
 "email": "recipient@example.com"
}
```


** **


#### **🟧 PayDo (method: 15)**


```shell
{
 "direction": "Commission payout",
 "referenceId": "recipient@example.com",
 "recipientAccountType": 1
}
```



### **Commission Types:**


<table>
  <tr>
   <td>Type
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Fee deducted from wallet
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>Fee deducted from transaction
   </td>
  </tr>
</table>



### **Sample Encrypted Payload Call:**


```shell
curl -X POST "https://api.payop.com/v1/withdrawals/create-mass" \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_JWT_TOKEN" \
 -H "idempotency-key: 5c2eeb7f-f7db-4b9f-ae1d-f1bc2e6e5694" \
 -d '{"data": "9kQ7v9nXLHjeOyIqi+hIJfEKuOCQZ2C5WWVcnmfPHUxh1EbK5g=="}'
```


** **


### **4. Get Available Withdrawal Methods**


### **Purpose:**

**Retrieve a list of withdrawal methods supported for the authenticated user.**


### **Endpoint:**


```shell
GET https://api.payop.com/v1/instrument-settings/payment-methods/available-withdrawal-for-user
```


** **


### **Request Example:**


```shell
curl -X GET "https://payop.com/v1/instrument-settings/payment-methods/available-withdrawal-for-user" \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_JWT_TOKEN"
```



### **Response Example:**


```shell
{
 "data": [
   {
     "type": 14,
     "name": "Volet",
     "currencies": ["EUR", "USD"]
   },
   {
     "type": 15,
     "name": "PayDo",
     "currencies": ["EUR", "USD"]
   }
 ],
 "status": 1
}
```



### **5. Withdrawal Flow Overview**



1. **Encrypt the withdrawal payload using your public certificate.**
2. **Create Mass Withdrawal request with encrypted Base64-encoded data.**
3. **Save the returned withdrawal ID.**
4. **Poll Status every 10 minutes using the <code>GET /withdrawals/user-withdrawals?query[identifier]</code> endpoint.**
5. **Update internal status only after receiving a final status (<code>accepted</code> or <code>rejected</code>).**


### **6. IPN – Withdrawal Notifications**


### **Purpose:**

**Automatically notify the merchant about withdrawal status updates.**


### **Delivery:**

**Sent to the <code>Withdraw IPN URL</code> configured in your merchant account.**


### **IP Whitelist:**

**Only accept IPNs from:**


```shell
18.199.249.46   
35.158.36.143   
3.125.109.58  
3.127.103.117
```



### **IPN Payload Example:**


```shell
{
 "transaction": {
   "withdrawalId": "d024f697-ba2d-456f-910e-4d7fdfd338dd",
   "state": 1,
   "amount": 100,
   "currency": "USD",
   "comment": "Manager comment",
   "metadata": {
     "key1": "Value",
     "key2": "Value"
   },
   "error": {
     "message": "",
     "code": ""
   }
 }
}
```



### **IPN Status Handling:**



* **Accept only the first IPN if status and data are identical.**
* **Accept IPNs with different status updates (e.g., from <code>pending</code> → <code>accepted</code>).**
* **Retry IPN delivery occurs for 24h until your server returns HTTP 200.**