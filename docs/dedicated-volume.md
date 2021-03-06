---
layout: fandogh
id: dedicated-volume
title: Dedicated Volume
sidebar_label: Dedicated Volume
---
هر سرویس دارای اطلاعات ارزشمندی است که باید بتوان آن‌ها را در فضایی پایا ذخیره کرد تا بتوان در هر زمان به آن دسترسی داشت. در بستر فندق، این محل‌های ذخیره سازی **volume** نام دارند.

<br/>
> توجه  داشته باشید که در حال حاضر volume ها از ویژگی resize پشتیبانی نمی کنند  و باید به خاطر داشته باشید که تا فعال شدن این ویژگی٬ حجم مورد نیاز خود را به گونه ای انتخاب کنید که مشکلی برای اطلاعات شما پیش نیاید.
<br/>

## انواع volume ها

فندق برای آنکه سرویس‌ها بتوانند اطلاعات خود را در فضایی پایا و مانا ذخیره کنند دو ویژگی در نظر گرفته است:

**۱- فضای ذخیره سازی اشتراکی  
۲- فضای ذخیره سازی اختصاصی (Dedicated Volume)**

  

#### ۱- فضای ذخیره سازی اشتراکی :

همانطور که از اسم این فضا مشخص است، سکوی فندق برای هر کاربر یک فضای ذخیره سازی اختصاصی را فراهم می کند.

با اینکه این فضا مشکل ذخیره سازی اطلاعات را حل می کند اما گزینه‌ای همه منظوره نیست. این فضا از فضای ذخیره سازی خود فندق بوده و سرعت خواندن و نوشتن بر روی آن کندتر از volume اختصاصی است.

برای موارد مصرفی همچون پایگاه‌های داده و یا سرویس‌های دیگری که در آن ها سرعت پروسه I/O از اهمیت بالایی برخوردار است٬ فندق Dedicated Volume ها را در اختیار شما قرار می‌دهد.

#### ۲- فضای ذخیره سازی اختصاصی (Dedicated Volume) :

برای آنکه بتوان از افت سرعت و کیفیت سرویس ها جلوگیری کرد فندق Dedicated Volume را به سکو خود اضافه کرده است تا با استفاده از آن هر سرویس بتواند فضایی اختصاصی برای ذخیره سازی اطلاعات خود داشته باشد.

تمام volume هایی که کاربران میسازند، همگی توسط فندق مدیریت می‌شوند تا شما نگران مدیریت و نگهداری آنها نباشید و همینطور در طراحی و پیاده سازی تیم فندق تمام تلاش خود را کرده است تا رویه ساخت و استفاده از volume ها تا حد امکان برای کاربرها ساده و به دور از هر گونه سختی باشد.

> توجه داشته باشید هر volume تنها به یک سرویس متصل می شود٬ لذا اگر تعداد replica بیشتر از ۱ باشد شما مجاز به استفاده از آن volume نخواهید بود و با پیغام خطا مواجه خواهید شد.

## مقایسه Volume اشتراکی با اختصاصی

| قابلیت                                      	| فضای اختصاصی 	| فضای اشتراکی 	|
|---------------------------------------------	|--------------	|--------------	|
|     قابلیت استفاده همزمان توسط چند سرویس    	| ندارد        	| دارد         	|
| قابلیت استفاده با سرویس‌هایی که رپلیکا دارند 	| ندارد        	| دارد         	|
| سرعت خواندن و نوشتن 	| سریع‌تر        	| کند‌تر         	|
| امکان تغییر مشخصات سرویس بدون داون تایم 	| ندارد        	| دارد         	|
| قابلیت افزایش فضا پس از ساخت	| بزودی        	| دارد         	|


## چگونگی ساخت volume
 فضاهای اختصاصی نیز مانند تمام منابع دیگری که در فندق ساخته می شوند نیازمند یک درخواست هستند.
برای این منظور کاربر می تواند با استفاده از دستور add درخواستی مبنی بر ساخت یک volume به سرور فندق بفرستد.

```
fandogh volume add --name vol1 -c 10
fandogh volume add --name vol2 -c 30
fandogh volume add --name vol3 -c 20
```


**پارامترهای دستور add :**

**n-** یا **name--** 
با استفاده از این پارامتر کاربر نام دلخواهی برای volume خود انتخاب می کند.

  
c- یا **capacity--** 
با استفاده از این پارامتر کاربر می تواند مقدار فضای مورد نیاز خود را وارد کند. 


> **توجه داشته باشید که واحد ورودی فضا گیگابایت است**

بعد از آنکه درخواستی مبنی بر ساخت یک volume به سمت سرور بیاید، فندق این درخواست ها را بررسی کرده و سریعا به Dedicated Volume خود منتقل می کند تا رویه ساخت volume انجام شود که در شکل زیر نمایی مفهومی از این رویه را مشاهده میکنید.

![Volume Creation](/img/docs/volume_creation.png "Volume Creation")

بعد از آنکه درخواست ساخت volume با موفقیت به پایان برسد، سرور پاسخی را در قالب جدول زیر برای شما ارسال خواهد کرد.

| Creation Date | Capacity | Volume | Mounted To | Status | Name |
|------|--------|------------|--------------------|------------|-----|
| 2018-11-19 13:15:17+00:00 | 10Gi | pvc-28a98eabebfd11e8 | None | Bound  | vol1

همانطور که در جدول بالا مشاهده می کنید، یک پیغام مبنی بر موفقیت آمیز بودن ساخت volume با نام vol1 نمایش داده شده و در ادامه آن یک جدول نمایش داده شده است که دارای چند ستون می باشد که در در آن اطلاعاتی مربوط به volume تازه ساخته شده آورده شده که در ادامه در مورد هر کدام از این فیلدها توضیح خواهیم داد.

