---
layout: post
title: Threading
date: 2022-11-20 11:45:00 0330
# categories: "Unit1"
imgPath: ../../../assets/img/
---
```kotlin
suspend fun fetchDataFromNetwork() {
	withContext(Dispatchers.IO) {
		fetch()
	}
}
```

صدا زدن (call کردن) data sourceها و repositoryها باید main safe باشه. یعنی بدون بروز مشکلی و بصورت safe اون‌ها رو از main thread صدا زد. این یعنی data sourceها و repositoryها باید زمانی که اجرای لاجیک، long-running یا blocking operation هست، اون منطق رو در ترد مناسبی منتقل و اجرا کنند. (در مثال بالا، فانکشنِ `fetchDataFromNetwork` عملِ fetch روی ترد IO-optimized انجام می‌ده)