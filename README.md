حتماً، گابلین‌ستیز. من یک فایل **README.md** آماده کرده‌ام که می‌توانید مستقیم در پروژه گیت خود قرار دهید. این فایل شامل توضیح کامل، نحوه استفاده و مثال‌هاست:

```markdown
# LILIYAR Payment Handler API

**Plugin Name:** LILIYAR Payment Handler  
**Version:** 1.0.1  
**Author:** Peyman Jalili  
**Website:** [https://elixweb.ir](https://elixweb.ir)  

این پلاگین وردپرس برای پردازش پرداخت‌ها از سایت‌های دیگر طراحی شده است. سایت‌های خارجی (حتی غیر وردپرسی) می‌توانند با ارسال درخواست HTTP به سایت شما (`liliyar.com`) پرداخت‌ها را انجام دهند و نتیجه پرداخت را دریافت کنند.

---

## ویژگی‌ها

- پردازش سفارش‌ها از سایت‌های خارجی
- افزودن محصول به سبد خرید و ایجاد سفارش در ووکامرس
- اعمال مبلغ سفارشی و ارز دلخواه
- ارسال اطلاعات پرداخت موفق به سایت مبدأ
- مدیریت سایت‌های معتبر و اختصاص کد احراز هویت
- انتخاب محصول پیش‌فرض برای پرداخت‌ها

---

## نحوه استفاده برای سایت مبدأ

### آدرس API:
```

[https://liliyar.com/?elixweb\_payment=process](https://liliyar.com/?elixweb_payment=process)

````

### پارامترهای GET قابل ارسال:

| پارامتر            | توضیح                                                                 |
|-------------------|-----------------------------------------------------------------------|
| auth_code         | کد احراز هویت اختصاصی برای سایت مبدأ (۱۰ کاراکتر)                     |
| amount            | مبلغ پرداخت                                                         |
| order_id          | شماره سفارش سایت مبدأ                                               |
| currency          | کد ارز ووکامرس (مثلاً IRR یا USD)                                     |
| product_details   | جزئیات محصول به صورت JSON (اختیاری، برای آینده)                       |
| callback          | آدرس URL بازگشتی برای ارسال نتیجه پرداخت                            |
| source_site       | دامنه سایت مبدأ (برای نمایش در نام محصول)                            |

---

### مثال درخواست:

```http
GET https://liliyar.com/?elixweb_payment=process
    &auth_code=ABC123XYZ9
    &amount=500000
    &order_id=1234
    &currency=IRR
    &callback=https://siteb.com/payment/callback
    &source_site=https://siteb.com
````

---

## پاسخ پرداخت موفق

بعد از پرداخت و ایجاد سفارش در ووکامرس، سایت شما اطلاعات پرداخت را با پارامترهای زیر به **callback URL** سایت مبدأ ارسال می‌کند:

| پارامتر              | توضیح                                 |
| -------------------- | ------------------------------------- |
| order\_id\_a         | شماره سفارش ایجاد شده در سایت LILIYAR |
| order\_id\_b         | شماره سفارش سایت مبدأ                 |
| status               | وضعیت سفارش (processing)              |
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

**مثال URL بازگشتی:**

```
https://siteb.com/payment/callback?order_id_a=5678&order_id_b=1234&status=processing&transaction_id=987654321&auth_code=ABC123XYZ9&billing_first_name=Ali&billing_last_name=Jalili&billing_email=ali@example.com&billing_phone=09123456789&billing_address_1=Tehran Street&billing_city=Tehran&billing_postcode=12345&billing_country=IR&source_site=siteb.com
```

---

## نکات امنیتی

* همیشه `auth_code` را بررسی کنید تا فقط سایت‌های معتبر بتوانند پرداخت ایجاد کنند.
* پارامتر `callback` باید HTTPS باشد.
* سایت شما می‌تواند چندین سایت مبدأ را مدیریت کند و کد احراز هویت هرکدام متفاوت باشد.

---

## نصب و راه‌اندازی

1. افزونه را در مسیر `/wp-content/plugins/liliyar-payment-handler/` قرار دهید.
2. از پنل وردپرس افزونه را فعال کنید.
3. در بخش **Authorized Payment Sites**، دامنه سایت مبدأ را ثبت کنید تا یک `auth_code` اختصاص داده شود.
4. در بخش **Select Product**، محصول پیش‌فرض ووکامرس را برای پردازش انتخاب کنید.

---

## تماس و پشتیبانی

برای راهنمایی بیشتر یا سوالات، به وبسایت [elixweb.ir](https://elixweb.ir) مراجعه کنید.

```

---

اگر بخوای، من می‌تونم یه نسخه **واقعی فایل README.md** آماده کنم که مستقیم روی گیت قرار بگیره و حتی یه **نمونه JSON محصول و callback** هم بهش اضافه کنم تا توسعه‌دهنده‌ها راحت‌تر تست کنن.  

میخوای این کارو انجام بدم؟
```
