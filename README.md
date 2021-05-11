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
برای ذخیره‌سازی اطلاعات در بستر Parse باید از ParseObject استفاده کنیم.
هر ParseObject، جفت هایی از Key-Value ها را شامل میشود.
توجه کنید که این Key-Value ها باید سازگار با JSON باشند.
بنابرین Key ها تنها میتوانند String باشند.
اما Value ها مقداری متفاوتی میتوانند داشته باشند مانند عدد، رشته، مقدار منطقی و حتی آبجکت و یا آرایه.
به شرط اینکه از شرایط JSON تبعیت کنند. مثلا آبجکت دوری نباشد.
هر ParseObject شامل یک ClassName نیز میشود تا Object ها به صورت دسته ای از هم تمییز یابند.

طبق استاندازد های جاوا و پارس، بهتر است که اسامی کلاس ها از استاندارد UpperCamelCase و کلید ها از استاندارد lowerCamelCase پیروی کنند.

به شکل زیر میتوانیم یک ParseObject بسازیم.

<div dir="ltr">

```java
ParseObject gameScore = new ParseObject("GameScore");
```
</div>

و همچنین میتوانیم مقادیری دلخواهی را به عنوان argument به شکل زیر به آن نسبت بدهیم.

<div dir="ltr">

```java
gameScore.put("score", 1337);
gameScore.put("playerName", "Sean Plott");
gameScore.put("cheatMode", false);
```
</div>

در نهایت با صدا کردن تابع `saveInBackground` میتوانیم آن را به سرور ارسال و در سرور ذخیره کنیم.

<div dir="ltr">

```java
gameScore.saveInBackground();
```
</div>

در نهایت در سرور، ابجکتی مشابه شکل زیر ذخیره خواهد شد.

<div dir="ltr">

```java
objectId: "xWMyZ4YEGZ",
score: 1337,
playerName: "Sean Plott",
cheatMode: false,
createdAt:"2011-06-10T18:33:42Z",
updatedAt:"2011-06-10T18:33:42Z"
```
</div>

همان گونه که Object را در سرور ذخیره کردیم، با داشتن objectId یک ابجکت و دانستن اینکه متغیر مورد نظر از کدام کلاس است، میتوانیم آن متغیر را از سرور دریافت کنیم.

<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.getInBackground("xWMyZ4YEGZ", new GetCallback<ParseObject>() {
    public void done(ParseObject object, ParseException e) {
        if (e == null) {
            // object will be your game score
        } else {
            // something went wrong
        }
    }
});
```
</div>

همچنین به سادگی میتوانیم، Value یک Key مورد نظر را در type دلخواه دریافت کنیم.

<div dir="ltr">

```java
int score = gameScore.getInt("score");
String playerName = gameScore.getString("playerName");
boolean cheatMode = gameScore.getBoolean("cheatMode");
```
</div>

جدا از Key-Value ها، 4 getter برای Field های ویژه تعریف شده اند.

<div dir="ltr">

```java
String objectId = gameScore.getObjectId();
Date updatedAt = gameScore.getUpdatedAt();
Date createdAt = gameScore.getCreatedAt();
ParseACL acl = gameScore.getACL();
```
</div>

توجه کنید که ممکن لازم شود ک object را بروزرسانی کنیم تا بروزترین اطلاعات در سرور را به دست آوریم.
به جای آنکه دوباره آن آبجکت را با کوئری مورد نظر از سرور دریافت کنیم، میتوانیم به سادگی از متد `fetchInBackground` استفاده کنیم و آن را روی آبجکت مورد نظر صدا کنیم.

<div dir="ltr">

```java
myObject.fetchInBackground(new GetCallback<ParseObject>() {
    public void done(ParseObject object, ParseException e) {
        if (e == null) {
            // Success!
        } else {
            // Failure!
        }
    }
});
```
</div>

توجه کنید که علی الرعم معماری انطباق پذیر با آسنکرون بودن تابع و استفاده از Callback، کد درون Callback روی ترد صدا کننده اجرا میشود.

در این SDK، یک local datastore تعبیه شده است که در آن اطلاعات روی خود دستگاه ذخیره می شود.
با ذخیره داده روی local datastore، چیزی به سرور ارسال نمیشود اما این datastore به طور ویژه برای ذخیره موقتی اطلاعاتی که بعدا به سرور ارسال خواهند شد مفید است.

اگر بخواهیم از این local datastore استفاده کنیم، باید که در زمان initialize کردن پارس، آن را فعال کنیم. به این صورت که پیش از صدا کردن تابع
`initialize`
در کلاس Application، تابع
`enableLocalDatastore`
را صدا بزنیم.

به سادگی و مشابه آنچه پیشتر دیدیم، میتوانیم ParseObject را روی Local Datastore ذخیره کنیم یا از Object های ذخیره شده روی آن، Query بگیریم.

<div dir="ltr">

```java
ParseObject gameScore = new ParseObject("GameScore");
gameScore.put("score", 1337);
gameScore.put("playerName", "Sean Plott");
gameScore.put("cheatMode", false);
gameScore.pinInBackground();
```
</div>

<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.fromLocalDatastore();
query.getInBackground("xWMyZ4YEGZ", new GetCallback<ParseObject>() {
    public void done(ParseObject object, ParseException e) {
        if (e == null) {
            // object will be your game score
        } else {
            // something went wrong
        }
    }
});
```
</div>

