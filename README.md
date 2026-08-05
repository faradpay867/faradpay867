# 💳 FaradPay — Zero-Fee Crypto Payment Gateway SDK & Integration Guide

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![API Status](https://img.shields.io/badge/API-Online-brightgreen.svg)](https://documenter.getpostman.com/view/46702761/2sB3BBrXqH#3bf42af3-905c-4acf-955b-59fc1a7a3b01)
[![Supported Coins](https://img.shields.io/badge/Supported--Coins-BCH%20%7C%20BTC%20%7C%20ETH%20%7C%20USDT%20%7C%20USDC-orange.svg)](#-supported-currencies)
[![Build Status](https://img.shields.io/badge/Build-Passing-success.svg)](https://faradpay.com)

Welcome to the official integration repository for [FaradPay](https://faradpay.com) — a high-performance, non-custodial, zero-fee cryptocurrency payment gateway built for e-commerce merchants, Web3 SaaS platforms, and enterprise backend applications.

---

## 🌟 Key Features

* **0% Processing Fees:** Process 100% of merchant transactions with zero processing cuts.
* **Instant Settlement (0-Conf):** High-speed payment detection on Bitcoin Cash (BCH) and supported networks.
* **Native CashAddr Support:** Full native support for Bitcoin Cash addresses (`bitcoincash:q...`) to eliminate user address errors.
* **Automated Webhooks & IPNs:** Real-time server-to-server HTTP callbacks with instant confirmation delivery.
* **Multi-Currency Ecosystem:** Support for Bitcoin Cash (BCH), Bitcoin (BTC), Ethereum (ETH), USDT, USDC, and popular EVM tokens.

---

## 🚀 Quickstart Integration Walkthrough

### 1. Environment Setup

Obtain your API Key from your merchant account dashboard on [FaradPay Official Website](https://faradpay.com) and store it securely in your environment config:

```env
FARADPAY_API_KEY=fp_live_your_api_key_here
FARADPAY_BASE_URL=https://api.faradpay.com
```

---

### 2. Create Invoice (Server-Side)

#### **cURL Command**

```bash
curl -X POST "https://api.faradpay.com/v1/invoices" \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "price_amount": 49.99,
    "price_currency": "USD",
    "order_id": "ORDER_100234",
    "callback_url": "https://yourdomain.com/api/webhooks/faradpay",
    "success_url": "https://yourdomain.com/checkout/success",
    "cancel_url": "https://yourdomain.com/checkout/cancel",
    "description": "Digital Product / Web3 Subscription"
  }'
```

#### **Node.js / Express Implementation**

```javascript
const axios = require('axios');

async function createFaradPayInvoice(orderData) {
  try {
    const response = await axios.post(
      'https://api.faradpay.com/v1/invoices',
      {
        price_amount: orderData.amount,
        price_currency: 'USD',
        order_id: orderData.orderId,
        callback_url: 'https://yourdomain.com/api/webhooks/faradpay',
        success_url: 'https://yourdomain.com/checkout/success',
        cancel_url: 'https://yourdomain.com/checkout/cancel',
        description: orderData.description
      },
      {
        headers: {
          'x-api-key': process.env.FARADPAY_API_KEY,
          'Content-Type': 'application/json'
        }
      }
    );

    // Redirect customer to the checkout payment URL
    return response.data.invoice_url;
  } catch (error) {
    console.error('FaradPay Invoice Error:', error.response?.data || error.message);
    throw error;
  }
}
```

---

### 3. Handle Webhooks / Instant Payment Notifications (IPN)

Configure a secure HTTP POST endpoint on your backend to handle automatic payment updates:

```javascript
const express = require('express');
const app = express();

app.use(express.json());

app.post('/api/webhooks/faradpay', (req, res) => {
  const { invoice_id, order_id, payment_status, pay_amount, pay_currency } = req.body;

  // Process payment confirmation
  if (payment_status === 'confirmed' || payment_status === 'paid') {
    // 1. Update order status in your database
    // 2. Grant product access or user service
    console.log(`[SUCCESS] Payment verified for Order #${order_id}: ${pay_amount} ${pay_currency}`);
  }

  // Acknowledge receipt to FaradPay servers
  res.status(200).json({ received: true });
});

app.listen(3000, () => console.log('FaradPay Webhook Receiver active on port 3000'));
```

---

## 📊 Payment Lifecycle Statuses

| Status | Description | Action Required |
| :--- | :--- | :--- |
| `pending` | Invoice created; awaiting user transaction. | Show checkout interface |
| `detecting` | Transaction detected in mempool (0-conf). | Show pending confirmation |
| `confirmed` | Transaction confirmed on-chain. | Fulfill order / Deliver service |
| `expired` | Customer did not send payment within window. | Close transaction |
| `partial` | Payment amount insufficient. | Prompt user for remaining balance |

---

## 📚 Interactive API Documentation

For the complete collection of API endpoints, full request payload specs, and interactive testing environments:

👉 **[View FaradPay Postman API Documentation](https://documenter.getpostman.com/view/46702761/2sB3BBrXqH#3bf42af3-905c-4acf-955b-59fc1a7a3b01)**

---

## 🔗 Official Links & Resources

* **Website:** [FaradPay Zero-Fee Payment Gateway](https://faradpay.com)
* **API Portal:** [FaradPay Postman Collection](https://documenter.getpostman.com/view/46702761/2sB3BBrXqH#3bf42af3-905c-4acf-955b-59fc1a7a3b01)
* **Support:** [support@faradpay.com](mailto:support@faradpay.com)

---

## 📄 License

This project and integration guide are distributed under the [MIT License](LICENSE).
