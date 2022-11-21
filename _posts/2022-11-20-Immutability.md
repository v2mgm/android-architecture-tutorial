---
layout: post
title: Immutability
date: 2022-11-20 11:43:00 0330
# categories: "Unit1"
imgPath: ../../../assets/img/
---

حال بیایید راجع به immutability یا تغییرناپذیری صحبت کنیم. داده‌هایی که توسط data layer در دسترس قرار میگیرن (expose میشن) نباید mutable یا تغییرپذیر باشن در غیرِاین‌صورت دیگر کلاس‌ها خواهند تونست با داده‌ها وَر برن. که در این صورت، داده‌ها رو در ریسکِ inconsistent state یا داده‌هایی ناهماهنگ قرار میده.
یکی دیگر از مزایای immutable data اینه که داده‌ها بصورت امن در حالت multiple thread قابل هندل خواهند بود.

```kotlin
data class ArticleApiModel(
	val id: Long,
	val title: String,
	val content: String,
	val publicationDate: Date,
	val modifications: Array<ArticleApiModel>,
	val comments: Array<CommentApiModel>,
	val lastModificationDate: Date,
	val authorId: Long,
	val authorName: String,
	val authorDateOfBirth: Date,
	val readTimeMin: Int
}
```

یکی از ابزارهای عالی برای داشتن immutable data دیتاکلاس‌ها هستند.  به هر حال زمانیکه entityها یا موجودیت‌ها model میشن باید در نظر داشته باشید مدل نوشته‌شده برای دیتابیس یا remote API نباید اون چیزی باشه که دیگر لایه‌ها هم نیاز دارن. برای مثال در مدلِ `ArticleApiModel` به `modification` یا `author’s date of birth` و برخی از فیلدها نیازی نیست، یک مدل دیگری برای UI layer بسازید.

```kotlin
data class Article(
	val id: Long,
	val title: String,
	val content: String,
	val publicationDate: Date,
	val authorName: String,
	val readTimeMin: Int
)
```

این علاوه بر اینکه کد شما رو مرتب‌تر می‌کنه، separation of concerns رو هم ممکن می‌کنه. برای هر لایه مدلی که نیاز داره رو تعریف کنید.
منظور از separation of concerns: تفکیک قطعات نرم افزاری؛ در واقع یک قاعده‌ی طراحی است که در آن برنامه به بخش‌های مجزا تقسیم می‌شوند و هر بخش کار خاصی را انجام می‌دهد. (دیکشنری آبادیس)