به همین سادگی نیز میتوانیم ابجکت ها را از local datastore حذف کنیم.

<div dir="ltr">

```java
gameScore.unpinInBackground();
```
</div>

یکی از مزایای Parse، مدیریت کردن شرایط بدون اینترنت است.
ممکن است در مواقعی، بخواهیم که یک object در اولین فرصت که شرایط انترنت مهیا شد، در سرور ذخیره شود.
در این شرایط میتوانیم از تابع `saveEventually` استفاده کنیم.
این تابع در اولین زمانی که اینترنت مهیا شود، ابجکت مورد نظر را به سرور ارسال میکند.
در این زمان اگر Application باز و بسته شود نیز اشکالی بوجود نمی آید.
اگر local datastore را در برنامه استفاده کنیم، object در زمانی که در انتظار برقراری اتصال اینترنت برای ارسال به سرور است، در local datastore ذخیره میشود.
توجه کنید که مشابه تابغ `saveEventually`، تابع `deleteEventually` نیز وجود دارد.

<div dir="ltr">

```java
ParseObject gameScore = new ParseObject("GameScore");
gameScore.put("score", 1337);
gameScore.put("playerName", "Sean Plott");
gameScore.put("cheatMode", false);
gameScore.saveEventually();
```
</div>

## Query در Parse

## کاربر در Parse 

## Role در Parse

## Session در Parse

## File در Parse
ParseFile به کاربر اجازه میدهد که در سرور فایل ذخیره یا از آن فایل دریافت کند.
توجه کنید که محتویات یک فایل را میتوانیم با یک ParseObject بفرستیم اما بسیار سنگین میشود و عملکرد خوبی نخواهد داشت.

ساخت ParseFile بسیار ساده است.
توجه کنید که هر فایل در سیستم، معادل یک رشته از byte است.
بنابرین میتوانیم هر فایلی در سیستم را به یک رشته بایت تبدیل کنیم.
متد سازنده ParseFile نیز اسم فایل و رشته از بایت های سازنده آن فایل را دریافت میکند.

<div dir="ltr">

```java
byte[] data = "This is a new file's contents".getBytes();
ParseFile file = new ParseFile("resume.txt", data);
```
</div>

توجه کنید که در مورد فایل ها دو نکته شایان ذکر است.

- نگرانی بابت تعارض اسم فایل ها با یک دیگر وجود ندارد.
  میتوان چند فایل با یک نام آپلود کرد.
  این موضوع توسط Parse با یک unique identifier که به هر فایل تعلق میگیرد، مدیریت میشود.
  
- اطمینان حاصل کنید که نام فایل دارای پسوند مناسب است تا Parse بتواند رفتار مناسب را با فایل انجام دهد.

مشابه قبل برای ذخیره یک فایل، متودهایی وجود دارد. (پیشتر در بخش ابجکت ها توضیح دادیم.) با توجه به نیاز میتوان از هر یک از متودهای `save` استفاده کرد.

<div dir="ltr">

```java
file.saveInBackground();
```
</div>

پس از آنکه عملیات ذخیره تکمیل شود، میتوان با فایل مانند مشابه یک object عادی برخورد کرد و حتی آن را درون ParseObject قرار داد.

<div dir="ltr">

```java
ParseObject jobApplication = new ParseObject("JobApplication");
jobApplication.put("applicantName", "Joe Smith");
jobApplication.put("applicantResumeFile", file);
jobApplication.saveInBackground();
```
</div>

همچنین مشابه قبل، میتوان با Query، فایل ها را دریافت کنیم.
به عنوان مثال فایل ذخیره شده در کد فوق را میتوانیم به شکل زیر دریافت کنیم.

<div dir="ltr">

```java
ParseFile applicantResume = (ParseFile) anotherApplication.get("applicantResumeFile");
applicantResume.getDataInBackground(new GetDataCallback() {
    public void done(byte[] data, ParseException e) {
        if (e == null) {
            // data has the bytes for the resume
        } else {
            // something went wrong
        }
    }
});
```
</div>

یکی از مزایای Parse این است که موضوع پیچیده ای مانند Progress در آن پیاده‌سازی شده است.
به سادگی در هر دو متود `saveInBackground` و `getDataInBackground` میتوانیم با پیاده‌سازی کلاس `ProccessCallback`، میتوانیم از وضعیت پیشرفت فرآیند آگاه شویم و با این آگاهی می توانیم مواردی مانند spinner و یا ... را بروزرسانی کنیم.

<div dir="ltr">

```java
byte[] data = "Working at Parse is great!".getBytes();
ParseFile file = new ParseFile("resume.txt", data);

file.saveInBackground(new SaveCallback() {
    public void done(ParseException e) {
        // Handle success or failure here ...
    }
}, new ProgressCallback() {
    public void done(Integer percentDone) {
        // Update your progress spinner here. percentDone will be between 0 and 100.
    }
});
```
</div>

## Local Datastore در Parse

</div>