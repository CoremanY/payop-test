# How to Create and Encrypt Withdrawal Payload

**This guide explains how to prepare, encrypt, and submit a withdrawal request to the Payop API using your personal certificate and libsodium encryption.**


### **🧩 Step 1: Generate and Download a Certificate**



1. **Log in to your[ Payop Merchant Dashboard](https://payop.com/).**
2. **Navigate to <code>Settings</code> → <code>Certificates</code>.**
3. **Click Generate Certificate.**
4. **Enter the 6-digit 2FA code received on your registered email or app.**
5. **Download the <code>x25519.pub</code> certificate file.**
6. **Save the file to your working directory (e.g., <code>/project/x25519.pub</code>).**

> 🔐 This certificate is used to encrypt the payload so only Payop can decrypt it.



### **🧾 Step 2: Prepare the Withdrawal Payload (Raw JSON)**

**Prepare the JSON structure you want to send.**


```shell
[
 {
   "method": 14,
   "type": 1,
   "amount": 1000,
   "currency": "EUR",
   "additionalData": {
     "direction": "Test Payout",
     "email": "recipient@example.com"
   },
   "metadata": {
     "internalOrderId": "ORDER-2024-001"
   }
 }
]
```

* **<code>method</code>: Withdrawal method ID (e.g., <code>14</code> = Volet, <code>15</code> = PayDo)**
* **<code>type</code>: Commission type (1 - from wallet, 2 - from withdrawal amount)**
* **<code>amount</code>: Amount to be withdrawn**
* **<code>currency</code>: Payout currency (EUR, USD, etc.)**
* **<code>additionalData</code>: Method-specific fields (check required fields per method)**
* **<code>metadata</code>: Optional custom data (for internal tracking)**


---


### **🛠️ Step 3: Encrypt the Payload Using PHP and Sodium**


```shell
<?php
// Step 1: Load the certificate (binary file)

$certFilePath = '/project/x25519.pub'; // Update with actual path
$certificate = file_get_contents($certFilePath);

// Step 2: Prepare the withdrawal payload (same structure as in Step 2)

$data = [
   [
       'method' => 14,
       'type' => 1,
       'amount' => 1000,
       'currency' => 'EUR',
       'additionalData' => [
           'direction' => 'Affiliate Payout',
           'email' => 'recipient@example.com'
       ],
       'metadata' => [
           'internalOrderId' => 'ORDER-2024-001'
       ]
   ]
];

// Step 3: Encrypt using Sodium

$encrypted = sodium_crypto_box_seal(json_encode($data), $certificate);

// Step 4: Base64 encode

$base64Payload = base64_encode($encrypted);
echo "Encrypted payload:\n" . $base64Payload;
```


** **


> ⚠️ PHP 7.2+ includes Sodium natively. No extra dependencies needed.


---


### **🧪 Step 4: Submit the Payload to Payop API**

**Use <code>curl</code>, Postman, or your backend to send the request:**


```shell
curl -X POST https://api.payop.com/v1/withdrawals/create-mass \
 -H "Content-Type: application/json" \
 -H "Authorization: Bearer YOUR_JWT_TOKEN" \
 -H "idempotency-key: UNIQUE-UUID" \
 -d '{
   "data": "YOUR_BASE64_ENCRYPTED_PAYLOAD_HERE"
}'
```

> Replace <code>"YOUR_BASE64_ENCRYPTED_PAYLOAD_HERE"</code> with the output from the PHP script above.


### **✅ Example Encrypted Payload**


```
{"data":"YwukuaG2G0whJfHWpdIXrMay76o1\/VuSmUr8cpSUGCDhqEnry4FsFOTJpmccoQ6w\/Z2VmQKgkvJ\/Hz7v8VrvYSfoTsnX4cKvoUasgC2xOwgPdYzcmx5zIq4SlEHx418OwM\/oqAHb5cEO\/IFwBlMHzL1IAc7yFCwOjSUMg+8SNlawtTLGRNtIb8V6\/gqZRZXoyQHuXoclRE1tR\/2GZjjiD6aEM0JQPNg3NssPxuQuRiRAgzhMPrnCS53FQIjZrR9sVDcb4iJxhpXORHpSZ8vLgT9Ya6RSdsNUWrpaTUBr1MACULgN8Oib1G8U7PK3YoNb8APfibnAklTLuo7HDkvFB7FQ+xIOvYfg4LgK5vbQIRensLzGVz8ktIOyLpwOw2wBgf6HrUI2HiooCg+o9hR0qqRoZCSi\/psRgQU5Ry3fSSWsw+Q39pNCm9sbQvYQZ6akti7KrcDYdLAjKFKu3DdB2lX38shjErM\/QMxjBegeOsl50DouknCq1BImSCR5HYJhJgRP1sZo8S5RfiBjSzXbgcddaSG6kIkRirCt"}
```