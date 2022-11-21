---
layout: post
title: Data Sources
date: 2022-11-20 11:20:00 0330
# categories: "Unit1"
imgPath: ../../../assets/img/
---

از ابتدا شروع می‌کنیم. از لایه‌ی داده. اگه معماری‌ رو در یک خطِ سیر یا direction بکشیم data layer در پایین‌ترین نقطه قرار خواهد گرفت.

لایه داده شامل داده‌های اپلیکیشن و business logic هستش. Business logic چیزی هست که به اَپ‌ شما value میده. Business logic هست که نحوه ساخته شدن، ذخیره شدن و تغییر داده‌های اپلیکیشن رو مشخص می‌کنه.

لایه داده از repositoryها ساخته شده که هر کدوم از این repositoryها میتونن با صفر، یک یا چندین data source تعامل داشته باشن. Data sourceها همانطور که از اسمش مشخصه، مسئول تأمین داده برای اَپ با توجه به فرایندی که در برنامه روی میده (همون event) هستش.

![Data-Sources]({{ page.imgPath }}Data-Sources-1.gif#center)

دیتا‌سورس‌ها میتونن دیتا رو از شبکه، دیتابیس محلی (local database)، فایلها، و یا حتی از مموری ارایه بدن. 

نمونه‌ی نامگذاریِ کلاس‌هایِ Data Source با توجه به، جایی که داره ازش داده‌ها رو می‌گیره، می‌تونه بصورتِ زیر باشه:

1- Remote**Article**DataSource

2- Local**Users**DataSource

3- Remote**Movies**DataSource

و البته باید مسئول کار روی تنها یه سورسِ داده باشن که معمولا حاوی یک بخش از business logic هستش. برای مثال articles، users، یا movies.