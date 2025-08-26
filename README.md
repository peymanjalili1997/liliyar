
-----


# مستندات API پرداخت لیلیار برای سایت‌های غیر وردپرسی

این مستندات برای توسعه‌دهندگانی تهیه شده است که قصد دارند سیستم پرداخت **لیلیار (LILIYAR)** را در سایت‌ها و پلتفرم‌های **غیر وردپرسی** پیاده‌سازی کنند. با استفاده از این API، می‌توانید درخواست‌های پرداخت را به سادگی ایجاد کرده و نتیجه آن را به صورت خودکار دریافت و پردازش کنید.

-----

## ۱. آدرس API (Endpoint)

برای ارسال درخواست‌های پرداخت، از آدرس زیر استفاده کنید. تمام پارامترها باید به این آدرس ارسال شوند.

```
https://liliyar.com/?elixweb_payment=process
```

-----

## ۲. پارامترهای ورودی (ارسال به LILIYAR)

درخواست‌های پرداخت باید شامل پارامترهای زیر باشند. این پارامترها را می‌توانید از طریق متد `GET` یا `POST` ارسال کنید.

| پارامتر | نوع | توضیح |
| :--- | :--- | :--- |
| `auth_code` | `string` | کد احراز هویت اختصاصی که برای هر سایت صادر می‌شود. **(اجباری)** |
| `amount` | `float` | مبلغ پرداخت (مثلاً: `500000`). **(اجباری)** |
| `order_id` | `string` | شماره سفارش منحصر به فرد در سایت مبدأ. **(اجباری)** |
| `callback` | `string` | آدرس URL بازگشتی برای دریافت نتیجه پرداخت. **(اجباری)** |
| `source_site` | `string` | دامنه یا نام سایت مبدأ (مثلاً: `example.com`). **(اجباری)** |
| `currency` | `string` | واحد پول پرداخت (مانند `IRR`, `USD`). **(اختیاری)** |
| `product_details` | `JSON` | جزئیات محصول به صورت یک رشته JSON. **(اختیاری)** |

> **نکته:** درخواست‌ها می‌توانند از هر زبان برنامه‌نویسی (مانند PHP, Python, Node.js, Java) ارسال شوند.

-----

## ۳. مثال درخواست (متد GET)

در مثال زیر، یک درخواست پرداخت با تمام پارامترهای لازم به API ارسال می‌شود:

```http
GET https://liliyar.com/?elixweb_payment=process&auth_code=ABC123XYZ9&amount=500000&order_id=1234&currency=IRR&callback=https://example.com/payment/callback&source_site=https://example.com
```

-----

## ۴. پارامترهای خروجی (دریافت از LILIYAR)

پس از پرداخت موفق توسط کاربر، لیلیار اطلاعات تراکنش را به **callback URL** که در درخواست اولیه ارسال کرده‌اید، ارسال می‌کند.

| پارامتر | توضیح |
| :--- | :--- |
| `order_id_a` | شماره سفارش ایجاد شده در سیستم لیلیار. |
| `order_id_b` | همان شماره سفارشی که از سایت مبدأ ارسال شده بود. |
| `status` | وضعیت سفارش (`processing` یا `completed`). |
| `transaction_id` | شماره تراکنش صادر شده توسط ووکامرس. |
| `auth_code` | کد احراز هویت سایت مبدأ برای تایید صحت درخواست. |
| `billing_first_name` | نام خریدار. |
| `billing_last_name` | نام خانوادگی خریدار. |
| `billing_email` | ایمیل خریدار. |
| `billing_phone` | شماره تلفن خریدار. |
| `billing_address_1` | آدرس خریدار. |
| `billing_city` | شهر خریدار. |
| `billing_postcode` | کد پستی خریدار. |
| `billing_country` | کشور خریدار. |
| `source_site` | دامنه سایت مبدأ. |

**مثال URL بازگشتی نهایی:**

```
https://example.com/payment/callback?order_id_a=5678&order_id_b=1234&status=processing&transaction_id=987654321&auth_code=ABC123XYZ9&billing_first_name=Ali&billing_last_name=Jalili&billing_email=ali@example.com&billing_phone=09123456789&billing_address_1=Tehran Street&billing_city=Tehran&billing_postcode=12345&billing_country=IR&source_site=example.com
```

-----

## ۵. مراحل پیاده‌سازی در سایت مبدأ

1.  **آماده‌سازی اطلاعات:** در مرحله پرداخت کاربر، اطلاعاتی مانند `amount`, `order_id`, `currency` را جمع‌آوری کنید.
2.  **ارسال درخواست:** یک درخواست `GET` یا `POST` به آدرس API لیلیار با پارامترهای مورد نیاز ارسال کنید.
3.  **ایجاد سفارش در لیلیار:** لیلیار با استفاده از اطلاعات دریافتی، یک محصول پیش‌فرض در ووکامرس ایجاد کرده و کاربر را به صفحه پرداخت هدایت می‌کند.
4.  **دریافت نتیجه:** پس از پرداخت موفق، لیلیار به `callback URL` شما اطلاعات کامل تراکنش را ارسال می‌کند.
5.  **تکمیل سفارش:** اطلاعات دریافتی را در دیتابیس خود ذخیره کرده و سفارش کاربر را در سایت خودتان تکمیل و تایید کنید.

