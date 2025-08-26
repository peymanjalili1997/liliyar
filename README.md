```markdown
# LILIYAR Payment API for Non-WordPress Sites

این مستندات برای سایت‌ها و سیستم‌هایی که **وردپرس نیستند** تهیه شده است. با استفاده از این API، سایت‌ها می‌توانند درخواست پرداخت را به **LILIYAR** ارسال کنند و نتیجه پرداخت را دریافت کنند.

---

## آدرس API

```

[https://liliyar.com/?elixweb\_payment=process](https://liliyar.com/?elixweb_payment=process)

````

این آدرس باید برای درخواست HTTP به کار رود.

---

## پارامترهای ورودی

| پارامتر            | نوع      | توضیح                                                      |
|-------------------|---------|-----------------------------------------------------------|
| auth_code         | string  | کد احراز هویت اختصاصی که برای هر سایت صادر می‌شود       |
| amount            | float   | مبلغ پرداخت (مثلاً 500000)                                |
| order_id          | string  | شماره سفارش در سایت مبدأ                                  |
| currency          | string  | ارز ووکامرس (مثلاً IRR، USD)                              |
| product_details   | JSON    | جزئیات محصول (اختیاری، برای آینده)                        |
| callback          | string  | URL بازگشتی برای ارسال نتیجه پرداخت                     |
| source_site       | string  | دامنه یا نام سایت مبدأ                                    |

> نکته: درخواست‌ها می‌توانند از هر زبان برنامه‌نویسی ارسال شوند (PHP, Python, Node.js, Java, ...).

---

## مثال درخواست GET

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

## پاسخ پرداخت موفق

پس از پردازش سفارش، LILIYAR اطلاعات پرداخت را به **callback URL** شما ارسال می‌کند:

| پارامتر              | توضیح                                 |
| -------------------- | ------------------------------------- |
| order\_id\_a         | شماره سفارش ایجاد شده در LILIYAR      |
| order\_id\_b         | شماره سفارش سایت مبدأ                 |
| status               | وضعیت سفارش (processing یا completed) |
| transaction\_id      | شماره تراکنش ووکامرس                  |
| auth\_code           | کد احراز هویت سایت مبدأ               |
| billing\_first\_name | نام خریدار                            |
| billing\_last\_name  | نام خانوادگی خریدار                   |
| billing\_email       | ایمیل خریدار                          |
| billing\_phone       | شماره تلفن خریدار                     |
| billing\_address\_1  | آدرس خریدار                           |
| billing\_city        | شهر خریدار                            |
| billing\_postcode    | کدپستی خریدار                         |
| billing\_country     | کشور خریدار                           |
| source\_site         | دامنه سایت مبدأ                       |

**مثال callback URL نهایی:**

```
https://example.com/payment/callback?order_id_a=5678&order_id_b=1234&status=processing&transaction_id=987654321&auth_code=ABC123XYZ9&billing_first_name=Ali&billing_last_name=Jalili&billing_email=ali@example.com&billing_phone=09123456789&billing_address_1=Tehran Street&billing_city=Tehran&billing_postcode=12345&billing_country=IR&source_site=example.com
```

---

## نکات مهم و امنیتی

1. همیشه `auth_code` را بررسی کنید تا فقط سایت‌های معتبر بتوانند پرداخت ایجاد کنند.
2. `callback` باید HTTPS باشد تا اطلاعات حساس امن بمانند.
3. سایت شما می‌تواند چندین سایت مبدأ داشته باشد، هرکدام با `auth_code` متفاوت.
4. داده‌های ارسال شده به callback را برای ذخیره سفارش و تطبیق با سفارش داخلی سایت خود استفاده کنید.

---

## مراحل برنامه‌نویسی سایت مبدأ (غیر وردپرس)

1. مرحله پرداخت کاربر: مقدار `amount`, `order_id`, `currency` و اطلاعات محصول را آماده کنید.
2. یک درخواست GET یا POST به آدرس API بالا ارسال کنید و پارامترهای لازم را ارسال کنید.
3. LILIYAR محصول پیش‌فرض ووکامرس را اضافه کرده و سفارش را ایجاد می‌کند.
4. پس از پرداخت موفق، LILIYAR callback URL شما را با اطلاعات کامل تراکنش فراخوانی می‌کند.
5. اطلاعات دریافتی را ذخیره و سفارش سایت خود را تکمیل کنید.

---

## تماس و پشتیبانی

برای سوالات بیشتر یا راهنمایی، به وبسایت [https://elixweb.ir](https://elixweb.ir) مراجعه کنید.

```

---

اگر بخوای، من می‌تونم این رو یه **فایل واقعی `README.md` آماده دانلود** هم کنم تا مستقیم تو گیت پروژه قرار بدی و برنامه‌نویس‌ها بتونن راحت ازش استفاده کنن.  

میخوای برات آماده کنم؟
```
