# مستندات API پرداخت لیلیار برای سایت‌های غیر وردپرسی

این مستندات برای سایت‌ها و سیستم‌هایی که **وردپرس نیستند** تهیه شده است.  
با استفاده از این API، سایت‌ها می‌توانند درخواست پرداخت را به **LILIYAR** ارسال کنند و نتیجه پرداخت را دریافت کنند.

---

## آدرس API

https://liliyar.com/?elixweb_payment=process

yaml
Copy

---

## پارامترهای ورودی

| پارامتر            | نوع      | توضیح                                                      |
|-------------------|---------|-----------------------------------------------------------|
| auth_code         | string  | کد احراز هویت اختصاصی که برای هر سایت صادر می‌شود       |
| amount            | float   | مبلغ پرداخت (مثلاً 500000)                                |
| order_id          | string  | شماره سفارش در سایت مبدأ                                  |
| currency          | string  | ارز پرداخت (IRR, USD, ...)                                 |
| product_details   | JSON    | جزئیات محصول (اختیاری)                                     |
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
پاسخ پرداخت موفق
پس از پردازش سفارش، LILIYAR اطلاعات پرداخت را به callback URL شما ارسال می‌کند:

پارامتر	توضیح
order_id_a	شماره سفارش ایجاد شده در LILIYAR
order_id_b	شماره سفارش سایت مبدأ
status	وضعیت سفارش (processing یا completed)
transaction_id	شماره تراکنش ووکامرس
auth_code	کد احراز هویت سایت مبدأ
billing_first_name	نام خریدار
billing_last_name	نام خانوادگی خریدار
billing_email	ایمیل خریدار
billing_phone	شماره تلفن خریدار
billing_address_1	آدرس خریدار
billing_city	شهر خریدار
billing_postcode	کدپستی خریدار
billing_country	کشور خریدار
source_site	دامنه سایت مبدأ

مثال callback URL نهایی:

perl
Copy
https://example.com/payment/callback?order_id_a=5678&order_id_b=1234&status=processing&transaction_id=987654321&auth_code=ABC123XYZ9&billing_first_name=Ali&billing_last_name=Jalili&billing_email=ali@example.com&billing_phone=09123456789&billing_address_1=Tehran Street&billing_city=Tehran&billing_postcode=12345&billing_country=IR&source_site=example.com
نکات مهم و امنیتی
همیشه auth_code را بررسی کنید تا فقط سایت‌های معتبر بتوانند پرداخت ایجاد کنند.

callback باید HTTPS باشد تا اطلاعات حساس امن بمانند.

سایت شما می‌تواند چندین سایت مبدأ داشته باشد، هرکدام با auth_code متفاوت.

داده‌های ارسال شده به callback را برای ذخیره سفارش و تطبیق با سفارش داخلی سایت خود استفاده کنید.

مراحل برنامه‌نویسی سایت مبدأ (غیر وردپرس)
مرحله پرداخت کاربر: مقدار amount, order_id, currency و اطلاعات محصول را آماده کنید.

یک درخواست GET یا POST به آدرس API بالا ارسال کنید و پارامترهای لازم را ارسال کنید.

LILIYAR محصول پیش‌فرض ووکامرس را اضافه کرده و سفارش را ایجاد می‌کند.

پس از پرداخت موفق، LILIYAR callback URL شما را با اطلاعات کامل تراکنش فراخوانی می‌کند.

اطلاعات دریافتی را ذخیره و سفارش سایت خود را تکمیل کنید.
