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

### ذخیره object ها 
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

### دریافت object ها 
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

### بروز رسانی object ها 
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

### استفاده از حافظه محلی 
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

### ذخیره کردن object ها در حالت offline 
یکی از مزایای Parse، مدیریت کردن شرایط بدون اینترنت است.
ممکن است در مواقعی، بخواهیم که یک object در اولین فرصت که شرایط اینترنت مهیا شد، در سرور ذخیره شود.
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

### تغییر object ها 
ممکن است بخواهیم در ویژگی ها یک object تغییری ایجاد کنیم و آن را در سرور ارسال کنیم.
این کار به سادگی انجام میشود.
پس از دریافت object، میتوانیم key های مورد نظر را دوباره مقدار دهی کنیم.
مقدار دهی مجدد موجب تغییر مقدار میشود.
سپس ابجکت به دست آمده را مجددا ذخیره کنیم.

<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");

// Retrieve the object by id
query.getInBackground("xWMyZ4YEGZ", new GetCallback<ParseObject>() {
    public void done(ParseObject gameScore, ParseException e) {
        if (e == null) {
            // Now let's update it with some new data. In this case, only cheatMode and score
            // will get sent to your Parse Server. playerName hasn't changed.
            gameScore.put("score", 1338);
            gameScore.put("cheatMode", true);
            gameScore.saveInBackground();
        }
    }
});
```
</div>

پارس به صورت خودکار، فیلدهایی را که دچار تغییر شده اند، شناسایی میکند و تنها آنها را از طریق شبکه ارسال میکند.
بنابرین نیازی به نگرانی راجب به سربار شبکه نیست.

### شمارنده ها در Parse 
تصور کنید که دو کلاینت، همزمان میخواهند در سرور یک شمارنده را افزایش دهند.
فرض کنید مقدار این متغیر در هر دو کلاینت 100، است.
هر دو پس از یک واحد افزایش یه 101 میرسند.
سپس با ارسال درخواست به سرور مقدار نهایی 101 خواهد شد.
در Parse برای متغیر های عددی، متود increment تعبیه شده است.
با فراخوانی آن برای یک کلید مشخص در یک object، مقدار آن به صورت تضمین شده یک واحد افزایش پیدا می کند.

<div dir="ltr">

```java
gameScore.increment("score");
gameScore.saveInBackground();
```
</div>

همچنین می توانیم از فرم زیر استفاده کنیم تا increment را به مقدار دلخواه انجام دهیم.
توجه کنید که میتوانیم با increment به مقدار منفی، decrement را پیاده‌سازی کنیم.

<div dir="ltr">

```java
increment("key", "amount");
```
</div>

### آرایه ها
به سادگی می توانیم یک کلید را آرایه فرض کنیم و از توابع SDK برای کار با آرابه بهره بگیریم.

<div dir="ltr">

```java
add("key", object);
addAll("key", list);
addUnique("key", object);
addUniqueAll("key", list);
removeAll("key", object); // remove all instance of object given in array.
```
</div>

به عنوان مثال داریم:

<div dir="ltr">

```java
gameScore.addAllUnique("skills", Arrays.asList("flying", "kungfu"));
gameScore.saveInBackground();
```
</div>

### حذف یک کلید از Object
با استفاده از تابع remove، میتوانیم کلید مورد نظر را از سرور حذف کنیم. مثال:

<div dir="ltr">

```java
myObject.remove("playerName");
myObject.saveInBackground(); // Saves the field deletion to your Parse Server
```
</div>

مشابه آنچه پیشتر در بروزرسانی دیدیم، parse به صورت خودکار کشف میکند که کدام کلید حذف شده و همان را به سرور ارسال میکند.
نیازی به نگرانی در مورد سربار شبکه وجود ندارد.

### حذف ابجکت از سرور

به سادگی میتوانیم دستور حذف یک ابجکت را برای سرور به شکل زیر ارسال کنیم.

<div dir="ltr">

```java
myObject.deleteInBackground();
```
</div>

### Data Types
توجه کنید که به عنوان مقادیر مجاز یک کلید در ParseObject، میتوانیم از Type های زیر استفاده کنیم.
مقابل هر type، تبدیل آن را برای سریالیزه شدن نوشته ایم.

<div dir="ltr">

String => String <br/>
Number => primitive numeric values such as ints, doubles, longs, or floats <br/>
Bool => boolean <br/>
Array => JSONArray <br/>
Object => JSONObject <br/>
Date => java.util.Date <br/>
File => ParseFile <br/>
Pointer => other ParseObject <br/>
Relation => ParseRelation <br/>
Null => JSONObject.NULL <br/>
</div>

میتوانیم با استفاده از JSONObject و JSONArray، داده های پیچیده تری را درون یک ParseObject ذخیره کنیم.

### ارث بری از ParseObject
می توانیم، هنگام تعریف کلاس ها، از کلاس ParseObject ارث بری کنیم.
در این صورت، نیازی نیست که هر بار یک ParseObject از روی object مورد نظر بسازیم و بر روی آن متودهایی مانند `saveInBackground` و یا موارد مشابه را call کنیم.

به مثال زیر توجه کنید.
در حالت سنتی که پیش از این بیان کردیم، اینگونه عمل میکردیم.

<div dir="ltr">

```java
ParseObject shield = new ParseObject("Armor");
shield.put("displayName", "Wooden Shield");
shield.put("fireproof", false);
shield.put("rupees", 50);
shied.saveInBackground();
```
</div>

در روش جدید، کلاس Armor را از ParseObject، گسترش میدهیم، (extend میکنیم) و به این صورت عمل می کنیم.

<div dir="ltr">

```java
Armor shield = new Armor();
shield.setDisplayName("Wooden Shield");
shield.setFireproof(false);
shield.setRupees(50);
shied.saveInBackground();
```
</div>

مزیت این روش این است که مدل و ParseObject با یکدیگر ادغام میشوند و حجم کد کاهش می یابد.

برای اینکه یک زیرکلاس از ParseObject درست کنیم، مراحل زیر را باید طی کنیم.

- از کلاس ParseObject، یک کلاس extend کنیم.

- با استفاده از Annotation، نام کلاس ParseObject را، که پیش از این در کانستراکتور ParseObject وارد میکردیم، مشخص کنیم.

- اطمینان حاصل کنید که کلاس مورد نظر یک constructor با صفر پارامتر ورودی دارد. این کانستراکتور میتواند Default Constructor باشد، هیچ یک از Field های parse object را نباید در آن تغییر دهیم.
  نبود پارامتر در کانستراکتور را می توانیم با مقداردهی value ها با setter جبران کنیم.
  یا میتوانیم از الگوهایی مانند Builder استفاده کنیم.

- در نهایت باید در کلاس Application پیش از initialize کردن Parse، با استفاده از متود registerSubclass، کلاس مورد نظر را به عنوان یک زیرکلاس از ParseObject به Parse بشناسانیم.

یک مثال ساده از فرایند فوق را می توانید مشاهده کنید.

<div dir="ltr">

```java
import com.parse.ParseObject;
import com.parse.ParseClassName;

@ParseClassName("Armor") 
public class Armor extends ParseObject {
}
```
</div>

<div dir="ltr">

```java
import com.parse.Parse;
import android.app.Application;

public class App extends Application { 
    @Override 
    public void onCreate() { 
        super.onCreate();
        
        ParseObject.registerSubclass(Armor.class);
        Parse.initialize(this);
    }
}
```
</div>

توجه کنید که تعریف کردن Field در ParseObject بی معنی است.
همچنان باید با استفاده از همان داده ساختار Key-Value کار ها را انجام دهیم.
اما به سادگی می توانیم getter و setter تعریف کنیم.

<div dir="ltr">

```java
@ParseClassName("Armor")
public class Armor extends ParseObject { 
    public String getDisplayName() { 
        return getString("displayName"); 
    }
    
    public void setDisplayName(String value) { 
        put("displayName", value); 
    }
}
```
</div>

اکنون که getter و setter داریم، میتوانی رفتار مشابه field با آنها داشته باشیم.

همچنین با استفاده از متود getQuery نیز میتوانیم از query مورد نظر را از زیرکلاس تعریف شده از سرور دریافت کینم.

<div dir="ltr">

```java
ParseQuery<Armor> query = ParseQuery.getQuery(Armor.class);
query.whereLessThanOrEqualTo("rupees", ParseUser.getCurrentUser().get("rupees"));
query.findInBackground(new FindCallback<Armor>() {
    @Override
    public void done(List<Armor> results, ParseException e) {
        for (Armor a : results) {
            // ...
        }
    }
});
```
</div>

### Object های مرتبط

توجه کنید که ممکن است دو object به یک دیگر مرتبط باشند. به این صورت که یکی از آنها لینکی به دیگری داشته باشد. به این حالت relation میگوییم.

به عنوان مثال، در یک وبلاگ، کامنت ها مربوط به یک Post مشخص هستند.

بنابرین می توانیم مشابه مثال زیر، یک آبجکت را در یک آبجکت دیگر قرار دهیم.

<div dir="ltr">

```java
// Create the post
ParseObject myPost = new ParseObject("Post");
myPost.put("title", "I'm Hungry");
myPost.put("content", "Where should we go for lunch?");

