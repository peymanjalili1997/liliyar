
---

```markdown
# LILIYAR Payment API for Non-WordPress Sites
# مستندات API پرداخت لیلیار برای سایت‌های غیر وردپرسی

این مستندات برای سایت‌ها و سیستم‌هایی که **وردپرس نیستند** تهیه شده است.  
با استفاده از این API، سایت‌ها می‌توانند درخواست پرداخت را به **LILIYAR** ارسال کنند و نتیجه پرداخت را دریافت کنند.

This documentation is for non-WordPress sites.  
Using this API, sites can send payment requests to **LILIYAR** and receive the payment results.

---

## آدرس API / API Endpoint

```

[https://liliyar.com/?elixweb\_payment=process](https://liliyar.com/?elixweb_payment=process)

````

این آدرس باید برای درخواست HTTP به کار رود.  
This endpoint should be used for HTTP requests.

---

## پارامترهای ورودی / Request Parameters

| پارامتر / Parameter    | نوع / Type | توضیح / Description |
|-----------------------|------------|-------------------|
| auth_code             | string     | کد احراز هویت اختصاصی که برای هر سایت صادر می‌شود / Authentication code issued for each site |
| amount                | float      | مبلغ پرداخت (مثلاً 500000) / Payment amount, e.g., 500000 |
| order_id              | string     | شماره سفارش در سایت مبدأ / Order ID in source site |
| currency              | string     | ارز پرداخت (IRR, USD, ...) / Currency (IRR, USD, etc.) |
| product_details       | JSON       | جزئیات محصول (اختیاری) / Product details (optional) |
| callback              | string     | URL بازگشتی برای ارسال نتیجه پرداخت / Callback URL for payment result |
| source_site           | string     | دامنه یا نام سایت مبدأ / Domain or name of source site |

> نکته: درخواست‌ها می‌توانند از هر زبان برنامه‌نویسی ارسال شوند (PHP, Python, Node.js, Java, ...).  
> Note: Requests can be sent from any programming language (PHP, Python, Node.js, Java, ...).

---

## مثال درخواست GET / Example GET Request

```http
GET https://liliyar.com/?elixweb_payment=process
    &auth_code=ABC123XYZ9
    &amount=500000
    &order_id=1234
    &currency=IRR
    &callback=https://example.com/payment/callback
    &source_site=https://example.com
````

---

## پاسخ پرداخت موفق / Successful Payment Response

پس از پردازش سفارش، LILIYAR اطلاعات پرداخت را به **callback URL** شما ارسال می‌کند:
After processing the order, LILIYAR will send the payment information to your **callback URL**.

| پارامتر / Parameter  | توضیح / Description                                                            |
| -------------------- | ------------------------------------------------------------------------------ |
| order\_id\_a         | شماره سفارش ایجاد شده در LILIYAR / Order ID in LILIYAR                         |
| order\_id\_b         | شماره سفارش سایت مبدأ / Order ID in source site                                |
| status               | وضعیت سفارش (processing یا completed) / Order status (processing or completed) |
| transaction\_id      | شماره تراکنش ووکامرس / WooCommerce transaction ID                              |
| auth\_code           | کد احراز هویت سایت مبدأ / Auth code from source site                           |
| billing\_first\_name | نام خریدار / Customer first name                                               |
| billing\_last\_name  | نام خانوادگی خریدار / Customer last name                                       |
| billing\_email       | ایمیل خریدار / Customer email                                                  |
| billing\_phone       | شماره تلفن خریدار / Customer phone                                             |
| billing\_address\_1  | آدرس خریدار / Customer address                                                 |
| billing\_city        | شهر خریدار / City                                                              |
| billing\_postcode    | کدپستی / Postal code                                                           |
| billing\_country     | کشور / Country                                                                 |
| source\_site         | دامنه سایت مبدأ / Source site domain                                           |

**مثال callback URL نهایی / Example final callback URL:**

```
https://example.com/payment/callback?order_id_a=5678&order_id_b=1234&status=processing&transaction_id=987654321&auth_code=ABC123XYZ9&billing_first_name=Ali&billing_last_name=Jalili&billing_email=ali@example.com&billing_phone=09123456789&billing_address_1=Tehran Street&billing_city=Tehran&billing_postcode=12345&billing_country=IR&source_site=example.com
```

---

## نکات مهم و امنیتی / Security Notes

1. همیشه `auth_code` را بررسی کنید تا فقط سایت‌های معتبر بتوانند پرداخت ایجاد کنند.
   Always verify `auth_code` to ensure only authorized sites can create payments.

2. `callback` باید HTTPS باشد تا اطلاعات حساس امن بمانند.
   The `callback` URL must be HTTPS to secure sensitive information.

3. سایت شما می‌تواند چندین سایت مبدأ داشته باشد، هرکدام با `auth_code` متفاوت.
   Your system can have multiple source sites, each with a unique `auth_code`.

4. داده‌های ارسال شده به callback را برای ذخیره سفارش و تطبیق با سفارش داخلی سایت خود استفاده کنید.
   Use the data sent to the callback to store the order and match it with your local order records.

---

## مراحل برنامه‌نویسی سایت مبدأ (غیر وردپرس) / Integration Steps for Non-WordPress Sites

1. مرحله پرداخت کاربر: مقدار `amount`, `order_id`, `currency` و اطلاعات محصول را آماده کنید.
   Prepare `amount`, `order_id`, `currency`, and product details during user checkout.

2. یک درخواست GET یا POST به آدرس API بالا ارسال کنید و پارامترهای لازم را ارسال کنید.
   Send a GET or POST request to the API endpoint with the required parameters.

3. LILIYAR محصول پیش‌فرض ووکامرس را اضافه کرده و سفارش را ایجاد می‌کند.
   LILIYAR will add a default WooCommerce product and create the order.

4. پس از پرداخت موفق، LILIYAR callback URL شما را با اطلاعات کامل تراکنش فراخوانی می‌کند.
   After successful payment, LILIYAR calls your callback URL with full transaction information.

5. اطلاعات دریافتی را ذخیره و سفارش سایت خود را تکمیل کنید.
   Store the received data and complete the order on your site.

```

---
