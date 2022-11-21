---
layout: post
title: Multiple levels of repositories
date: 2022-11-20 11:51:00 0330
# categories: "Unit1"
imgPath: ../../../assets/img/
---
همانطور که قبلاً دیدین repository می‌تونه به یک یا چندین data source وابسته باشه. در بعضی مواقع شاید بخواهید در چند سطح repository داشته باشین. و این اصلا ایرادی نداره و کاملا مساعد هستش.

![Multiple levels of repositories]({{ page.imgPath }}Multiple-levels-of-repositories-1.png#center){:width="500px" height="310px"}

در این مثال، UserRepository داده‌هایی رو از LoginRepository و RegistrationRepository نیاز داره. همینطور این دو repository میتونن data source رو هم به اشتراک بذارن.