// Create the comment
ParseObject myComment = new ParseObject("Comment");
myComment.put("content", "Let's do Sushirrito.");

// Add a relation between the Post and Comment
myComment.put("post", myPost);

// This will save both myPost and myComment
myComment.saveInBackground();
```
</div>

در نهایت با سیو کردن کامنت در دیتابیس، پست نیز در دیتابیس ذخیره میشود.

ممکن است که ابجکت را در حافظه نداشته باشیم.
در این صورت میتوانیم با استفاده از objectId نیز به شکل زیر ارتباط را برقرار کنیم.

<div dir="ltr">

```java
// Add a relation between the Post with objectId "1zEcyElZ80" and the comment
myComment.put("post", ParseObject.createWithoutData("Post", "1zEcyElZ80"));
```
</div>

مثال بالا را در نظر بگیرید، به صورت کلی با دریافت کامنت از سرور، پست مربوط به آن دریافت نمیشود مگر اینکه از تابع `fetchIfNeededInBackground` استفاده کنیم.
الگوی استفاده از این تابع به شکل زیر است.

<div dir="ltr">

```java
fetchedComment.getParseObject("post").fetchIfNeededInBackground(new GetCallback<ParseObject>() {
    public void done(ParseObject post, ParseException e) {
        String title = post.getString("title");
        // Do something with your new title variable
        // }
});
```
</div>

در این حالت به دلیل استفاده ای که از post میکنیم، parse به صورت خودکار post را از سرور دریافت خواهد کرد.

برای حذف ارتباط، کافی است که کلید سازنده ارتباط را در key-value های object حذف کنیم.

<div dir="ltr">

```java
comment.remove(post);
```
</div>

## کوئری در Parse 

مشابه کوئری های متداول از دیتابیس، میتوانیم Constraint های متفاوتی بر روی دیتا قرار دهیم.
در پارس نیز میتوانیم انواع این شروط را بر روی کوئری برای رسیدن به نتیجه دلخواه قرار دهیم.
مشابه آنچه پیشتر در بخش ابجکت نشان دادیم، کوئری از به صورت Async دریافت میشود.
برای اینکه کوئری های دلخواهی را انجام دهیم، از تابع `findInBackground` استفاده میکنیم.
الگوی کلی کوئری ها در پارس به شکل زیر است.

<div dir="ltr">

```java
// Declaration of Object
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
// Constraints
query.whereEqualTo("playerName", "Dan Stemkoski");
// Run Query
query.findInBackground(new FindCallback<ParseObject>() {
    public void done(List<ParseObject> scoreList, ParseException e) {
        if (e == null) {
            Log.d("score", "Retrieved " + scoreList.size() + " scores");
        } else {
            Log.d("score", "Error: " + e.getMessage());
        }
    }
});
```
</div>

توجه کنید که نام ابجکت مانند نام جدول در دیتابیس است.
برای ابجکت های برگشتی، شرایط متفاوتی میتوانیم بزاریم.
مانند آنکه ستونی برابر مقداری دلخواه باشد یا نباشد و موارد متعدد دیگر.
اکنون به تفکیک، برخی از موارد را نشان میدهیم.

میتوانیم برای اینکه ستونی برابر ستونی دیگر باشد یا نباشد، از تابع زیر استفاده کنیم.

<div dir="ltr">

```java
query.whereEqualTo("playerName", "Michael Yabuti");
query.whereNotEqualTo("playerName", "Michael Yabuti");

```
</div>

میتوانیم تعداد ایتم های برگشتی را محدود کنیم.

<div dir="ltr">

```java
query.setLimit(10); // limit to at most 10 results
```
</div>

همچنین میتوانیم مشخص کنیم که از لیست به دست آمده، تعدادی دلخواه از اول لیست نادیده گرفته شوند.

<div dir="ltr">

```java
query.setSkip(10); // skip the first 10 results
```
</div>

برای مواردی که قابل مرتب سازی و قابل مقایسه هستند، میتوانیم از چنین توابعی برای محدود کردن یا مرتب کردن کوئری نیز استفاده کنیم.
از تابع زیر میتوانیم برای مرتب کردن کوئری به ترتیب صعودی یا نزولی استفاده کنیم.

<div dir="ltr">

```java
query.orderByAscending("score");
query.orderByDescending("score");
```
</div>

همچنین می توانیم مشخص کنیم که درصورت برابر بودن مقدار مشخص شده برای مرتب‌سازی، پارس به سراغ الویت دوم یا n ام برود.

<div dir="ltr">

```java
query.addAscendingOrder("score");

query.addDescendingOrder("score");
```
</div>

همچنین میتوانیم به شکل زیر از عملیات های مقایسه ای استفاده کنیم.

<div dir="ltr">

```java
query.whereLessThan("wins", 50);

query.whereLessThanOrEqualTo("wins", 50);

query.whereGreaterThan("wins", 50);

query.whereGreaterThanOrEqualTo("wins", 50);
```
</div>

تصور کنید که میخواهیم از کوئری های مربوط به سه نام متفاوت، اجتماع بگیریم. در این حالت میتوانیم با استفاده از تابع زیر مشخص کنیم که نام ابجکت برگشتی، حتما باید عضو لیست زیر باشد.

<div dir="ltr">

```java
String[] names = {"Jonathan Walsh", "Dario Wunsch", "Shawn Simon"};
query.whereContainedIn("playerName", Arrays.asList(names));
```
</div>

<div dir="ltr">

```java
String[] names = {"Jonathan Walsh", "Dario Wunsch", "Shawn Simon"};
query.whereNotContainedIn("playerName", Arrays.asList(names));
```
</div>


### استفاده از Datastore محلی

اگر شما Datastore محلی را با صدا زدن تابع `()Parse.enableLocalDatastore` قبل از صدا زدن تابع `()Parse.initialize` فعال کرده باشید، سپس شما می‌توانید از کوئری روی آبجکت‌های ذخیره‌شده روی دستگاهتان استفاده کنید. برای انجام این کار، تابع `fromLocalDatastore` را روی کوئری صدا بزنید.

<div dir="ltr">

```java
query.fromLocalDatastore();
query.findInBackground(new FindCallback<ParseObject>() {
  public void done(final List<ParseObject> scoreList, ParseException e) {
    if (e == null) {
      // Results were successfully found from the local datastore.
    } else {
      // There was an error.
    }
  }
});
```
</div>

استفاده از کوئری در Datastore محلی، همانند استفاده از آن روی شبکه است و نتایج شامل تمامی اشیای مورد نظر کوئری می‌باشد. در این حالت کوئری حتی تغییرات ذخیره‌نشده روی شی را در نظر می‌گیرد.

### ذخیره‌کردن کوئری

#### با استفاده از Datastore محلی

اکثرا مفید است که نتیجه کوئری را روی دستگاه ذخیره کنید. چون به شما اجازه می‌دهد که زمانی که کاربر آفلاین است، اطلاعات را به نمایش بگذارید یا زمانی که پاسخ درخواست‌های شبکه هنوز برنگشته است. آسان‌ترین راه برای انجام این کار استفاده از Datastore محلی است. شما می‌توانید با برچسب اشیا را پین کنید که این کار به شما اجازه می‌دهد که یک دسته از اشیا را هم‌زمان با هم مدیریت کنید. برای این کار می‌توانید یک برچسب را به تابع `pinAllInBackground` بدهید.

<div dir="ltr">

```java
final String TOP_SCORES_LABEL = "topScores";

// Query for the latest objects from Parse.
query.findInBackground(new FindCallback<ParseObject>() {
  public void done(final List<ParseObject> scoreList, ParseException e) {
    if (e != null) {
      // There was an error or the network wasn't available.
      return;
    }

    // Release any objects previously pinned for this query.
    ParseObject.unpinAllInBackground(TOP_SCORES_LABEL, scoreList, new DeleteCallback() {
      public void done(ParseException e) {
        if (e != null) {
          // There was some error.
          return;
        }

        // Add the latest results for this query to the cache.
        ParseObject.pinAllInBackground(TOP_SCORES_LABEL, scoreList);
      }
    });
  }
});
```
</div>

اکنون با دادن کوئری به دستور `fromLocalDatastore`، این اشیا در صورت هم‌خوانی با کوئری در نتایج این تابع ظاهر خواهند شد. `ParseQuery` به شما اجازه می‌دهد که از شبکه و از Datastore محلی به صورت زنجیری یا موازی کوئری بگیرید.

<div dir="ltr">

```java
final ParseQuery query = ParseQuery.getQuery("GameScore");
query.fromLocalDatastore().findInBackground().continueWithTask((task) -> {
  // Update UI with results from Local Datastore ...
  ParseException error = task.getError();
  if(error == null){
    List<ParseObject> gameScore = task.getResult();
    for(ParseObject game : gameScore){
        //...
    }
  }
  // Now query the network:
  return query.fromNetwork().findInBackground();
}, Task.UI_EXECUTOR).continueWithTask((task) -> {
  // Update UI with results from Network ...
  ParseException error = task.getError();
  if(error == null){
    List<ParseObject> gameScore = task.getResult();
    for(ParseObject game : gameScore){
        //...
    }
  }
  return task;
}, Task.UI_EXECUTOR);
```
</div>

#### بدون استفاده از Datastore محلی

به طور پیش‌فرض، کوئری از cache استفاده نمی‌کند اما شما می‌توانید آن رفتار را با تابع `setCachePolicy` تغییر دهید. برای مثال زمانی که شبکه در دسترس نیست و می‌خواهید از اطلاعات ذخیره‌شده پیش از کوئری استفاده کنید.

<div dir="ltr">

```java
query.setCachePolicy(ParseQuery.CachePolicy.NETWORK_ELSE_CACHE);
query.findInBackground(new FindCallback<ParseObject>() {
  public void done(List<ParseObject> scoreList, ParseException e) {
    if (e == null) {
      // Results were successfully found, looking first on the
      // network and then on disk.
    } else {
      // The network was inaccessible and we have no cached data
      // for this query.
    }
  }
});
```
</div>

ParseQuery توابعی را در اختیار شما قرار داده است که با کمک آن می‌توانید رفتار با cache را مدیریت کنید.

### شمردن اشیا

اگر لازم دارید که بدانید چه تعدادی از اشیا با کوئری هم‌خوانی دارند و به خود اشیا نیازی ندارید، شما می‌توانید که از تابع `count` به جای تابع `find` استفاده کنید.


<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.whereEqualTo("playerName", "Sean Plott");
query.countInBackground(new CountCallback() {
  public void done(int count, ParseException e) {
    if (e == null) {
      // The count request succeeded. Log the count
      Log.d("score", "Sean has played " + count + " games");
    } else {
      // The request failed
    }
  }
});
```
</div>

اگر می‌خواهید که ترد صدازننده را بلاک کنید، می‌توانید از تابع سنکرون `()query.count` استفاده کنید.

### کوئری‌های ترکیبی

اگر می‌خواهید آبجکت‌هایی را بیابید که با حداقل یکی از چند کوئری مد نظر شما هم‌خوانی داشته باشند، کافیست که از تابع `ParseQuery.or` برای ساختن آن کوئری استفاده کنید.

<div dir="ltr">

```java
ParseQuery<ParseObject> lotsOfWins = ParseQuery.getQuery("Player");
lotsOfWins.whereGreaterThan(150);

ParseQuery<ParseObject> fewWins = ParseQuery.getQuery("Player");
fewWins.whereLessThan(5);

List<ParseQuery<ParseObject>> queries = new ArrayList<ParseQuery<ParseObject>>();
queries.add(lotsOfWins);
queries.add(fewWins);

ParseQuery<ParseObject> mainQuery = ParseQuery.or(queries);
mainQuery.findInBackground(new FindCallback<ParseObject>() {
  public void done(List<ParseObject> results, ParseException e) {
    // results has the list of players that win a lot or haven't won much.
  }
});
```
</div>

شما می‌تواند از عملگرهای دیگری همانند `and` به جای `or` برای ساختن کوئری جدید استفاده کنید.

## کاربر در Parse 
در هسته بسیاری از برنامه ها، ایدهٔ حساب های کاربری وجود دارد که به کاربران اجازه میدهند به شیوه ای ایمن به اطلاعاتشان دسترسی داشته باشند.
ما یک کلاس مخصوص user به نام ParseUser ارائه می‌دهیم که به صورت خودکار بسیاری از کار های لازم برای مدیریت حساب کاربری را به دست میگیرد.

ParseUser زیرکلاسی از ParseObject است و همان ویژگی ها را دارد.
با این کلاس، شما میتوانید عملکرد حساب کاربری را به برنامه‌تان اضافه کنید.

### خصوصیات ParseUser

ParseUser چند خصوصیت دارد که آن را از ParseObject متمایز میسازد:

- username: نام کاربری برای کاربر (الزامی)

- password: رمز عبور برای کاربر (مورد نیاز در هنگام ثبت نام)

- email: آدرس ایمیل برای کاربر (اختیاری)

بعدا هنگامی که وارد موارد استفاده مختلف برای کاربران شدیم، هر یک از این ها را به تفصیل شرح میدهیم.
به خاطر داشته باشید که اگر شما username و email را با استفاده از setter ها ثبت کنید، دیگر نیازی نیست که با متد put آنها را ثبت کنید.

### ثبت نام
اولین کاری که برنامه شما احتمالا انجام میدهد درخواست ثبت نام از کاربر است.
کدی که در ادامه آمده یک ثبت نام معمولی را نشان می‌دهد:

<div dir="ltr">

```java
ParseUser user = new ParseUser();
user.setUsername("my name");
user.setPassword("my pass");
user.setEmail("email@example.com");

// other fields can be set just like with ParseObject
user.put("phone", "650-253-0000");

user.signUpInBackground(new SignUpCallback() {
    public void done(ParseException e) {
        if (e == null) {
            // Hooray! Let them use the app now.
        } else {
            // Sign up didn't succeed. Look at the ParseException
            // to figure out what went wrong
        }
    }
});
```
</div>
این درخواست به صورت آسنکرون یک کاربر جدید در برنامه parse شما ایجاد خواهد کرد.
قبل از این که این کار را انجام دهد، چک میکند که مطمئن شود که هم username و هم email یکتا باشند.
همچنین به صورت ایمن پسورد را با استفاده از bcrypt در فضای ابری hash میکند.
رمز عبور ها هرگز به صورت متن ساده ذخیره نمی شوند و همینطور هرگز رمز به صورت متن ساده به کلاینت مخابره نمیشود.

به خاطر داشته باشید که ما از متد signUpInBackground استفاده کردیم، نه از متد saveInBackground.
همیشه باید ParseUser های جدید با متد signUpInBackground(یا signUp) ساخته شوند.
بروزرسانی های بعدی برای یک کاربر میتواند با فراخوانی save انجام شود.

متد signUpInBackground به صورت های مختلفی در دسترس است.
با قابلیت بازگردانی خطا ها، و همچنین نسخه های همگام.
طبق معمول، ما به شدت توصیه میکنیم که از نسخه های آسنکرون استفاده کنید، تا UI برنامه شما را متوقف نکند.
شما میتوانید در مورد این متد های خاص در
[داک های API](https://docs.parseplatform.org/android/guide/)
ما اطلاعات بیشتری بدست آورید.

اگر یک ثبت نام موفقیت آمیز نباشد، میتوانید error object ی که بازگردانده میشود را بخوانید.
مورد بسیار محتمل این است که نام کاربری یا ایمیل در حال حاضر توسط کاربر دیگری در حال استفاده است.
شما باید این مورد را به صورت واضح به کاربر مخابره کنید، و از ایشان بخواهید که یک نام کابری متفاوت را امتحان کنند.

شما آزادید که از یک آدرس ایمیل به عنوان نام کاربری استفاده کنید.
به سادگی از کاربران درخواست کنید که ایمیلشان را وارد کنند، ولی آن را در محل username بنویسند - ParseUser به حالت معمول رفتار خواهد کرد.
در خصوص این که این مورد چگونه مدیریت میشود در مبحث بازنشانی رمز عبور صحبت میکنیم.

### ورود

بعد از این که به کاربران اجازه دادید که ثبت نام کنند، شما نیاز دارید به آنها اجازه دهید که در آینده به حساب هایشان وارد شوند.
برای انجام این کار، شما میتوانید از متد logInInBackground استفاده کنید:

<div dir="ltr">

```java
ParseUser.logInInBackground("Jerry", "showmethemoney", new LogInCallback() {
    public void done(ParseUser user, ParseException e) {
        if (user != null) {
            // Hooray! The user is logged in.
        } else {
            // Signup failed. Look at the ParseException to see what happened.
        }
    }
});
```
</div>

### تایید کردن ایمیل

فعال کردن تایید ایمیل در تنظیمات یک برنامه به برنامه این امکان را میدهد که بخشی از تجربیات را صرفا به کاربران با ایمیل تایید شده اختصاص دهد.
تایید کردن ایمیل کلید emailVerified را به آبجکت ParseUser اضافه میکند.
وقتی که email یک ParseUser تنظیم یا عوض شود، emailVerified به false تغییر میکند.
سپس پارس یک لینک برای کاربر ارسال میکند که emailVerified را به true تغییر خواهد داد.

سه حالت برای emailVerified وجود دارد:

1- true:
کاربر ایمیلش را با کلیک کردن روی لینکی که پارس برایش ارسال کرده تایید کرده است.
ParseUser ها در ابتدای ساختن حسابشان نمیتوانند این مقدار را داشته باشند.

2- false:
آخرین باری که آبجکت ParseUser گرفته شده، کاربر ایمیلش را تایید نکرده بوده است.
اگر emailVerified مقدار false دارد، فراخوانی ()fetch روی ParseUser را در نظر بگیرید.

3- missing:
آبجکت ParseUser وقتی ایجاد شده که تایید ایمیل خاموش بوده یا ParseUser ایمیلی ندارد.

### کاربر کنونی

شما میتوانید با استفاده از آبجکت currentUser کش شده کاربر را بدون نیاز به log in دوباره وارد برنامه کنید.

هرگاه که شما از هر روش ثبت نام یا ورود استفاده کنید، کاربر روی حافظه دیسک کش میشود.
شما متیوانید با این کش به عنوان نشست رفتار کنید، و به صورت خودکار تصور کنید که کاربر وارد شده است:

<div dir="ltr">

```java
ParseUser currentUser = ParseUser.getCurrentUser();
if (currentUser != null) {
    // do stuff with the user
} else {
    // show the signup or login screen
}
```
</div>

شما میتوانید کاربر کنونی را با logOut پاک کنید:

<div dir="ltr">

```java
ParseUser.logOut();
ParseUser currentUser = ParseUser.getCurrentUser(); // this will now be null
```
</div>

### کاربران ناشناس

گاهی اوفات شما میخواهید که بتوانید داده و آبجکت ها را به کاربران منحصر بفرد پیوند دهید بدون این که کاربر را مجبور به انتخاب نام کاربری و رمز عبور کنید.

یک کاربر ناشناس کاربری است که میتواند بدون نام کاربری و رمز عبور ساخته شود اما همچنان تمام ظرفیت ها را همچون سایر ParseUser ها دارد. 
بعد از خروج از حساب، یک حساب ناشناس ترک میشود، و داده هایش دیگر قابل دسترسی نیستند.

شما میتوانید یک حساب کاربری ناشناس را با استفاده از ParseAnonymousUtils بسازید:

<div dir="ltr">

```java
ParseAnonymousUtils.logIn(new LogInCallback() {
    @Override
    public void done(ParseUser user, ParseException e) {
        if (e != null) {
            Log.d("MyApp", "Anonymous login failed.");
        } else {
            Log.d("MyApp", "Anonymous user logged in.");
        }
    }
});
```
</div>

شما میتوانید یک کاربر ناشناس را با ثبت نام کاربری و رمز عبور و فراخوانی
()signUp
به یک کاربر معمولی تبدیل کنید، یا با ورود یا اتصال به یک سرویس همانند Facebook و Twitter این کار را انجام دهید.
کاربر تبدیل شده میتواند تمام داده هایش را نگه دارد.
برای مشخص کردن این که آیا کاربر کنونی ناشناس است یا خیر میتوانید
()ParseAnonymousUtils.isLinked
را بررسی کنید.

<div dir="ltr">

```java
if (ParseAnonymousUtils.isLinked(ParseUser.getCurrentUser())) {
    enableSignUpButton();
} else {
    enableLogOutButton();
}
```
</div>

کاربران ناشناس همچنین میتوانند به صورت خودکار بدون نیاز به درخواست اینترنت برای شما ساخته شوند،در نتیجه شما میتوانید کارتان را با حسابتان بلافاصله وقتی برنامه آغاز میشود شروع کنید.
وقتی که ساختن خودکار حساب ناشناس را در هنگام آغاز برنامه فعال کنید،
()ParseUser.getCurrentUser
هرگز null نخواهد بود.
حساب کاربری به صورت خودکار در فضای ابری ذخیره خواهد شد در اولین وقتی که کاربر یا هر آبجکت مربوط به آن ذخیره شود.
تا آن نقطه، ID آبجکت حساب کاربری null خواهد بود.
فعال کردن ایجاد حساب خودکار، پیوند داده ها با کاربرانتان را بی دردسر خواهد نمود.
به عنوان مثال، در متد
()Application.onCreate
شما میتوانید بنویسید:

<div dir="ltr">

```java
ParseUser.enableAutomaticUser();
ParseUser.getCurrentUser().increment("RunCount");
ParseUser.getCurrentUser().saveInBackground();
```
</div>

### مشخص کردن کاربر کنونی

اگر شیوه هویت سنجی خودتان را استفاده میکنید یا میخواهید از طرف سرور وارد یک حساب کاربری شوید، میتوانید session token را به client ارسال کنید و از متد become استفاده کنید.
این متد قبل از تعیین کردن کاربر کنونی، اطمینان حاصل میکند که token معتبر است.

<div dir="ltr">

```java
ParseUser.becomeInBackground("session-token-here", new LogInCallback() {
    public void done(ParseUser user, ParseException e) {
        if (user != null) {
            // The current user is now set to user.
        } else {
            // The token could not be validated.
        }
    }
});
```
</div>

### امنیت برای objectهای user

کلاس ParseUser به صورت پیش فرض ایمن است.
داده ذخیره شده در یک ParseUser صرفا توسط همان کاربر قابل تغییر است.
به صورت پیش فرض، داده میتواند توسط هر کلاینتی خوانده شود.
از این روی، برخی آبجکت های ParseUser تصدیق میشوند و میتوان آنها را اصلاح کرد، درحالی که برخی دیگر صرفا قابل خواندن هستند.

به طور ویژه، شما قادر نیستید که هیچ یک از انواع متد های save یا delete را فراخوانی کنید، مگر اینکه ParseUser را با یکی از روش های اعتبارسنجی به دست آورده باشید، همچون logIn یا signUp.
این روش اطمینان حاصل میکند که تنها خود کاربر میتواند اطلاعاتش را تغییر دهد.

کد زیر این سیاست امنیتی را نشان میدهد:

<div dir="ltr">

```java
ParseUser user = ParseUser.logIn("my_username", "my_password");
user.setUsername("my_new_username"); // attempt to change username
user.saveInBackground();// This succeeds, since the user 
// was authenticated on the device

// Get the user from a non-authenticated manner
ParseQuery<ParseUser> query = ParseUser.getQuery();
query.getInBackground(user.getObjectId(), new GetCallback<ParseUser>() {
    public void done(ParseUser object, ParseException e) {
        object.setUsername("another_username");

        // This will throw an exception, since the ParseUser is not authenticated
        object.saveInBackground();
    }
});
```
</div>

ParseUser ای که از ()getCurrentUser گرفته میشود همیشه اعتبارسنجی شده است.

اگر نیاز دارید که چک کنید آیا یک ParseUser اعتبارسنجی شده است یا خیر، میتوانید از متد ()isAuthenticated استفاده کنید.
نیازی نیست که آبجکت های ParseUser ای که از طریق یک روش اعتبارسنجی بدست آمده اند را با این متد بررسی کنید.

### امنیت برای سایر object ها
همان مدلی که برای ParseUser ها اعمال میشد میتواند برای سایر object ها نیز اعمال شود.
برای هر object ای میتوانید مشخص کنید که کدام کاربران اجازه دارند که آن را بخوانند و کدام کاربران مجازند که آن را اصلاح کنند.
برای پشتیبانی از این نوع امنیت، هر آبجکت یک
[لیست مدیریت دسترسی](https://en.wikipedia.org/wiki/Access_control_list)
دارد، که با ParseACL پیاده سازی شده است.

ساده ترین راه برای استفاده از یک ParseACL مشخص کردن این است که یک object میتواند تنها توسط یک کاربر خوانده یا نوشته شود.
برای ساختن چنین objectی، ابتدا باید یک ParseUser وارد شده باشد.
سپس، new ParseACL(user) یک ParseACL میسازد که دسترسی به آن کاربر را محدود میکند.
لیست مدیریت دسترسی (ACL) یک object وقتی که آن object ذخیره شود به روز رسانی میشود.
از این روی، برای ساختن یک یادداشت خصوصی که تنها توسط کاربر کنونی قابل دسترسی باشد چنین میکنیم:

<div dir="ltr">

```java
ParseObject privateNote = new ParseObject("Note");
privateNote.put("content", "This note is private!");
privateNote.setACL(new ParseACL(ParseUser.getCurrentUser()));
privateNote.saveInBackground();
```
</div>

این یادداشت تنها برای کاربر کنونی در دسترس خواهد بود، همچنین روی تمام دستگاه هایی که این کاربر در آنها وارد حسابش شده است نیز قابل دسترسی است.
این قابلیت برای برنامه هایی که میخواهند قابلیت دسترسی به اطلاعات کاربر را در بین چند دستگاه داشته باشند مفید است، مثل برنامه یادداشت شخصی.

مجوز ها را میتوانید طی یک روال برای هر کاربر بدهید.
میتوانید اجازه ها را تک به تک به یک ParseACL با استفاده از setReadAccess و setWriteAccess بدهید.
برای مثال فرض کنید که یک پیغام دارید که به یک گروه با اعضای متعدد فرستاده خواهد شد و تمام اعضا حق خواندن و پاک کردن آن را دارند:

<div dir="ltr">

```java
ParseObject groupMessage = new ParseObject("Message");
ParseACL groupACL = new ParseACL();

// userList is an Iterable<ParseUser> with the users we are sending this message to.
for (ParseUser user : userList) {
    groupACL.setReadAccess(user, true);
    groupACL.setWriteAccess(user, true);
}

groupMessage.setACL(groupACL);
groupMessage.saveInBackground();
```
</div>

همچنین میتوانید با setPublicReadAccess و setPublicWriteAccess به همه کاربر ها به صورت یکجا اجازه دهید.
این اجازه میدهد الگوهایی مانند ارسال کامنت را برای پیام ها ایجاد کنید.
برای مثال، جهت ایجاد یک پست که تنها میتواند توسط نویسنده اش اصلاح شود، اما همه میتوانند آن را بخوانند داریم:

<div dir="ltr">

```java
ParseObject publicPost = new ParseObject("Post");
ParseACL postACL = new ParseACL(ParseUser.getCurrentUser());
postACL.setPublicReadAccess(true);
publicPost.setACL(postACL);
publicPost.saveInBackground();
```
</div>

برای کمک به این که مطمئن باشید داده ی کاربرانتان به صورت پیش فرض ایمن است، میتوانید یک ACL پیش فرض را به تمام ParseObject های جدید اعمال کنید:

<div dir="ltr">

```java
ParseACL.setDefaultACL(defaultACL, true);
```
</div>

در کد بالا پارامتر دوم تابع به Parse اطلاع میدهد که مطمئن شود که ACL پیش فرض که هنگام ساخته شدن object محول شده است در آن زمان به کاربر کنونی اجازه خواندن و نوشتن میدهد.
بدون این تنظیمات، شما نیاز دارید که ACL پیش فرض را هر دفعه که یک کاربر وارد یا خارج میشود reset کنید تا کاربر کنونی دسترسی های مناسب داشته باشد.
با این تنظیمات، شما میتوانید از تغییرات روی کاربر کنونی چشم پوشی کنید تا وقتی که به طور صریح نیاز داشته باشید که دسترسی های متفاوتی اعطا کنید.

ACL های پیش فرض ساختن برنامه هایی که روال های دسترسی معمول دارند را ساده میکند.
به عنوان مثال برنامه ای چون Twitter، که محتوای کاربر به طور عمومی برای جهان قابل مشاهده است، میتواند یک default ACL را به این صورت تنظیم کند:

<div dir="ltr">

```java
ParseACL defaultACL = new ParseACL();
defaultACL.setPublicReadAccess(true);
ParseACL.setDefaultACL(defaultACL, true);
```
</div>

برای برنامه ای مثل DropBox، که داده ی کاربر تنها برای خودش قابل دسترسی است مگر این که به صراحت اجازه داده شده باشد، شما میتوانید از یک default ACL استفاده کنید که تنها به کاربر کنونی دسترسی دهد:

<div dir="ltr">

```java
ParseACL.setDefaultACL(new ParseACL(), true);
```
</div>

برنامه ای که در Parse، داده را Log میکند ولی به هیچ کاربری اجازه دسترسی به آن داده را نمیدهد میتواند با ارائه ی یک ACL منع کننده، دسترسی کاربر کنونی را منع کند:

<div dir="ltr">

```java
ParseACL.setDefaultACL(new ParseACL(), false);
```
</div>

عملیات هایی که ممنوع هستند، همچون پاک کردن object ی که اجازه نوشتن روی آن را ندارید، منجر به دریافت خطای ParseException.OBJECT_NOT_FOUND میشوند.
برای اهداف امنیتی، این مانع میشود که کاربران تشخیص دهند که کدام object id ها وجود دارند اما ایمن شده اند، و کدام ها اصلا وجود ندارند.
### بازنشانی رمز عبور
کتابخانه ما یک راه برای کاربران ارائه میدهد که رمز عبورشان را به طور امن بازنشانی کنند.

برای شروع جریان بازنشانی رمز عبور، از کاربر بخواهید که آدرس ایمیلش را وارد کند، سپس این کد را فراخوانی کنید:

<div dir="ltr">

```java
ParseUser.requestPasswordResetInBackground("myemail@example.com", new RequestPasswordResetCallback() {
    public void done(ParseException e) {
        if (e == null) {
        // An email was successfully sent with reset instructions.
        } else {
        // Something went wrong. Look at the ParseException to see what's up.
        }
    }
});
```
</div>

این کد در بین نام های کاربری یا ایمیل ها به دنبال ایمیلی که از کاربر دریافت کرده میگردد و برای آن ایمیل بازیابی رمز عبور میفرستد.
با این کار شما میتوانید انتخاب کنید که از ایمیل کاربر به عنوان نام کاربری اش استفاده کنید یا آن را در یک فیلد جدا ذخیره کنید.

جریان بازنشانی کلمه عبور به صورت زیر است:

1- کاربر با دادن ایمیل خود درخواست بازنشانی کلمه عبور را میدهد.

2- Parse یک ایمیل حاوی لینک مخصوص به آدرس ایمیل میفرستد.

3- کاربر بر روی لینک کلیک میکند تا به یک صفحه جدید هدایت شود تا رمز جدید را در آن وارد کند.

4- کلمه عبور بازنشانی شده و به رمز جدید تغییر میکند.

در نظر داشته باشید که در این جریان، برنامه شما با همان نامی که در هنگام ساخت در Parse مشخص کرده اید معرفی میشود.

### Query زدن

جهت ایجاد query برای کاربران، میتوان از query مخصوص کاربر استفاده کرد:

<div dir="ltr">

```java
ParseQuery<ParseUser> query = ParseUser.getQuery();
query.whereEqualTo("gender", "female");
query.findInBackground(new FindCallback<ParseUser>() {
    public void done(List<ParseUser> objects, ParseException e) {
        if (e == null) {
        // The query was successful.
        } else {
        // Something went wrong.
        }
    }
});
```
</div>

همچنین میتوان برای گرفتن ParseUser با استفاده از id آن، از get استفاده کرد.

### وابستگی ها 

وابستگی های مربوط به ParseUser بدون نیاز به تنظیم خاصی قابل ایجاد هستند، به عنوان مثال در یک برنامه بلاگ نویسی برای ایجاد پست جدید و بازیابی پست های قبلی یک کاربر داریم:

<div dir="ltr">

```java
ParseUser user = ParseUser.getCurrentUser();
// Make a new post
 ParseObject post = new ParseObject("Post");
 post.put("title", "My New Post");
 post.put("body", "This is some great content.");
 post.put("user", user);
 post.saveInBackground();
// Find all posts by the current user
ParseQuery<ParseObject> query = ParseQuery.getQuery("Post");
query.whereEqualTo("user", user);
query.findInBackground(new FindCallback<ParseObject>() { ... });
```
</div>

## Session در Parse

session ها نشان دهنده log in بودن یک کاربر بر روی یک دستگاه هستند.
هر گاه کاربر در دستگاهی log in کند یا sign up کند، session قبلی از دستگاه پاک میشود و یک session جدید ایجاد میشود.
session ها به ازای هر زوج کاربر-دستگاه یکتا هستند و اگر کاربری با چند دستگاه وارد شده باشد، چند session مختلف خواهد داشت.
همچنین اگر کاربری با دستگاهی که قبلا وارد شده است دوباره log in کند، session قبلی اش پاک میشود و یک نشست جدید به جایش ایجاد میشود.
object های session در Parse در کلاس Session ذخیره میشوند و با کمک Parse Dashboard Data Browser میتوان آنها را مشاهده کرد.
همچنین چند API برای مدیریت Session ها در برنامه موجود است.

Session یک زیر کلاس از Parse Object است، در نتیجه میتوانید آن را مثل سایر آبجکت ها به روز رسانی، حذف یا ذخیره کنید.
از آنجا که Parse server به طور خودکار هنگام log in، نشست را میسازد، لازم به ساختن آن به طور دستی نیست.
پاک کردن session، در دستگاهی که token آن نشست را استفاده میکند، کاربر را از حساب خارج میکند.

بر خلاف سایر object ها، session ها کد مربوط به cloud را تحریک نمیکنند، در نتیجه نمیتوان برای آن ها تابع afterSave و beforeSave نوشت.

### خصوصیات session 

- sessionToken: یک شناسه از جنس string که به صورت تصادفی تولید میشودو جهت تایید هویت در درخواست های Parse API استفاده میشود.
در پاسخ query های session، تنها session کنونی دارای شناسه خواهد بود.

- user: یک اشاره گر به کاربری که این session مربوط به آن است. این مورد قابل تغییر نیست و read only است.

- cratedWith: اطلاعاتی در مورد این که نشست چگونه ایجاد شده است. این مورد نیز read only است. مثال:
  { "action": "login", "authProvider": "password"}

    - action: میتواند مقادیر login, signup, create و upgrade را به خود بگیرد.
  مقدار create برای وقتی است که توسعه دهنده به صورت دستی session object را ذخیره کند.
      مقدار upgrade برای وقتی است که 
کاربر از یک legacy session token به یک session قابل لغو ارتقا پیدا کرده است.

    - authProvider: منبع اعتبار سنجی.
  میتواند مقدایر facebook, twitter, password یا anonymous را به خود بگیرد.

- expiresAt: زمان تقریبی منقضی شدن session در فرمت UTC. 
  این مقدار readonly است و قابل تغییر نیست، میتوان از طریق صفحه Parse Dashboard setting میزان اعتبار نشست ها را تعیین کرد(یک سال عدم فعالیت یا اعتبار دائمی).

- installationId: (تنها یکبار میتوان آن را تنظیم کرد) یک رشته که به installation ای اشاره میکند که session از آنجا log in شده.
برای Parse Sdk ها به طور خودکار وقتی که کاربر ثبت نام میکند یا وارد میشود تنظیم میشود.
  تمام فیلد های ویژه به غیر از این فیلد میتوانند به صورت خودکار توسط Parse Server پر شوند.
  اگر Class-Level Permissions را غیر فعال نکنید، سایر session های یک حساب کاربری هم میتوانند این فیلد را ببینند.

### مدیریت خطای session token نامعتبر 

اگر session ها قابل فسخ باشند، ممکن است session token غیر معتبر شود چون دیگر به هیچ session ای در Parse Server اشاره نمیکند.
حالت های مختلفی وجود دارد که این اتفاق بیفتد: مثلا این که به صورت دستی پاک شود، یا اعتبار session تمام شود، یا از طریق دستگاه های دیگر session پاک شود(در صورتی که این قابلیت پیاده سازی شده باشد).
وقتی که یک session token دیگر به session ای روی سرور اشاره نکند، request از طرف آن دستگاه با خظای  “Error 209: invalid session token” مواجه خواهد شد.

برای مدیریت این خطا توصیه میشود تابعی global بنویسید که در Parse Request error callback فراخوانی شود و این مورد را مدیریت کند.
میتوانید بعد از این خطا بعد از اعلام به کاربر، به صفحه ورود رفته و از کاربر بخواهید نام کاربری و رمز عبور را دوباره وارد کند تا یک session جدید ساخته شود.

<div dir="ltr">

```java
public class ParseErrorHandler {
  public static void handleParseError(ParseException e) {
    switch (e.getCode()) {
      case INVALID_SESSION_TOKEN: handleInvalidSessionToken()
        break;
      ... // Other Parse API errors that you want to explicitly handle
    }
  }
  private static void handleInvalidSessionToken() {
    //--------------------------------------
    // Option 1: Show a message asking the user to log out and log back in.
    //--------------------------------------
    // If the user needs to finish what they were doing, they have the opportunity to do so.
    //
    // new AlertDialog.Builder(getActivity())
    //   .setMessage("Session is no longer valid, please log out and log in again.")
    //   .setCancelable(false).setPositiveButton("OK", ...).create().show();
    //--------------------------------------
    // Option #2: Show login screen so user can re-authenticate.
    //--------------------------------------
    // You may want this if the logout button could be inaccessible in the UI.
    //
    // startActivityForResult(new ParseLoginBuilder(getActivity()).build(), 0);
  }
}
// In all API requests, call the global error handler, e.g.
query.findInBackground(new FindCallback<ParseObject>() {
    public void done(List<ParseObject> results, ParseException e) {
        if (e == null) {
            // Query successful, continue other app logic
        } else {
            // Query failed
        ParseErrorHandler.handleParseError(e);}
    }
});
```
</div>

### امنیت session 

object های session تنها برای کاربری که session مربوط به آن است قابل دسترسی است.
یعنی تمام session ها یک ACL دارند که تنها به کاربر مربوطه اجازه خواندن و نوشتن میدهد و اگر برای session ها query درخواست کنید، تنها session های مربوط به کاربری که در حال حاضر وارد شده است را دریافت میکنید.

وقتی یک کاربر را با شیوه ی User login یا sign up یا twitter و facebook وارد میکنید، یک Parse به طور خودکار یک session محدود نشده در سرور میسازد.

همچون سایر کلاس های Parse، شما میتوانید برای session ها نیز یک مجوز سطح-کلاس (CLP) تعریف کنید تا خواندن و نوشتن را از session API محدود کنید.
البته این کار، خللی در امور خودکار سرور جهت ساختن و پاک کردن session ها هنگام ثبت نام و ورود و خروج ایجاد نمیکند.
توصیه میشود که CLP هایی که نیازی به آنها ندارید را غیر فعال کنید.
در اینجا چند مثال از کاربرد CLP ها آورده شده است:

- Find, Delete:
با کمک آن میتوان صفحه ای ساخت که کاربر در آن سایر session هایش را در دستگاه های دیگر مشاهده کرده و از آنها خارج شود.
اگر برنامه شما چنین صفحه ای ندارد توصیه میشود این را غیر فعال کنید.

- Create:
این مورد برای برنامه هایی کاربرد دارد که کاربر بخواهد از دستگاهش یک session برای دستگاهی دیگر ایجاد کند.
اگر برای موبایل یا وب برنامه مینویسید بهتر است این گزینه را غیرفعال کنید.
استفاده اصلی این مورد برای برنامه های مربوط به اینترنت اشیا (IoT) است، در این شرایط هم اگر دستگاه دیگر واقعا نیازی به داده های کاربر ندارد بهتر است آن را غیر فعال کنید.

- Get, Update, Add Field: 
اگر به این گزینه ها نیازی ندارید آنها را غیر فعال کنید.

## Role در Parse

با بزرگ شدن برنامه و زیاد شدن تعداد کاربران، ممکن است نیاز به افزودن دسترسی هایی بزرگ تر از ACL به کاربران داده شود، با استفاده از role میتوانید این کار را انجام دهید.
Parse از  Role-based Access Control پشتیبانی میکند.
role ها روشی منطقی برای اعطای دسترسی هستند، role آبجکتی است که شامل تعدادی user و role است، هر دسترسی که به role داده شود برای کاربران داخل آن و role هایی که در آن موجود هستند نیز داده میشود.

به عنوان مثال در برنامه ای جهت انتشار محتوا، شما میتوانید کاربرانی به عنوان مدیر داشته باشید که محتوای کاربران عادی را اصلاح یا حذف کنند.
همچنین میتوانید نقش ادمین را به تعدادی از کاربران بدهید که همان دسترسی های مدیر را داشته باشند اما بتوانند تنظیماتی را در برنامه نیز اعمال کنند.
با اعطای این نقش ها به تعدادی از کاربران، میتوانید مطمئن باشید که تمام کاربران میتوانند به این نقش ها ارتقا داده شوند، بدون این که بخواهید به طور دستی این کار را انجام دهید.

برای این کار کلاس ParseRole در کلاینت وجود دارد که فرزند کلاس ParseObject است.

### خصوصیات ParseRole 

- name: 
این نام را تنها یکبار هنگام ساختن role میتوان آن را انتخاب کرد.
  این نام باید از کاراکتر های الفبا، فاصله و خط فاصله تشکیل شده باشد.
  هدف از داشتن نام این است که برای شناسایی نقش، به جای objectId از این فیلد استفاده شود.
  
- users: 
یک relation به آن دسته از کاربران است که دسترسی های role را به ارث میبرند.
  
- rolse:
یک relation به role هایی که کاربرانش دسترسی های این role را به ارث میبرند.
  
### امنیت برای object های role 

امنیت برای ParseRole مشابه امینت سایر object هاست، با این تفاوت که ACL باید به طور صریح برای آن مشخص شود.
به طور عمومی در برنامه ها Administrator ها و master user که دسترسی های زیادی دارند باید بتوانند role جدید بسازند یا آنها را تغییر دهند.
مراقب باشید که اگر به یک کاربر اجازه نوشتن در یک ParseRole را داده باشید، میتواند به آن role کاربر جدید ایجاد کند یا آن را کاملا پاک کند.

برای ساختن role جدید، میتوانید از این روش استفاده کنید:

<div dir="ltr">

```java
// By specifying no write privileges for the ACL, we can ensure the role cannot be altered.
ParseACL roleACL = new ParseACL();
roleACL.setPublicReadAccess(true);
ParseRole role = new ParseRole("Administrator", roleACL);
role.saveInBackground();
```
</div>

شما میتوانید role ها و کاربران را به role جدید اضافه کنید که دسترسی های آن را به ارث ببرند.
برای این کار باید relation مربوط را بسازید:

<div dir="ltr">

```java
ParseRole role = new ParseRole(roleName, roleACL);
for (ParseUser user : usersToAddToRole) {
  role.getUsers().add(user)
}
for (ParseRole childRole : rolesToAddToRole) {
  role.getRoles().add(childRole);
}
role.saveInBackground();
```
</div>

### امنیت بر پایه role برای سایر object ها 

با ترکیب کردن role و ParseACL میتوانید برای هر object مشخص کنید که هر کاربر چه دسترسی هایی به object دارد.
هر ParseObject یک ACL دارد که مشخص میکند کدام کاربران میتوانند آن را بخوانند و کدام ها میتوانند در آن بنویسند.

برای دادن قابلیت خواندن یا نوشتن روی یک object برای یک role چند روش موجود است:

<div dir="ltr">

```java
ParseRole moderators = /* Query for some ParseRole */;
        ParseObject wallPost = new ParseObject("WallPost");
        ParseACL postACL = new ParseACL();
        postACL.setRoleWriteAccess(moderators);
        wallPost.setACL(postACL);
        wallPost.saveInBackground();
```
</div>

اگر نخواهید query بزنید، میتوانید نام role را برای ACL مشخص کنید:

<div dir="ltr">

```java
ParseObject wallPost = new ParseObject("WallPost");
ParseACL postACL = new ParseACL();
postACL.setRoleWriteAccess("Moderators", true);
wallPost.setACL(postACL);
wallPost.save();
```
</div>

همچنین هنگام تعریف ACL پیش فرض، میتوانید از ACL بر پایه role استفاده کنید.
با این کار ضمن حفاظت از اطلاعات کاربرانتان، میتوانید به کاربران با مقام بالاتر، دسترسی های لازم را بدهید.
به عنوان مثال یک برنامه forum میتواند به این شکل عمل کند:

<div dir="ltr">

```java
ParseACL defaultACL = new ParseACL();
// Everybody can read objects created by this user
defaultACL.setPublicReadAccess(true);
// Moderators can also modify these objects
defaultACL.setRoleWriteAccess("Moderators");
// And the user can read and modify its own objects
ParseACL.setDefaultACL(defaultACL, true);
```
</div>

### سلسله مراتب role 

همانطور که قبل تر گفته شد، اگر یک role را فرزند یک role دیگر قرار دهیم، تمام دسترسی هایی که role پدر در اختیار دارد، در اختیار role فرزند نیز خواهد بود.
برای نمونه، فرض کنید در برنامه ای، نقش Moderator وجود دارد که پست های کاربران را مدیریت میکند.
در این برنامه اگر بخواهیم نقش Administrator را اضافه کنیم که علاوه بر تنظیمات برنامه، بتواند پست های کاربران را همانند Moderator ها مدیریت کند خواهیم داشت:

<div dir="ltr">

```java
ParseRole administrators = /* Your "Administrators" role */;
ParseRole moderators = /* Your "Moderators" role */;
moderators.getRoles().add(administrators);
moderators.saveInBackground();
```
</div>

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
Parse Android SDK یک Datastore محلی در اختیار ما می‌گذارد که از آن می‌توان برای ذخیره و بازیابی `ParseObject`ها استفاده کرد، حتی زمانی که اینترنت در دسترس نیست. برای استفاده از این کاربرد، کافی است که `()Parse.enableLocalDatastore` را قبل از صدا کردن `initialize` صدا کنیم.

<div dir="ltr">

```java
import com.parse.Parse;
import android.app.Application;

public class App extends Application {
  @Override
  public void onCreate() {
    super.onCreate();

    Parse.enableLocalDatastore(this);
    Parse.initialize(this);
  }
}
```
</div>

اگر از یک `Parse.Configuration.Builder` استفاده می‌کنید، آن را آنجا فعال کنید:

<div dir="ltr">

```java
Parse.initialize(new Parse.Configuration.Builder(context)
  .server(...)
  .applicationId(...)
  .enableLocalDataStore()
  .build());
```
</div>

فعال کردن Datastore محلی یک سری عوارض جانبی دارد که شما باید از آن‌ها آگاه باشید. وقتی که فعال شود، فقط یک مورد از هر `ParseObject` موجود خواهد بود. برای مثال، تصور کنید که شما یک نمونه از کلاس `"GameScore"` با `"objectId = "xWMyZ4YEGZ` دارید و سپس شما یک `ParseQuery` برای همهٔ موارد `"GameScore"` با همان `objectId` چاپ می‌کنید. نتیجه همواره همان نمونه از شیئی خواهد بود که شما در حافظه از قبل داشتید.

یک اثر جانبی دیگر این است که کاربر فعلی و نسخهٔ نصب‌شده فعلی در حافظه در Datastore محلی ذخیره خواهند شد. پس شما می‌توانید که تغییرات ذخیره‌نشدهٔ این اشیا را بین اجراهای مختلف برنامهٔ‌تان را با کمک توابع زیر نگه‌دارید.

صدا زدن تابع `saveEventually` روی یک `ParseObject` باعث می‌شود که آن شی تا زمانی که ذخیره کردن کامل شود، به Datastore محلی پین شود. پس الآن، اگر شما `ParseUser` فعلی را عوض کنید و `()ParseUser.getCurrentUser().saveEventually` را صدا بزنید، همواره برنامهٔ شما تغییراتی را که شما اعمال کرده‌اید را می‌بیند.

### پین کردن

شما می‌توانید یک `ParseObject` را در Datastore محلی به وسیلهٔ پین کردن آن ذخیره کنید. پین کردن یک `ParseObject` همانند ذخیره کردن بازگشتی است، پس هر شیئی که توسط شیء پین‌شده به آن اشاره شده باشد هم پین می‌شود. زمانی که یک شی پین شده است، هر بار که شما آن را با ذخیره کردن یا گرفتن اطلاعات جدید آپدیت کنید، کپی آن شی در Datastore محلی به صورت خودکار آپدیت خواهد شد و شما نیازی نیست که نگران آن باشید.

<div dir="ltr">

```java
ParseObject gameScore = new ParseObject("GameScore");
gameScore.put("score", 1337);
gameScore.put("playerName", "Sean Plott");
gameScore.put("cheatMode", false);

gameScore.pinInBackground();
```
</div>

اگر شما چند شی دارید، شما می‌توانید که همهٔ آن‌ها را یک جا با تابع `pinAllInBackground` پین کنید.

<div dir="ltr">

```java
ParseObject.pinAllInBackground(listOfObjects);
```
</div>

### بازیابی کردن

ذخیره کردن اشیا عالیست اما تنها هنگامی مفید است که شما بتوانید اشیا را بعدا بازیابی کنید. برای بازیابی یک شی از Datastore محلی، تابع `fromLocalDatastore` را صدا بزنید تا به `ParseQuery` بگوید که کجا را برای نتایج آن نگاه کند.

<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.fromLocalDatastore();
```
</div>

تنها تفاوت این است که شما قادر نخواهید بود که به اطلاعاتی که توسط لیست‌های کنترل‌کننده دسترسی بر پایه نقش مراقبت می‌شود، دسترسی پیدا کنید، به علت اینکه نقش‌ها روی سرور ذخیره شده‌اند. برای دسترسی به این اطلاعات، شما ممکن است نیاز داشته باشید که از لیست‌های کنترل‌کننده دسترسی صرف نظر کنید. برای این کار کافی است که هنگام اجرای یک Local datastore query با صدا کردن تابع `ignoreAcls` روی query این کار را انجام دهید. توجه کنید که بعد از صدا کردن این تابع، شما نمی‌توانید که از همان query برای لود کردن از شبکه استفاده کنید.

<div dir="ltr">

```java
// If data is protected by Role based ACLs:
query.ignoreAcls();
```
</div>

سپس شما می‌توانید که اشیا را طبق معمول بازیابی کنید.

<div dir="ltr">

```java
query.getInBackground("xWMyZ4YE", new GetCallback<ParseObject>() {
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

### Querying the Local Datastore

اکثرا شما می‌خواهید که یک لیست از اشیایی را بیابید که با یک معیار به‌خصوص تطابق دارند، به جای اینکه تنها یک شی را با کمک ID بگیرید. برای انجام این کار، شما می‌توانید که از یک 
<a href="https://docs.parseplatform.org/android/guide/#queries">
ParseQuery	
</a>
 استفاده کنید. هر `ParseQuery` می‌تواند همراه Datastore محلی همانگونه استفاده شود که همراه با شبکه استفاده می‌شود. نتایج شامل هر شیئی است که شما پین کرده‌اید و با query تطابق دارد.همهٔ تغییرات ذخیره‌نشده‌ای که شما در شی ایجاد کرده‌اید، هنگام ارزیابی query در نظر گرفته خواهد شد. پس شما می‌توانید که یک شیء محلی که تطبیق دارد را بیابید، حتی اگر آن هیچ‌گاه از طرف سرور برای این query به‌خصوص return نشده باشد.
 
<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.whereEqualTo("playerName", "Joe Bob");
query.fromLocalDatastore();
// If data is protected by Role based ACLs:
query.ignoreAcls();
query.findInBackground(new FindCallback<ParseObject>() {
    public void done(List<ParseObject> scoreList,
                     ParseException e) {
        if (e == null) {
            Log.d("score", "Retrieved " + scoreList.size());
        } else {
            Log.d("score", "Error: " + e.getMessage());
        }
    }
});
```
</div>

### از پین برداشتن

زمانی که کار شما با یک شی تمام شده‌است و دیگر به آن در Datastore محلیتان نیازی ندارید، شما می‌توانید به راحتی آن را از پین بردارید. این کار مقداری از فضای حافظه را آزاد می‌کند و باعث می‌شود که queryهای شما سریع‌تر اجرا شوند.

<div dir="ltr">

```java
gameScore.unpinInBackground();
```
</div>

همچنین یک تابع هم وجود دارد که چند شی را از پین برمی‌دارد.

<div dir="ltr">

```java
ParseObject.unpinAllInBackground(listOfObjects);
```
</div>

### پین‌کردن همراه با برچسب‌ها

پین‌کردن و از پین برداشتن دستی هر شی به صورت تکی تا حدی شبیه استفاده از دستورهای `malloc` و `free` است. این یک ابزار بسیار قدرتمند است ولی این می‌تواند سخت باشد که مدیریت کنیم چه اشیایی در سناریوهای پیچیده ذخیره شوند. به عنوان مثال، فرض کنید که شما دارید یک بازی با لیست‌های جدا برای امتیازات بالای جهانی و امتیازات بالای دوستانتان می‌سازید. اگر یکی از دوستانتان یک امتیاز بالای جهانی داشته باشد، شما نیاز دارید که مطمئن شوید که شما آن‌ها را کاملا از پین برنمی‌دارید، زمانی که شما آن‌ها را از یکی از queryهای ذخیره‌شده را حذف می‌کنید. برای اینکه این سناریوها آسان‌تر شوند، شما می‌توانید با یک برچسب پین کنید. برچسب‌ها نشانگر یک گروه از اشیا هستند که باید با همدیگر ذخیره شوند.

<div dir="ltr">

```java
// Add several objects with a label.
ParseObject.pinAllInBackground("MyScores", someGameScores);

// Add another object with the same label.
anotherGameScore.pinInBackground("MyScores");
```
</div>

برای از پین برداشتن هم‌زمان همهٔ اشیا با برچسب یکسان، شما می‌توانید که یک برچسب را به توابع از پین برداشتن پاس دهید. این شما را نجات می‌دهد از دنبال کردن دستی اشیایی که در گروه‌هایی هستند که برای شما مهم است.

<div dir="ltr">

```java
ParseObject.unpinAllInBackground("MyScores");
```
</div>

هر شیئی می‌تواند در Datastore بماند، تا زمانی که با هر کدام از برچسب‌ها پین بماند. به عبارت دیگر، اگر شما یک شی را با دو برچسب پین کنید و یکی از برچسب‌ها را از پین بردارید، شی همچنان به خاطر برچسب دیگرش پین می‌ماند.

### ذخیره‌کردن نتایج query

پین کردن با برچسب‌ها کار را برای ذخیره‌کردن نتایج queryها آسان می‌کند. شما می‌توانید از یک برچسب برای پین کردن نتایج هر query مختلف استفاده کنید. برای گرفتن نتایج جدید از شبکه، فقط یک query انجام دهید و اشیای پین‌شده را آپدیت کنید.


<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.orderByDescending("score");

// Query for new results from the network.
query.findInBackground(new FindCallback<ParseObject>() {
  public void done(final List<ParseObject> scores, ParseException e) {
    // Remove the previously cached results.
    ParseObject.unpinAllInBackground("highScores", new DeleteCallback() {
    public void done(ParseException e) {
      // Cache the new results.
      ParseObject.pinAllInBackground("highScores", scores);
    }
  });
  }
});
```
</div>

زمانی که شما می‌خواهید نتایج ذخیره‌شده را برای یک query بگیرید، شما می‌توانید آن query را در Datastore محلی اجرا کنید.

<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.orderByDescending("score");
query.fromLocalDatastore();

query.findInBackground(new FindCallback<ParseObject>() {
  public void done(List<ParseObject> scores, ParseException e) {
    // Yay! Cached scores!
  }
});
```
</div>

### همگام‌کردن تغییرات محلی

زمانی که شما تعدادی از تغییرات به صورت محلی ذخیره کرده‌اید، تعدادی راه مختلف وجود دارد که شما می‌توانید آن تغییرات را در Parse روی شبکه ذخیره کنید. آسان‌ترین راه برای انجام این کار با `saveEventually` می‌باشد. زمانی که شما `saveEventually` را روی یک `ParseObject` صدا می‌زنید، آن شی تا زمانی که بتواند ذخیره شود، پین می‌ماند. همچنین SDK مطمئن می‌شود که شی را دفعه بعد که شبکه در دسترس است، ذخیره کند.

<div dir="ltr">

```java
gameScore.saveEventually();
```
</div>

اگر شما علاقه دارید که کنترل بیشتری روی روشی که اشیا همگام می‌شوند داشته باشید، شما می‌توانید آن‌ها را در Datastore محلی نگه‌دارید، آن هم تا زمانی که شما آماده باشید که خودتان آن‌ها را با کمک `saveInBackground` ذخیره کنید. برای مدیریت مجموعهٔ اشیایی که نیاز به ذخیره‌شدن دارند، شما می‌توانید باز هم از یک برچسب استفاده کنید. تابع `fromPin` روی `ParseQuery`، گرفتن تنها اشیایی که برای شما مهم هستند را آسان می‌کند.

<div dir="ltr">

```java
ParseQuery<ParseObject> query = ParseQuery.getQuery("GameScore");
query.fromPin("MyChanges");
query.findInBackground(new FindCallback<ParseObject>() {
  public void done(List<ParseObject> scores, ParseException e) {
    for (ParseObject score in scores) {
      score.saveInBackground();
      score.unpinInBackground(“MyChanges”);
    }
  }
});
```
</div>
</div>