**توضیح ستون ها :**

-   **Name**
    این ستون بیانگر نام volume می باشد که شما در دستور خود به سرور فرستاده اید.
    
-   **Status**
    این ستون بیانگر این موضوع است که volume ساخته شده به درستی به Block Storage متصل شده است یا خیر. مقادیر نمایش داده شده در این ستون دو حالت دارد:
    

- **Bound**
یعنی volume به درستی ساخته و تنظیم شده است و آماده اتصال به یک سرویس میشود.

- **Unbound**
یعنی ‌Block Storage هنوز در حال آماده سازی volume می باشد و باید کمی صبر کنید تا کار Block Storage تمام شده و وضعیت به Bound تغییر کند تا بتوانید volume را به سرویس مورد نظر خود متصل کنید.

  
**توجه:** برای اطلاع از وضعیت volume های خود می توانید از دستور زیر استفاده کنید.
```
fandogh volume list
```


این دستور جدولی شامل لیست تمام volume های شما را نمایش خواهد داد.

-   **Mounted To**
    این ستون به ما میگوید که آیا در حال حاضر به سرویسی متصل شده است یا خیر. در صورتی volume به سرویسی متصل باشد٬ نام آن سرویس در این ستون نمایش داده خواهد شد.
    
-   **Volume**
    این ستون نام کوبرنتیزی volume شما را نمایش میدهد.
    
-   **Capacity**
    این ستون حجم volume شما را نمایش میدهد.
    

-   **Creation Date**
همانطور که از نام این ستون مشخص است٬ تاریخ ساخته شدن یک volume در این ستون نمایش داده می شود.

## چگونگی اتصال سرویس به volume


![Volume Attachment](/img/docs/volume_attachment.png "Volume Attachment")

بعد از آنکه شما یک volume را ایجاد کردید٬ با استفاده از [مانیفست](https://docs.fandogh.cloud/docs/service-manifest.html) می توانید سرویس خود را٬ به volume دلخواهتان متصل کنید.

تنها کاری که باید انجام دهید این است که مانیفست سرویس فعلی را باز کرده و mount_path ای که میخواهید به volume متصل شود را نوشته و volume_name را که همان نام volume ساخته شده می باشد را هم مانند مانیفست زیر  به آن اضافه کنید.

```
volume_mounts:
- mount_path: /data
  volume_name: vol1
```


```
kind: InternalService  
name: cache  
spec:  
  image: library/redis:latest  
  image_pull_policy: IfNotPresent  
  replicas: 1  
  volume_mounts:  
   - mount_path: /data
     volume_name: vol1  
```
یا برای یک managed-service می‌توانید بسته به نوع آن از parameter مربوط به volume استفاده کنید:
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
      value: name_of_volme
  resources:
      memory: 500Mi


```

حال با استفاده از دستور service apply می توانید سرویس خود را اجاره کرده و در آخر ببینید که سرویس شما به درستی به volume متصل شده و اطلاعات مورد نیاز از مسیر data/ را در volume با نام vol1 ذخیره سازی می کند.

> توجه داشته باشید مانند تکه کد زیر، شما می توانید sub_path را هم به volume خود اضافه کنید تا از شلوغ شدن فضای ذخیره سازی جلوگیری به عمل آورید.
```
volume_mounts:
 - mount_path: /data
   sub_path: /sub_directory
   volume_name: vol1
```

## چگونگی جدا کردن سرویس از volume
برای آنکه بتوانید یک سرویس را از یک volume جدا کنید یا اصطلاحا detach کنید٬ کافی است سرویسی را که به آن متصل است destroy کرده و نام volume‌ای (volume_name) که در مانیفست آن سرویس بوده را حذف کنید.


## چگونگی حذف یک volume

برای آنکه بتوانید یک volume را حذف کنید، کافی است تا نام volume مورد نظر را به دستور زیر بدهید.

```
fandogh volume delete --name vol1
```

> **توجه : پاک کردن یک volume منجر به پاک شدن تمام محتوای داخل آن شده و بازگرداندن آن امکان‌ پذیر نخواهد بود.**

> توجه داشته باشید، اگر volume ای که قصد پاک کردن آن را دارید به یک سرویس متصل باشد، ابتدا باید آن سرویس را [destroy](https://docs.fandogh.cloud/docs/services.html) کرده و بعد تلاش به حذف volume مورد نظر کنید.



##  مدیریت volume‌ها
![ CLI Image](/img/docs/cli_image.png "CLI Image")

>شما همچنین می توانید با وارد کردن دستور`fandogh volume --help` در CLI لیست دستورات موجود را مشاهده کنید.

###  add

با وارد کردن دستور `fandogh volunme add --name VOLUME_NAME --capacity VOLUME_CAPACITY` شما می‌توانید یک volume با نام VOLUME_NAME و ظرفیت VOLUME_CAPACITY بسازید.

> **توجه داشته باشید که واحد ورودی فضا گیگابایت است**

* **name--** یا **n-**
پارامتر name یا n نمایانگر نام volume‌ای می باشد که میخواهید بسازید.
* **capacity--** یا **c-**
پارامتر capacity یا c بیانگر ظرفیت volume می باشد.

###  **list**
با استفاده از دستور `fandogh volume list` می توانید لیست تمام volumeهایی که تا به حال ساخته اید را مشاهده کنید.

###  **delete**

با استفاده از دستور `fandogh volume delete --name VOLUME_NAME` ٬ volume با نام VOLUME_NAME را پاک کنید.

* **name--** یا **n-**
پارامتر name یا n نمایانگر نام volume‌ای می باشد که میخواهید آن را پاک کنید.
