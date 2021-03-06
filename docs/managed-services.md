---
layout: fandogh
id: managed-services
title: سرویس‌های مدیریت شده
sidebar_label: سرویس‌های مدیریت شده
---
## managed-service چیست؟
خیلی از سرویس‌ها مانند MySQL یا PostgreSQL به طور مداوم توسط کاربران فندق استفاده می‌شوند. ما در فندق برای ساده‌تر کردن راه‌اندازی این نوع سرویس‌ها٬ امکانی را طراحی کردیم که به کمک آن می‌توانید با سهولت بیشتری این نوع سرویس‌ها را راه‌اندازی کنید.

## انواع managed-service
برای مشاهده انواع managed-service می‌توانید از دستور `fandogh managed-service help` استفاده کنید و سپس با استفاده از دستور `fandogh managed-service deploy` سرویس مورد نظر خود را دیپلوی کنید.
هر managed-service بسته به نوع آن٬ حاوی مجموعه‌ای از پارامتر‌های قابل تنظیم است که می‌توانید از طریق سویچ `c-`  مشخص کنید، در اینجا به طور کامل تمام managed-service‌ها را بررسی می‌کنیم و پارامتر‌های قابل تنظیم هر کدام را توضیح می‌دهیم.


### ![MySQL + PHPMyAdmin](/img/docs/mysql-phpmyadmin.png "MySQL + PHPMyAdmin")

MySQL یکی از محبوب‌ترین RDBMS‌های امروزی است که طرفداران زیادی در ایران داد، به همین دلیل MySQL به عنوان اولین managed-service به فندق اضافه شد.\
این managed-service از دو image متفاوت تشکیل شده، یکی خود MySQL و دیگری PHPMyAdmin که یک رابط کاربری تحت وب برای MySQL است.\
برای دیپلوی کردن یک سرویس MySQL شما می‌توانید موارد زیر را هنگام دیپلوی مشخص کنید:
|کانفیگ|نوع|پیش‌فرض|توضیح|
|---	|---	|---	|---	|
|service_name| string| mysql| نامی که برای سرویس مایلید در نظر گرفته شود|
|phpmyadmin_enabled|boolean | true | فعال یا غیرفعال بودن PHPMyAdmin
|mysql_root_password| string| root| رمز عبور یوزر root دیتابیس|
|volume_name| string| None| نام volumeای که به سرویس وصل می شود|

> توجه داشته باشید که اگر میخواهید سرعت I/O در سرویس دیتابیس شما بیشتر شود٬ می‌توانید از volume‌ها استفاده کنید و نام آن را به صورت `c volume_name=VOLUME_NAME-` موقع ساخت Managed Service در cli وارد نمایید. ( VOLUME_NAME نام volume‌ای است که موقع ساخت آن تعیین کرده‌اید).

به عنوان مثال برای دیپلوی کردن یک MySQL می‌توانیم به این شکل یک سرویس بسازیم:
```
fandogh managed-service deploy mysql 9.1 \
     -c service_name=mydatabase \
     -c mysql_root_password=12341234\
     -c phpmyadmin_enabled=false
```
این دستور یک سرویس MySQL ایجاد می‌کند که :
* نام سرویس آن mydatabase است( یعنی در شبکه داخلی فضانام شما باقی سرویس‌ها از طریق نام mydatabase می‌توانند به آن متصل شوند) .
* رمز عبور root آن 12341234 است.
*  PHPMyAdmin هم در آن غیر‌فعال شده است.

### ![Postgresql + Adminer](/img/docs/postgresql-adminer.png "Postgresql + Adminer")

Postgresql یکی دیگر از RDBMS‌های معروف و پرطرفداری است که می‌توانید به سادگی به عنوان یک managed-service روی Namespace خود دیپلوی کنید.
هنگام دیپلوی کردن Postgresql پارامتر‌های زیر را می‌توانید مشخص کنید:
|کانفیگ|نوع|پیش‌فرض|توضیح|
|---	|---	|---	|---	|
|service_name| string| postgresql| نامی که برای سرویس مایلید در نظر گرفته شود|
|adminer_enabled|boolean | true | فعال یا غیرفعال بودن Adminer
|postgres_password| string| postgres| رمز عبور دیتابیس|
|volume_name| string| None| نام volumeای که به سرویس وصل می شود|

> توجه داشته باشید که اگر میخواهید سرعت I/O در سرویس دیتابیس شما بیشتر شود٬ می‌توانید از volume‌ها استفاده کنید و نام آن را به صورت `c volume_name=VOLUME_NAME-` موقع ساخت Managed Service در cli وارد نمایید. ( VOLUME_NAME نام volume‌ای است که موقع ساخت آن تعیین کرده‌اید).
> 
به عنوان مثال برای دیپلوی کردن یک Postgresql می‌توانیم به این شکل یک سرویس بسازیم:
```
  fandogh managed-service deploy postgresql 10.4 \
       -c service_name=test-dbms \
       -c adminer_enabled=false \
       -c postgres_password=test123
```
این دستور یک سرویس Postgresql ایجاد می‌کند که:
* نام سرویس آن test-dbms ( یعنی در شبکه داخلی فضانام شما باقی سرویس‌ها از طریق نام test-dbms می‌توانند به آن متصل شوند) .
* رمز عبور آن test123 است.
*  Adminer هم در آن غیر‌فعال شده است.
  

### Deploy With Manifest
  

شما همچنین می توانید برای اجرای راحت تر سرویس های مدیریت شده ار [مانیفست](https://docs.fandogh.cloud/docs/service-manifest.html) همانند مثال زیر استفاده کنید.

```
kind: ManagedService
name: db
spec:
  service_name: mysql
  version: 5.7
  parameters:
    - name: phpmyadmin_enabled
      value: true
    - name: mysql_root_password
      value: some_long_unpredictable_string
    - name: volume_name
      value: YOUR_VOLUME_NAME

  resources:
      memory: 800Mi
```
