# 💳 FaradPay - Zero-Fee Crypto Payment Gateway SDK & Integration Guide

Welcome to the official integration guide for FaradPay (faradpay.com), a high-performance, zero-fee cryptocurrency payment gateway designed for e-commerce, Web3 platforms, and custom backend applications.

---

## 🌟 Key Features

* **Zero Transaction Fees:** Process merchant payments with 0% processing fees.
* **Instant Settlement (0-Conf):** Fast payment confirmations on Bitcoin Cash (BCH) and other supported networks.
* **CashAddr Native Support:** Standard CashAddr format (`bitcoincash:q...`) for error-free transactions.
* **Automated Webhooks:** Instant Payment Notifications (IPNs) sent directly to your server.
* **Multi-Currency Support:** Bitcoin Cash (BCH), Bitcoin (BTC), Ethereum (ETH), USDT, USDC, and more.

---

## 📚 API Documentation

For the full interactive API collection, endpoint specifications, and request/response payloads:

👉 View FaradPay Postman API Documentation:
https://documenter.getpostman.com/view/46702761/2sB3BBrXqH#3bf42af3-905c-4acf-955b-59fc1a7a3b01

---

## ⚡ Quickstart Integration Walkthrough

### 1. Create Invoice (Server-Side)

Send a `POST` request to generate a payment checkout link for your customer:

```json
POST /api/v1/invoices
Headers:
  x-api-key: YOUR_API_KEY
  Content-Type: application/json

Body:
{
  "price_amount": 49.99,
  "price_currency": "USD",
  "order_id": "ORDER_100234",
  "callback_url": "[https://yourdomain.com/api/webhooks/faradpay](https://yourdomain.com/api/webhooks/faradpay)",
  "success_url": "[https://yourdomain.com/checkout/success](https://yourdomain.com/checkout/success)",
  "cancel_url": "[https://yourdomain.com/checkout/cancel](https://yourdomain.com/checkout/cancel)",
  "description": "Store Purchase / Web3 Digital Product"
}
