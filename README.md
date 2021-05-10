<p align="center">
    <img src="https://blog.theodo.com/static/a7a48712e6f25b8f1f78ffc12ccd8413/46604/parse-1.png" height="250px" alt="parse server logo">
    <br/>
Parse Android SDK
    <br/>
برنامه سازی موبایل، دانشگاه صنعتی شریف
    <br/>
ارائه دهنده درس: جناب آقای امید جعفری نژاد
    <br/>
نویسندگان: آرین احدی نیا، شهاب حسینی مقدم، محمدحسین کاشانی جباری
</p>

<div dir="rtl">

## Parse Server چیست؟
پارس سرور یک Back-end آماده است که بسیاری از موارد پرکاربرد در یک وب سرویس را مانند ساز و کار های مربوط به حساب کاربری را پیاده‌سازی کرده است.
در عین حال پارس سرور قابلیت پیاده‌سازی منطق ویژه سازی شده را برای توسعه دهندگان محفوظ میدارد و توسعه دهندگان میتوانند منطق دلخواه خود را برای Web-Server در بستر پارس سرور پیاده‌سازی کنند.
برای اطلاع بیشتر، خوب است بدانید که Parse Server در بستر Node.JS پیاده‌سازی شده است.

## Parse Android SDK چیست؟
برای وصل شدن به Server های پیاده‌سازی شده با Parse، میتوانیم از راهکار های متفاوتی استفاده کنیم.
میتوانیم از HTTP request به صورت خام بهره بگیریم. یکی از مزایای Parse این است که برای اتصال به سرور، SDK هایی را در بستر های مختلف مانند Android در اختیار توسعه دهندگان قرار میدهد.
وجود چنین SDK هایی ارتباط بین Client و Server را بسیار ساده می کند.
در این نوشته، تمرکز ما بر روی Parse Android SDK و چگونگی به کار گرفتن آن در پروژه های اندرویدی است.

## ساختار نوشته 
در این نوشته سعی می کنیم به ساختار Documentation اصلی Parse Android SDK وفادار بمانیم تا ادامه مطلب در Document اصلی برای مخاطب امکان‌پذیر باشد.
تلاش بر این است که افراد بتوانند با خواندن این نوشته، Parse Android SDK را درک و در بتوانند در پروژه های خود استفاده اولیه ای از آن داشته باشند.
میتوانید از طریق
<a href="https://docs.parseplatform.org/parse-server/guide/">
این لینک
</a>
به مستندات Parse Server و از طریق
<a href="https://docs.parseplatform.org/android/guide/#relations">
این لینک
</a>
به مستندات Parse Android SDK دسترسی پیدا کنید.

## مزایای استفاده از Parse Server و SDK های آن چیست؟ 
در Parse Platform، یعنی Parse Server و SDK های آن، بسیاری از مباحث پیچیده مانند مسائل امنیتی، Caching، ارسال پیام از Server به کلاینت و ... در نظر گرفته شده است.
استفاده از این پلتفرم میتواند برنامه نویس را از پیاده‌سازی این موارد رها کند.
در مقایسه با رقبا، Parse Server از طرف شرکت بزرگی مانند Facebook حمایت میشود.
همچنین Parse Platform، متن-باز و به سادگی میتوان از طریق مخازن این پروژه در
<a href="https://github.com/parse-community">
GitHub
</a>
به Source Code پارس سرور و SDK ها دسترسی پیدا کنید.
همچنین Parse Platform دارای Community نسبتا قوی است که برای چنین پروژه ای مزیت مهمی محسوب میشود.

## چگونه Parse Android SDK را به پروژه خود اضافه کنیم؟

### مرحله اول

ابتدا اطمینان حاصل کنید که در مخزن jitpack در build.gradle پروژه (و نه ماژول) قرار دارد.

<div dir="ltr">

```
allprojects {
	repositories {
		...
		maven { url "https://jitpack.io" }
	}
}
```
</div>

### مرحله دوم 
وابستگی (dependency) مربوط به parse android sdk را به build.gradle ماژول اضافه کنید.

<div dir="ltr">

```
dependencies {
    implementation "com.github.parse-community.Parse-SDK-Android:parse:latest.version.here"
}
```
</div>

### مرحله سوم 
کلاس Application را در `AndroidManifest.xml` پروژه تعریف کنید.

<div dir="ltr">

```xml
 <application
   android:name=".App">
   ...
 </application>
```
</div>

### مرحله چهارم 
کلاس Application را پیاده‌سازی کنید و آن تابع `onCreate` را پیاده‌سازی کنید و در آن، در آن Parse را با استفاده از Context برنامه initialize کنید.
منظور از initialize کردن، مشخص کردن آدرس سرور هدف (Target Server) و مشخص کردن مقادیر لازم برای احراز هویت Application به Server است.

<div dir="ltr">

```java
import com.parse.Parse;
import android.app.Application;

public class App extends Application {
  @Override
  public void onCreate() {
    super.onCreate();
    Parse.initialize(new Parse.Configuration.Builder(this)
      .applicationId("YOUR_APP_ID")
      .clientKey("YOUR_CLIENT_KEY")
      .server("http://localhost:1337/parse/")
      .build()
    );
  }
}
```
</div>

اکنون Application در شروع برنامه، Parse را initial می کند و در ادامه در هر کجای برنامه میتوانید از طریق متود های static کلاس Parse، ارتباطات لازم را برقرار کنید.

## Object در Parse

## Query در Parse

## کاربر در Parse 

## Role در Parse

## Session در Parse

## File در Parse

## Local Datastore در Parse

</div>