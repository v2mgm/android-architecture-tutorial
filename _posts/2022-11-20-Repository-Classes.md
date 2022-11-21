---
layout: post
title: Repository Classes
date: 2022-11-20 11:33:00 0330
# categories: "Unit1"
imgPath: ../../../assets/img/
---

برای اینکه دیگر لایه‌ها با data layer تعامل داشته باشن از کلاس‌های repository استفاده می‌کنیم. کلاس‌های repository مسئول انتقال (exposing) داده‌ها به باقی برنامه هستند. همچنین مسئول متمرکز کردن تغییرات در داده‌ها، رفع مغایرت‌ها بین چندین data source، و حاوی business logic هستن:

- Expose data
- Centralized changes
- Resolve conflicts
- Contain business logic

### One repository per data type

باید برای هر نوع داده‌ای که در برنامه‌تون هندل می‌کنید repository class بسازید. برای مثال، باید برای داده‌های مربوط به movies یک `moviesRepository` بسازیم و یا برای payments یک `paymentsRepository` بسازیم.

```kotlin
class moviesRepository(...)
```

```kotlin
class paymentsRepository(...)
```

### Dependencies

ریپازیتوری‌ها می‌تونن data source‌های مختلف رو با هم ترکیب کنن (یکی کنن) و البته در این حالت مسئول رفع هر نوع مغایرتِ ممکن بین این data sourceها هستن. برای مثال اگه تداخل داده‌‌ای بین دیتابیس محلی و سرور وجود داشت repository باید اون رو تشخیص بده و رفعش کنه.

```kotlin
class MoviesRepository(
	databaseSource: Database,
	apiSource: ApiDataSource,
) {
	...
}
```

### Exposing data and receiving events

در نظر داشته باشید که تمامی لایه‌های موجود در برنامه‌تون نباید به صورت مستقیم به data source وابسته (depend) باشن. نقطه‌ی ورود به data layer همیشه کلاس‌های repository هستن.

```kotlin
class MoviesRepository( ... ) {
	
	suspend fun markMovieAsFavorite(
		movieId: String, isFavorite: Boolean
	)	{ ... }

	suspend fun fetchMovies() { ... }

}
```

الگوی معمول در repositoryها عمل کردن بصورت one-shot call هست. مثل create، read، update و delete. این موارد می‌تونن با suspend functionها و یا flowها (مطلع شدن از هر تغییر در داده. این شاملِ داده‌هایی هست که بصورتِ stream ارائه می‌شن) پیاده‌سازی بشن.

```kotlin
class MoviesRepository( ... ) {
	
	suspend fun markMovieAsFavorite(
		movieId: String, isFavorite: Boolean
	)	{ ... }

	val movies: Flow<Movie> = ...

}
```