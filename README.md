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

## Query در Parse

## کاربر در Parse 
در هسته بسیاری از برنامه ها، ایده حساب های کاربری هستند که به کاربران اجازه میدهند به شیوه ای ایمن به اطلاعاتشان دسترسی داشته باشند.
ما یک کلاس مخصوص user ارائه میدهیم به نام ParseUser که به صورت خودکار بسیاری از کار های لازم برای مدیریت حساب کاربری را به دست میگیرد.

با این کلاس، شما میتوانید عملکرد حساب کاربری را به برنامه‌تان اضافه کنید.

ParseUser زیرکلاسی از ParseObject است و همان ویژگی ها را دارد، همچون الگوی انعطاف‌پذیر، دوام خودکار، و یک واسط key value.
تمام متد هایی که در ParseObject هستند در ParseUser نیز وجود دارند.
تفاوتشان در این است که ParseUser چند ویژگی اضافه دارد که مخصوص حساب کاربری است.

###خصوصیات ParseUser
ParseUser چند خصوصیت دارد که آن را از ParseObject متمایز میسازد:

<space><space><space>- username: نام کاربری برای کاربر (الزامی)

<space><space><space>- password: رمز عبور برای کاربر (مورد نیاز در هنگام ثبت نام)

<space><space><space>- email: آدرس ایمیل برای کاربر (اختیاری)

بعدا هنگامی که وارد موارد استفاده مختلف برای کاربران شدیم، هر یک از این ها را به تفصیل شرح میدهیم.
به خاطر داشته باشید که اگر شما username و email را با استفاده از setter ها ثبت کنید، دیگر نیازی نیست که با متد put آنها را ثبت کنید.

### ثبت نام
اولین کاری که برنامه شما احتمالا انجام میدهد درخواست ثبت نام از کاربر است.
کدی که در ادامه آمده یک ثبت نام معمولی را به تصویر میکشد:

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
این درخواست به صورت ناهمگام یک کاربر جدید در برنامه parse شما ایجاد خواهد کرد.
قبل از این که این کار را انجام دهد، چک میکند که مطمئن شود که هم username و هم email یکتا باشند.
همچنین به صورت ایمن پسورد را با استفاده از bcrypt در فضای ابری hash میکند.
ما هرگز رمز عبور ها را به صورت متن ساده ذخیره نمی کنیم و همینطور هرگز رمز را به صورت متن ساده به کلاینت مخابره نمیکنیم.

به خاطر داشته باشید که ما از متد signUpInBackground استفاده کردیم، نه از متد saveInBackground.
همیشه باید ParseUser های جدید با متد signUpInBackground(یا signUp) ساخته شوند.
بروزرسانی های بعدی برای یک کاربر میتواند با فراخوانی save انجام شود.

متد signUpInBackground به صورت های مختلفی در دسترس است.
با قابلیت بازگردانی خطا ها، و همچنین نسخه های همگام.
طبق معمول، ما به شدت توصیه میکنیم که اگر مقدور است، از نسخه های ناهمگام استفاده کنید، تا UI برنامه شما را متوقف نکند.
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
البته، بعد از این که به کاربران اجازه دادید که ثبت نام کنند، شما نیاز دارید به آنها اجازه دهید که در آینده به حساب هایشان وارد شوند.
برای انجام این کار، شما میتوانید از متد logInInBackground استفاده کنید.

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

###تایید کردن ایمیل ها
فعال کردن تایید ایمیل در تنظیمات یک برنامه به برنامه این امکان را میدهد که بخشی از تجربیاتش را به کاربران با ایمیل تایید شده اختصاص دهد.
تایید کردن ایمیل کلید emailVerified را به آبجکت ParseUser اضافه میکند.
وقتی که email یک ParseUser تنظیم یا عوض شود، emailVerified به false تغییر میکند.
سپس پارس یک لینک برای کاربر ارسال میکند که emailVerified را به true تغییر خواهد داد.

سه حالت مورد توجه برای emailVerified به شرح زیر است:

<space><space><space>1-<space><space><space> true:
کاربر ایمیلش را با کلیک کردن روی لینکی که پارس برایش ارسال کرده تایید کرده است.
ParseUser ها در ابتدای ساختن حسابشان نمیتوانند این مقدار را داشته باشند.

<space><space><space>2-<space><space><space> false:
آخرین باری که آبجکت ParseUser گرفته شده، کاربر ایمیلش را تایید نکرده بوده است.
اگر emailVerified مقدار false دارد، فراخوانی
()fetch

[comment]: <> (this might be wrong)
روی ParseUser را در نظر بگیرید.

<space><space><space>3-<space><space><space> missing:
آبجکت ParseUser وقتی ایجاد شده که تایید ایمیل خاموش بوده یا ParseUser ایمیلی ندارد.

###کاربر کنونی

اگر کاربر با هر بار باز کردن برنامه مجبور به ورود باشد باعث مزاحمت است.
شما میتوانید از این امر با استفاده از آبجکت currentUser کش شده اجتناب کنید.

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

###کاربران ناشناس
این که بتوانید داده و آبجکت ها را به کاربران منحصر بفرد پیوند دهید بسیار ارزشمند است، اما گاهی اوفات شما میخواهید که بتوانید این کار را انجام دهید بدون این که کاربر را مجبور به انتخاب نام کاربری و رمز عبور کنید.

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