# Payop REST-like API Reference

Payop API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer).

Payop API has predictable resource-oriented URLs, accepts [JSON](http://www.json.org/) request bodies,
returns [JSON](http://www.json.org/) responses and uses standard HTTPS response codes.

Each request to Payop API should have a `Content-Type` HTTPS header with `application/json` value.
    
## Contents

### 0. Integration

*Integration via API 
  *[API Integration Types](Integration/integrationApiTypes.md)
   * Pre-setup integration plugins
      * [Woocommerce](https://github.com/Payop/woocommerce-plugin)  
          
*[Response examples](Integration/responses.md)
*[Signature generation](Integration/signatureGenerator.md)

### 1. Invoice

An invoice is the core element of any payment. Every payment is made against an invoice, and checkout transactions can only be initiated for existing invoices.

* [Invoice](Invoice/invoice.md)

   
### 2. Checkout    

 Handling Payments Through the Checkout Flow

 * [IPN (instant payment notification)](Checkout/checkout.md)


### 3. Withdrawal

Steps to prepare and request a withdrawal.

* [Encrypt withdrawal data](Withdrawal/withdrawalEncrypt.md)
* [Send withdrawal](Withdrawal/withdrawalIpn.md)
   
### 4. Refund
    
How to process and handle refunds.

* [Refund](Refund/refund.md)


### 5. IPN (Instant Payment Notification)

Learn to receive and handle Instant Payment Notifications. 

* [IPN](IPN/ipn.md)
