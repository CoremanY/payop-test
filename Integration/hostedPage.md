* [Back to contents](../Readme.md#contents)

# Integration API Types

Payop offers two primary integration options for merchants to process payments: **Hosted Page Integration** and **Direct Integration**. Each method has its own benefits, depending on your business needs, technical capabilities, and the level of control you want over the payment process.

## **1. Hosted Page Integration**


### **Overview**

The **Hosted Page Integration** is the **simplest** and **most convenient** method for merchants who prefer a **quick and easy** way to accept payments without extensive development efforts. Payop handles most of the payment flow, including security, compliance, and user experience.


### **How It Works**

1. **Create an Invoice** – A request is sent to generate a payment invoice. `POST https://api.payop.com/v1/invoices/create` (See the Invoice section for more details)
2. **Redirect the Payer** – The payer is redirected to the **invoice preprocessing page**. (`https://checkout.payop.com/{{locale}}/payment/invoice-preprocessing/{{invoiceId}}`)
3. **Payer Enters Required Data:** On the Payop checkout page, the payer fills in necessary details (e.g., name, date of birth, email, etc.).
4. **Automatic Processing** – Payop determines the next steps, such as selecting the appropriate payment method or requiring additional details.
5. **Payment Confirmation** – If the payment is successful, the payer is redirected to the `resultUrl`. If the payment fails, the payer is redirected to the `failPath`.
6. **Receive IPN (Instant Payment Notification)** If IPNs are configured, Payop will automatically notify your server when the transaction status changes. This ensures your backend is updated even if the user does not return to your site. \
 *(See[ Checkout → IPN](LINK)*


### **Checkout Flow**

1. **Create Invoice** `POST https://api.payop.com/v1/invoices/create` (See the Invoice section for more details)
2. **Redirect payer to invoice preprocessing page:**
3. `https://checkout.payop.com/{{locale}}/payment/invoice-preprocessing/{{invoiceId}}`
    * `{{locale}}` → Language of the invoice (`en`, `ru`, etc.).
    * `{{invoiceId}}` → Unique invoice identifier.
4. If all required fields are correctly filled, the system attempts to process the payment.
5. If required fields are missing, the payer is redirected to **checkout form completion**.
6. If additional authentication is needed the payer is redirected to the appropriate forms.
7. **Redirection:**
    * On **successful payment**, the payer is redirected to the `resultUrl` specified when creating the invoice.
    * On **failed payment**, the payer is redirected to `failPath`.

### **Advantages of Hosted Page Integration**

✅ **Quick Setup:** No development expertise required.
✅ **Minimal Effort:** Payop handles the checkout process, reducing merchant workload. 
✅ **Secure and Compliant:** Payment processing fully complies with industry regulations and security standards without requiring additional efforts from merchants 
✅ **Global Payment Methods:** The payer sees only available methods based on their country and IP.


### **Best Use Cases**



* Businesses that want **a fast and hassle-free** integration.
* Merchants who **do not want to build** their own checkout page.
* Companies needing **multi-language** and **multi-currency** payment solutions with minimal setup.


## **2. Direct Integration**

The Direct Integration option provides a more advanced and customizable way for merchants to accept payments by bypassing the Payop-hosted checkout page. Instead, merchants integrate specific payment methods directly into their own system, giving them full control over the user experience, design, and data collection flow.

This method is ideal for businesses with development resources who want to embed payments directly into their own UI and reduce friction during the checkout process.


### **How It Works**