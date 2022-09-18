# Prototype
### به معنی شبیه سازی (clone) هم گفته میشود 



یک الگوی تولیدی میبایشد اشیاء موجود را بدون اینکه کد خود را به کلاس های آنها وابسته کنید، کپی کنید.  `Prototype`  

---
## 😟 مشکل

فرض کنید یک شی دارید و می خواهید یک کپی دقیق از آن ایجاد کنید. چگونه آن را انجام می دهید؟ ابتدا باید یک شی جدید از همان کلاس ایجاد کنید. سپس باید تمام فیلدهای شی اصلی را نگاه کنید  و مقادیر آنها را در شی جدید کپی کنید.


عالی! اما یک مشکل  وجود دارد. همه اشیاء را نمی توان به این روش کپی کرد زیرا برخی از فیلدهای شی ممکن است خصوصی باشند و از خارج از خود شی قابل دسترس نباشد .

![](https://github.com/ftg-iran/didp-persian/blob/main/4-Catalog%20Of%20Design%20Patterns/images/content/prototype/prototype-comic-1-en.png)


کپی کردن یک شی *از خارج آن* همیشه امکان پذیر نیست.

یک مشکل دیگر در روش مستقیم وجود دارد. از آنجایی که برای ایجاد یک کپی باید کلاس شی را بشناسید، کد شما به آن کلاس وابسته می شود. اگر وابستگی اضافی شما را نمی ترساند، یک مشکل دیگر وجود دارد. گاهی اوقات شما فقط اینترفیسی را می‌شناسید که شی از آن پیروی می‌کند، اما کلاس مشخص آن را نمی‌دانید، برای مثال، زمانی که یک پارامتر در یک متد، اشیایی را می‌پذیرد که از یک اینترفیس پیروی می‌کنند.
---
## 😃 راه حل

الگوی پروتوتایپ فرآیند `clone` را به اشیاء واقعی که در حال `being cloned` هستند واگذار می کند. این الگو یک رابط مشترک را برای همه اشیایی که از `clone` پشتیبانی می کنند، اعلام می کند. این رابط به شما امکان می دهد یک شی را بدون جفت کردن کد خود با کلاس آن شی `clone` کنید. معمولاً چنین اینترفیس فقط شامل یک روش `clone` می شود.

![img](https://github.com/ftg-iran/didp-persian/blob/main/4-Catalog%20Of%20Design%20Patterns/images/content/prototype/prototype-comic-2-en.png)

نمونه های اولیه از پیش ساخته شده می توانند جایگزینی برای طبقه بندی فرعی باشند.

شیئی که ازclone  پشتیبانی می کند، نمونه اولیه نامیده می شود. هنگامی که اشیاء شما دارای ده‌ها فیلد و صدها پیکربندی ممکن است، شبیه‌سازی آنها ممکن است به عنوان جایگزینی برای  subclassing باشد.

در اینجا نحوه کار این است: شما مجموعه ای از اشیاء را ایجاد می کنید که به روش های مختلف پیکربندی شده اند. هنگامی که به یک شی مانند آنچه پیکربندی کرده اید نیاز دارید، به جای ساختن یک شی جدید از ابتدا، فقط یک نمونه اولیه را clone می کنید

---
## 🚗 در دنیای واقعی 


![IMG](https://github.com/ftg-iran/didp-persian/blob/main/4-Catalog%20Of%20Design%20Patterns/images/content/prototype/prototype-comic-3-en.png)

تقسیم سلولی

از آنجایی که نمونه‌های اولیه  واقعاً خود را کپی نمی‌کنند، تشابه بسیار نزدیک‌تر به الگو، فرآیند تقسیم سلولی میتوزی است (زیست‌شناسی، یادتان هست؟). پس از تقسیم میتوزی، یک جفت سلول یکسان تشکیل می شود. سلول اصلی به عنوان یک نمونه اولیه عمل می کند و نقش فعالی در ایجاد کپی دارد.

## 🚧 ساختار

### پیاده سازی اولیه 

![img](https://github.com/ftg-iran/didp-persian/blob/main/4-Catalog%20Of%20Design%20Patterns/images/diagrams/prototype/structure-indexed.png)

1-رابط Prototype روش های  clone را اعلام می کند. در بیشتر موارد، این روش تک کلون است.

2-کلاس Concrete Prototype روش clone را پیاده سازی می کند. این روش علاوه بر کپی کردن داده‌های شی اصلی در clone ممکن است برخی از موارد جزیی فرآیند شبیه‌سازی مربوط به شبیه‌سازی اشیاء مرتبط، باز کردن وابستگی‌های بازگشتی و غیره را نیز انجام دهد.

3-کلاینت می تواند یک کپی از هر شی که از اینترفیس نمونه اولیه پیروی می کند تولید کند.


**Prototype registry پیاده سازی **

![IMG](https://github.com/ftg-iran/didp-persian/blob/main/4-Catalog%20Of%20Design%20Patterns/images/diagrams/prototype/structure-prototype-cache-indexed.png)


Prototype Registry

 راه آسانی برای دسترسی به نمونه های اولیه پرکاربرد فراهم می کند. مجموعه ای از اشیاء از پیش ساخته شده را که آماده کپی هستند ذخیره می کند. ساده ترین رجیستری نمونه اولیه `name -> prototype`  hash map  است. با این حال، اگر به معیارهای جستجوی بهتری نسبت به یک نام ساده نیاز دارید، می توانید نسخه بسیار قوی تری از رجیستری بسازید.

---
 ##  ️#️⃣ شبه کد

 در این مثال، الگوی Prototype به شما امکان می‌دهد کپی‌های دقیقی از اجسام هندسی تولید کنید، بدون اینکه کد را با کلاس‌های آنها تکرار (coupling) کنید. 
 
 
 ![img](https://github.com/ftg-iran/didp-persian/blob/main/4-Catalog%20Of%20Design%20Patterns/images/diagrams/prototype/example.png)

 شبیه سازی مجموعه ای از اشیاء که به یک سلسله مراتب کلاس تعلق دارند.

 همه کلاس های شکل (shape) از یک interface  یکسان پیروی می کنند که یک روش شبیه سازی (clone) را ارائه می دهد. یک زیر کلاس ممکن است قبل از کپی کردن مقادیر فیلد خود در شیء به دست آمده، روش شبیه‌سازی والد را فراخوانی کند


 ```c++
 
 // Base prototype.
abstract class Shape is
    field X: int
    field Y: int
    field color: string

    // A regular constructor.
    constructor Shape() is
        // ...  
    
    // The prototype constructor. A fresh object is initialized
    // with values from the existing object.
    constructor Shape(source: Shape) is
        this()
        this.X = source.X
        this.Y = source.Y
        this.color = source.color

    // The clone operation returns one of the Shape subclasses.
    abstract method clone():Shape
// Concrete prototype. The cloning method creates a new object
// and passes it to the constructor. Until the constructor is
// finished, it has a reference to a fresh clone. Therefore,
// nobody has access to a partly-built clone. This keeps the
// cloning result consistent.
class Rectangle extends Shape is
field width: int
field height: int

constructor Rectangle(source: Rectangle) is
    // A parent constructor call is needed to copy private
    // fields defined in the parent class.
    super(source)
    this.width = source.width
    this.height = source.height

method clone():Shape is
    return new Rectangle(this)


class Circle extends Shape is
    field radius: int

constructor Circle(source: Circle) is
    super(source)
    this.radius = source.radius

method clone():Shape is
    return new Circle(this)


// Somewhere in the client code.
class Application is
    field shapes: array of Shape

    constructor Application() is
        Circle circle = new Circle()
        circle.X = 10
        circle.Y = 10
        circle.radius = 20
        shapes.add(circle)

        Circle anotherCircle = circle.clone()
        shapes.add(anotherCircle)   
        // The `anotherCircle` variable contains an exact copy
        // of the `circle` object.

        Rectangle rectangle = new Rectangle()
        rectangle.width = 10
        rectangle.height = 20
        shapes.add(rectangle)

    method businessLogic() is
        // Prototype rocks because it lets you produce a copy of
        // an object without knowing anything about its type.
        Array shapesCopy = new Array of Shapes.
        // For instance, we don't know the exact elements in the
        // shapes array. All we know is that they are all
        // shapes. But thanks to polymorphism, when we call the
        // `clone` method on a shape the program checks its real
        // class and runs the appropriate clone method defined
        // in that class. That's why we get proper clones
        // instead of a set of simple Shape objects.
        foreach (s in shapes) do
            shapesCopy.add(s.clone())

        // The `shapesCopy` array contains exact copies of the
        // `shape` array's children.
 
 ```

---
 ## 💡 کاربرد

 *از الگوی Prototype زمانی استفاده کنید که کد شما نباید به کلاس‌های  اشیایی که باید کپی کنید بستگی داشته باشد.*

 ✨ وقتی کد شما با اشیایی که از کدهای بیرونی از طریق برخی از اینترفیس ها به شما ارسال شده است، این اتفاق زیاد می افتد. `concrete classes ` این اشیاء *ناشناخته* هستند و حتی اگر بخواهید نمی توانید به آنها وابسته باشید.

 الگوی Prototype کد کلاینت را با یک رابط کلی برای کار با تمام اشیایی که از شبیه سازی پشتیبانی می کنند، فراهم می کند. این رابط کد کلاینت را از کلاس های عینی اشیایی که شبیه سازی می کند مستقل می کند.


   وقتی می‌خواهید تعداد subClass هایی  را که فقط در نحوه مقداردهی اولیه اشیاء مربوطه خود متفاوت هستند، کاهش دهید، از این الگو استفاده کنید. کسی می‌توانست اینsubClassها را ایجاد کند تا بتواند اشیایی با یک پیکربندی خاص ایجاد کند.

   ✨ الگوی Prototype به شما امکان می دهد از مجموعه ای از اشیاء از پیش ساخته شده، به روش های مختلف پیکربندی شده، به عنوان  prototypes استفاده کنید.

به جای نمونه سازی یک  subClass که با برخی از configها  مطابقت دارد، مشتری می تواند به سادگی به prototypes اولیه مناسب باشد و آن را شبیه سازی کند.


---
## 📋 چگونه آن را پیاده سازی کنیم 

1-اینترفیس نمونه اولیه را ایجاد کنید و متد `clone` را در آن فراخوانی کنید. یا فقط متد را به تمام کلاس های سلسله مراتب کلاس موجود اضافه کنید، اگر یکی وجود دارد.

2-یک کلاس نمونه اولیه باید سازنده جایگزینی را تعریف کند که شیء آن کلاس را به عنوان آرگومان می پذیرد. سازنده باید مقادیر تمام فیلدهای تعریف شده در کلاس را از شی ارسال شده در نمونه جدید ایجاد شده کپی کند. اگر در حال تغییر یک کلاس فرعی هستید، باید سازنده والد را فراخوانی کنید تا به `SuperClass` اجازه دهید کلونینگ فیلدهای خصوصی خود را انجام دهد. اگر زبان برنامه نویسی شما از بارگذاری بیش از حد overloading) پشتیبانی نمی کند، می توانید روش خاصی را برای کپی کردن داده های شی تعریف کنید. سازنده مکان مناسب تری برای انجام این کار است زیرا شی حاصل را بلافاصله پس از *call* با `new operator` تحویل می دهد.

3-روش شبیه سازی معمولاً از یک خط تشکیل شده است: اجرای یک `new operator` با prototype سازنده. توجه داشته باشید که هر کلاس باید صراحتاً روش clone را override کند و از نام کلاس خود همراه با عملگر جدید استفاده کند. در غیر این صورت، روش شبیه سازی ممکن است یک شی از یک کلاس والد تولید کند.


4-به صورت اختیاری، یک رجیستری prototype برای ذخیره کاتالوگ نمونه های اولیه پرکاربرد ایجاد کنید. می‌توانید رجیستری را به‌عنوان یک کلاس factory جدید پیاده‌سازی کنید یا آن را با یک متد استاتیک برای واکشی prototype در کلاس Prototype قرار دهید. این روش باید یک نمونه اولیه را بر اساس معیارهای جستجو جستجو کند که کد client به متد ارسال می کند. معیارها ممکن است یک تگ رشته ساده یا مجموعه پیچیده ای از پارامترهای جستجو باشد. پس از یافتن prototype مناسب، رجیستری باید آن را شبیه سازی کند و کپی را به client برگرداند. در نهایت، فراخوانی‌های مستقیم سازنده‌های subclassها را با فراخوانی به روش factory رجیستری نمونه جایگزین کنید.

---
## ⚖️ سود و ضرر

✔️ شما می توانید اشیاء را بدون اتصال به concrete class  آنها شبیه سازی کنید.

✔️ شما میتوانید برای خلاص شدن از intialize مکرر کد به نفغ cloning نمونه های اولیه (prototype) از پیش ساخته شده استفاده کنید.

✔️ شما می توانید اشیاء پیچیده را راحت تر تولید کنید.

 ✔️ شما جایگزینی برای ارث بری هنگامی که با کانفیگ های شی های پیجیده درگیر هستید دارید 

 ✖️ شبیه سازی اشیاء پیچیده که دارای ارجاعات circular هستند ممکن است بسیار مشکل باشد.    

 ---
 ## 🔄 روابط با الگوهای دیگر

 بسیاری از طراحی ها با استفاده از روش Factory (کمتر پیچیده و قابل تنظیم تر از طریق (subclass)) شروع می شوند و به سمت Abstract Factory، Prototype یا Builder (انعطاف پذیرتر، اما پیچیده تر) تکامل می یابند.

 کلاس های Factory انتزاعی اغلب بر اساس مجموعه ای از Factory Methods هستند، اما شما همچنین می توانید از Prototype برای ترکیب متدهای این کلاس ها استفاده کنید.

 هنگامی که نیاز به ذخیره کپی از دستورات در تاریخ دارید،prototype می تواند به شما کمک کند.

 طرح هایی که به شدت از *Composite*  و *decorator* استفاده می کنند اغلب می توانند از استفاده از prototype بهره ببرند. اعمال الگو به شما این امکان را می دهد که ساختارهای پیچیده را به جای بازسازی مجدد از ابتدا clone کنید.



نمونه اولیه (prototype)مبتنی بر وراثت نیست، بنابراین اشکالاتی ندارد. از سوی دیگر، Prototype به یک مقداردهی اولیه پیچیده از شی کلون شده نیاز دارد. Factory Method بر اساس وراثت است اما نیازی به مرحله اولیه سازی ندارد.


گاهی اوقات Prototype می تواند جایگزین ساده تری برای Memento باشد. اگر شیء، وضعیتی که می‌خواهید در تاریخچه ذخیره کنید، نسبتاً ساده باشد و ارتباطی  به منابع خارجی نداشته باشد، یا ارتباطات(links) به راحتی دوباره برقرار شوند، کار می‌کند.

 
**Abstract Factories** و **Builders** و  **Prototypes** 

به عنوان singleton میتوانند ییاده سازی شوند

--- 
