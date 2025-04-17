* [Back to contents](../Readme.md#contents)

### **Summary of Invoice API Endpoints**


<table>
  <tr>
   <td><strong>Action</strong>
   </td>
   <td><strong>Method</strong>
   </td>
   <td><strong>Endpoint</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Create Invoice</strong>
   </td>
   <td><strong>POST</strong>
   </td>
   <td><strong><code>/v1/invoices/create</code></strong>
   </td>
  </tr>
  <tr>
   <td><strong>Get Invoice Info</strong>
   </td>
   <td>![GET](https://img.shields.io/badge/-GET-blue?style=for-the-badge)
   </td>
   <td><strong><code>/v1/invoices/{invoiceID}</code></strong>
   </td>
  </tr>
  <tr>
   <td><strong>Get Available Payment Methods</strong>
   </td>
   <td><strong>GET</strong>
   </td>
   <td><strong><code>/v1/instrument-settings/payment-methods/available-for-application/{ID}</code></strong>
   </td>
  </tr>
</table>



## **1. Create Invoice**


#### **Endpoint:**


```shell

POST https://api.payop.com/v1/invoices/create

```



#### **Purpose:**



* **This API is used to create an invoice for processing payments.**
* **The payer will be redirected to either the checkout page or a specific payment method, based on the request parameters.**


#### **How It Works:**



* **A <code>POST</code> request is sent with order details, payer information, and payment preferences.**
* **Optionally, a specific payment method ID can be provided to redirect the payer directly to that method.**


#### **Key Parameters:**


<table>
  <tr>
   <td>Parameter
   </td>
   <td>Type
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td><strong><code>publicKey</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Public key issued in the project (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>order</code></strong>
   </td>
   <td><strong><code>JSON object</code></strong>
   </td>
   <td><strong>Order details (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>order.id</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Payment ID (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>order.amount</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Payment amount (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>order.currency</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Payment currency (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>order.description</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Description of payment.</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>order.items</code></strong>
   </td>
   <td><strong><code>json array</code></strong>
   </td>
   <td><strong>List of products/services (can be an empty array).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>payer</code></strong>
   </td>
   <td><strong><code>JSON object</code></strong>
   </td>
   <td><strong>Payer details (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>payer.email</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Payer email (required).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>language</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>Language of the checkout page (<code>en</code>, <code>ru</code>, etc.).</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>resultUrl</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>URL for successful payment redirection.</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>failPath</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>URL for failed payment redirection.</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>signature</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>SHA-256 hash for security verification.</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>paymentMethod</code></strong>
   </td>
   <td><strong><code>string</code></strong>
   </td>
   <td><strong>(Optional) Specific payment method ID.</strong>
   </td>
  </tr>
  <tr>
   <td><strong><code>metadata</code></strong>
   </td>
   <td><strong><code>JSON object</code></strong>
   </td>
   <td><strong>(Optional) Additional data for merchant use.</strong>
   </td>
  </tr>
</table>



#### **Request Example:**


```shell
curl -X POST "https://api.payop.com/v1/invoices/create" \
 -H "Content-Type: application/json" \
 -d '{
   "publicKey": "YOUR_PUBLIC_KEY",
   "order": {
       "id": "12345",
       "amount": "3",
       "currency": "EUR",
       "items": [
           {
               "id": "487",
               "name": "Item 1",
               "price": "0.90"
           },
           {
               "id": "358",
               "name": "Item 2",
               "price": "2.10"
           }
       ],
       "description": "Test Payment"
   },
   "signature": "GENERATED_SIGNATURE", // Signature generated by secret key, invoice amount, invoice currency, invoice id
   "payer": {
       "email": "test.user@payop.com"
   },
   "paymentMethod": 261,
   "language": "en",
   "resultUrl": "https://your.site/result",
   "failPath": "https://your.site/fail"
}'

```


#### **Successful Response Example:**


```shell
{
   "data": "ec2aa893-e7f5-4a0d-98c4-ef1a424eaf5d",
   "status": 1
}

```


> 🧾 **Please Note:**
>* **The invoice identifier is returned in the <code>identifier</code> header.**
>* **Do not use the identifier in the response body (it will be removed in future API versions).**


#### **Error Response Example (Wrong Signature):**


```shell
{
 "message": "Wrong signature"
}

```


## **2. Get Invoice Info**


#### **Endpoint:**


```shell
GET https://api.payop.com/v1/invoices/{invoiceID}
```


#### **Purpose:**


* **Retrieves detailed information about a specific invoice.**
* **Includes invoice status, payer details, items, and associated transaction.**


#### **How It Works:**



* **A <code>GET</code> request is sent with the invoice ID.**
* **The response contains invoice status, amount, payment method, and metadata.**


#### **Request Example:**


```shell
curl -X GET "https://api.payop.com/v1/invoices/{invoiceID}" \
 -H "Content-Type: application/json"

```


#### **Successful Response Example:**


```shell
{
   "data": {
       "identifier": "81962ed0-a65c-4d1a-851b-b3dbf9750399",
       "status": 0,
       "type": 1,
       "amount": 3,
       "currency": "EUR",
       "orderIdentifier": "test",
       "items": [
           {
               "id": "487",
               "name": "Item 1",
               "price": "0.90"
           },
           {
               "id": "358",
               "name": "Item 2",
               "price": "2.10"
           }
       ],
       "resultUrl": "https://your.site/result",
       "failUrl": "https://your.site/fail",
       "payer": {
           "email": "test.user@payop.com"
       },
       "paymentMethod": {
           "identifier": 261,
           "formType": "standard"
       },
       "createdAt": 1567754398
   },
   "status": 1
}

```


#### **Error Response Example (Invoice Not Found):**


```shell
{
 "message": "Invoice (Invoice_ID) not found"
}

```


** **


#### **Possible Invoice Statuses:**


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
   <td>0
   </td>
   <td>New
   </td>
   <td>Invoice created but no action taken.
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Paid
   </td>
   <td>Invoice was successfully paid.
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>Overdue
   </td>
   <td>Invoice expired (default expiration is 24 hours).
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>Pending
   </td>
   <td>Invoice is awaiting payment.
   </td>
  </tr>
  <tr>
   <td>5
   </td>
   <td>Failed
   </td>
   <td>Payment failed (technical or financial issues).
   </td>
  </tr>
</table>


## **3. Get Available Payment Methods**


#### **Endpoint:**


```shell
GET https://api.payop.com/v1/instrument-settings/payment-methods/available-for-application/{ID}
```



#### **Purpose:**



* **Returns available payment methods for a given application.**
* **Only these payment methods can be used to create an invoice.**


#### **How It Works:**



* **A <code>GET</code> request is sent with the application ID.**
* **The response includes available payment methods, supported currencies, and country restrictions.**


#### **Request Example:**


```shell
curl -X GET "https://api.payop.com/v1/instrument-settings/payment-methods/available-for-application/{APPLICATION_ID}" \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_JWT_TOKEN"
```




#### **Successful Response Example:**


```shell
{
   "data": [
       {
           "identifier": 336,
           "type": "cards_local",
           "title": "Argencard",
           "logo": "https://payop.com/assets/images/payment_methods/argencard.jpg",
           "currencies": ["USD"],
           "countries": ["AR"],
           "config": {
               "fields": [
                   {
                       "name": "email",
                       "type": "email",
                       "required": true
                   },
                   {
                       "name": "name",
                       "type": "text",
                       "required": true
                   },
                   {
                       "name": "nationalid",
                       "type": "text",
                       "required": true
                   }
               ]
           }
       }
   ],
   "status": 1
}

```


#### **Error Response Example (Invalid Token):**


```shell
{
 "message": "Authorization token invalid"
}
```