-----

## ۶. نکات مهم و امنیتی

۱. **اعتبارسنجی `auth_code`:** همیشه در سمت سرور خود، `auth_code` دریافتی در callback را بررسی کنید تا مطمئن شوید درخواست از طرف لیلیار ارسال شده است.
۲. **استفاده از HTTPS:** برای جلوگیری از شنود اطلاعات حساس، حتماً از `https://` برای `callback URL` خود استفاده کنید.
۳. **مدیریت چند سایت:** شما می‌توانید چندین سایت مبدأ را مدیریت کنید که هر کدام `auth_code` منحصر به فرد خود را دارند.
۴. **تطبیق سفارش:** از `order_id_b` برای تطبیق اطلاعات بازگشتی با سفارش ثبت‌شده در سیستم خود استفاده کنید.

-----


# LILIYAR Payment API for Non-WordPress Sites

This documentation is for developers looking to integrate the **LILIYAR** payment system into **non-WordPress** websites and platforms. Using this API, you can easily create payment requests and automatically receive and process the results.

-----

## 1\. API Endpoint

Use the following URL to send payment requests. All parameters should be sent to this endpoint.

```
https://liliyar.com/?elixweb_payment=process
```

-----

## 2\. Request Parameters (Sent to LILIYAR)

Payment requests must include the following parameters. You can send them via `GET` or `POST` method.

| Parameter | Type | Description |
| :--- | :--- | :--- |
| `auth_code` | `string` | The unique authentication code issued for each site. **(Required)** |
| `amount` | `float` | The payment amount (e.g., `500000`). **(Required)** |
| `order_id` | `string` | The unique order ID from the source site. **(Required)** |
| `callback` | `string` | The URL to which the payment result will be sent. **(Required)** |
| `source_site` | `string` | The domain or name of the source site (e.g., `example.com`). **(Required)** |
| `currency` | `string` | The payment currency (e.g., `IRR`, `USD`). **(Optional)** |
| `product_details` | `JSON` | Product details as a JSON string. **(Optional)** |

> **Note:** Requests can be sent from any programming language (e.g., PHP, Python, Node.js, Java).

-----

## 3\. Example Request (GET Method)

The example below shows a payment request sent to the API with all required parameters:

```http
GET https://liliyar.com/?elixweb_payment=process&auth_code=ABC123XYZ9&amount=500000&order_id=1234&currency=IRR&callback=https://example.com/payment/callback&source_site=https://example.com
```

-----

## 4\. Response Parameters (Received from LILIYAR)

After a successful payment, LILIYAR will send the transaction details to the **callback URL** you provided in the initial request.

| Parameter | Description |
| :--- | :--- |
| `order_id_a` | The order ID created in the LILIYAR system. |
| `order_id_b` | The same order ID that was sent from the source site. |
| `status` | The order status (`processing` or `completed`). |
| `transaction_id` | The transaction ID issued by WooCommerce. |
| `auth_code` | The source site's authentication code to verify the request's origin. |
| `billing_first_name` | Customer's first name. |
| `billing_last_name` | Customer's last name. |
| `billing_email` | Customer's email. |
| `billing_phone` | Customer's phone number. |
| `billing_address_1` | Customer's address. |
| `billing_city` | Customer's city. |
| `billing_postcode` | Customer's postal code. |
| `billing_country` | Customer's country. |
| `source_site` | The source site domain. |

**Example Final Callback URL:**

```
https://example.com/payment/callback?order_id_a=5678&order_id_b=1234&status=processing&transaction_id=987654321&auth_code=ABC123XYZ9&billing_first_name=Ali&billing_last_name=Jalili&billing_email=ali@example.com&billing_phone=09123456789&billing_address_1=Tehran Street&billing_city=Tehran&billing_postcode=12345&billing_country=IR&source_site=example.com
```

-----

## 5\. Integration Steps for Source Site

1.  **Prepare Data:** During the user's checkout process, gather information such as `amount`, `order_id`, and `currency`.
2.  **Send Request:** Send a `GET` or `POST` request to the LILIYAR API endpoint with the required parameters.
3.  **Order Creation in LILIYAR:** LILIYAR will use the received data to create a default WooCommerce product and redirect the user to the payment page.
4.  **Receive Result:** After a successful payment, LILIYAR will send the complete transaction details to your `callback URL`.
5.  **Complete Order:** Store the received information in your database and complete the user's order on your site.

-----

## 6\. Security Notes

1.  **Validate `auth_code`:** Always verify the `auth_code` received in the callback on your server-side to ensure the request is from LILIYAR.
2.  **Use HTTPS:** To prevent eavesdropping on sensitive information, always use `https://` for your `callback URL`.
3.  **Manage Multiple Sites:** You can manage multiple source sites, each with its own unique `auth_code`.
4.  **Match Orders:** Use `order_id_b` to match the returned data with the corresponding order in your system.
