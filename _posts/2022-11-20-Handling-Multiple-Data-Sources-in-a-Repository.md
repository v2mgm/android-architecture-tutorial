---
layout: post
title: Handling Multiple Data Sources in a Repository
date: 2022-11-20 11:39:00 0330
# categories: "Unit1"
imgPath: ../../../assets/img/
---
هندل کردن چند data source در یک repository می‌تونه چالش‌برانگیز باشه. نیازه که یک source of truth رو انتخاب کنید و از این مطمئن باشیم که اون همیشه در یه consistent state باشه. یه مثال عینی و غیرانتزاعی در این مورد ببینیم.

بر فرض مثال برای news دو تا سورس داده داریم: 

یکی، کلاسِ LocalNewsDataSource به Room database وابسته هست و می‌تونه لیستی از مقاله‌ها رو return کنه و یا لیست مقالات رو آپدیت کنه. و دومی، کلاسِ RemoteNewsDataSource به API client (مثلِ Retrofit Client) وابسته هست. این یکی فقط می‌تونه اخبار رو fetch کنه.

```kotlin
class LocalNewsDataSource(news: NewsDao) {
	suspend fun fetchNews(): List<Article>{ ... }
	suspend fun updateNews(news: List<Article>) { ... }
}

class RemoteNewsDataSource(apiClient: ApiClient) {
	suspend fun fetchNews(): List<Article>{ ... }
}
```

حال، کلاسِ NewsRepository می‌تونه به دیتاسورس‌هایی که در پاراگراف بالایی بهشون اشاره شد، وابسته باشه.
اگه بخواهیم داده رو در دیگر لایه‌ها واکشی کنیم از متد fetchNews در این repository استفاده می‌کنیم. متد fetchNews سعی می‌کنه در ابتدا اخبار رو از اینترنت واکشی کنه. اگه موفق بود local database رو آپدیت می‌کنه. اگه به خاطر نبودِ کانکشن داده یا مشکل سایت ناموفق بود خطاها رو لاگ می‌کنه. بجای این لاگ شاید بخواهید چیزی رو در UI نمایش بدین. در نهایت، نتیجه رو از دیتابیس بعنوان source of truth برمی‌گردونه.

```kotlin
class NewsRepository (
	val localNewsDataSource: LocalNewsDataSource,
	val remoteNewsDataSource: RemoteNewsDataSource
) {
	suspend fun fetchNews() : List<Article>{
		try { 
			val news = remoteNewsDataSource.fetchNews()
			localNewsDataSource.updateNews(news)
		} catch (exception: RemoteDataSourceNotAvailableException) {
			Log.d("NewsRepository", "Connection failed, using local data source")
		}
		return localNewsDataSource.fetchNews()
}
```

البته مورد بالا خیلی آسون هستش چون از داده‌‌ای استفاده کردیم که کاربر نمیتونه ویرایشش کنه. به هر حال رفع مغایرت می‌تونه چالش‌برانگیز باشه. برای مثال برنامه تقویم رو در نظر بگیرید که meeting همزمان توسط دو کاربر ویرایش شده. این حالت نیاز به فکر کردن داره چون درست انجام دادنش به منظور رعایت تجربه کاربری offline-first مهمه.

حتی کتابخانه‌هایی مثل Firestore که remote APIها و remote databaseها رو consume می‌کنند و مکانیزم caching دارن، نیز میتونن با conflicts روبرو بشن.