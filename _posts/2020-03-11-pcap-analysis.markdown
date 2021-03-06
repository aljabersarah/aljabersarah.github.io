![](https://i.ibb.co/xjhgRQs/des-1.png)
--- 


#  مقدمة:

اهلاً بكم، في هذا المقال سوف اقوم بشرح تحليل ملف pcap لحظة اصابه جهاز بداخل الشبكة ب برمجية خبيثة. 


 هنا تجد المصادر لملف  
 [pcapflie](https://www.malware-traffic-analysis.net/2020/01/30/index.html)

##   عناصر المقالة: 

- مقدمة في Wireshark 
- ترتيب وتنظيم واجهة (Wireshark)
-   البرمجية الخبيثة: 
- خطوات حل التحدي:


##  مقدمة في Wireshark: 
  Wireshark يعد من أهم أدوات تحليل الشبكات ان لم يكن الأهم، برنامج Wireshark برنامج مفتوح المصدر، يقوم بتحليل الشبكة، بروتوكلاتها ، تفاصيل الأجهزة بالشبكة معرفة المشاكل في الشبكة  -ان وجد-  وتحديد مكان تواجدها، يُستفاد من البرنامج في تحليل وضع الشبكة كمية ونوع البيانات المرسله من والى داخل الشبكة، حجم البيانات المستخدمة لكل جهاز بالشبكة .. الخ. 

يستفاد أيضا من برنامج Wireshark في تحليل الأجهزة المصابه بالبرمجيات الخبيثة، تحديد تفاصيل هذه الأجهزة، الوقت التي تمت في الاصابة، من اين وكيف تمت اصابة الآجهزة ببرمجية xخبيثة، وهذا هو الهدف من مقالنا هذا. 

## ترتيب وتنظيم واجهة (Wireshark): 
في اللحظة التي يتم فيها تشغيل Wireshark سوف يقوم بجمع كل بيانات الشبكة (Traffic)،وعند التحليل قد لا يهمنا وجود كل هذه البيانات مما قد يجعل مهمة التحليل اصعب، لذلك سوف نقوم بترتيب البرنامج بحيث يسهل علينا ايجاد ما نبحث عنه بكل سهولة. 
 
<span style="color:DarkRed">أولا: ترتيب الأعمدة واظهار ما نحتاج اليه فقط: </span>

 
`  تنبيه: يوجد طريقتين ل إظهار هذه الأعمدة وسيتضح ذلك مع الشرح `

في هذا المثال نحتاج للقوائم التالية: 
- source ip 
- source port 
- Destination ip 
- Destination port 
- HTTP 
- HTTPS 
- Info 
- CNameString  

<span style="color:DarkRed">الطريقة الأولى: </span>
  

1.   اضغط بزر الفارة الأيمن اختر Column Preference 
2.   اضغط على علامة + 
3.   ستظهر بيانات العمود الجديد بالقائمة (بالجزء الأيسر قم باختيار الاسم المناسب للعمود مثلا source ip)
4.  بالجزء الأيمن قم ب إختيار نوع العمود وفي مثالنا هنا سيكون source address 
5. قم بالضغط على  OK  

![""](https://i.ibb.co/w4SwvvH/ezgif-com-video-to-gif.gif) 


<span style="color:DarkRed">الطريقة الثانية : </span>  
الطريقة الثانية تعتمد على نقل معلومات من خانة (Frame) ل أعمدة. 
مثال CNameString  
1.  من مربع كتابة الفلتر نقوم بكتابة kerberos.CNameString 
2. نقوم ب إختيار kerberos
3.نقوم ب إختيار as-req  
4. ثم إختيار  req-body 
5.  ثم إختيار cname
6. ثم cname-string 
7. الضغط بزر الفأرة الأيمن ثم اختيار Apply as a Column 

![""](https://i.ibb.co/MZHtDSF/CNAMEGIV.gif)



<span style="color:DarkRed"> ثانيا: تصفية النتائج (Filter) </span>  
كما ذكرت في بداية هذا الجزء، قد لا يهمنا في التحليل كل النتائج لذلك نستعين ب Filter ل تصفيه النتائج وإظهار ما نبحث عنه فقط.  
يختلف ملف pcap عن الاخر في التصفيه يعتمد على ما نبحث عنه بالضبط. 
وقد لا يسعنا في هذا المقال التحدث عنه بالتفصيل لذلك سوف أرفق ملف 
cheat sheet لمعرفة كل أنواع وأشكال ال filter ولكن سوف أقوم بذكر بعض الأمثله ،خصوصا ما قد نستخدمه ل حل هذا التحدي.  
1.  للبحث عن  IP Address 
في حالة البحث عن المرسل `ip.src == 10.20.30.227`

في حالة البحث عن المستقبل `ip.dst == 10.20.30.1`

وفي حالة البحث عن العنوان بشكل عام بدون تحديد ما اذا كان مرسل ام مستقبل `ip.addr == 10.20.30.227` 

2.  ل إظهار النتائج المرتبطه ب HTTP  يمكن فقط كتابة `http` وسيتم إظهار كل النتائج سواءاً request او accept او cookie .... الخ ويمكن ايضا تحديد بشكل مفصل جدا ما نبحث عن مثلاً نتائج request `http.request` أو نتائج ال  accept  `http.accept` 
3.  ل إظهار النتائج المرتبطه ب HTTPS `ssl.handshake.type == 1`
4. يمكن البحث ب نوع البرتوكول ب مجرد كتابة البرتوكول مثل:
`dns, ftp, arp, ssh, telnet, icmp`

سوف تتضح بعض الأمثلة الأخرى في جزئية حل التحدي في هذا المقال.  

 تجدون جميع الfilters  الممكنه  [هنا](https://www.wireshark.org/docs/dfref/). 

 وافضل ايضا الإطلاع على [هذا](https://www.cellstream.com/resources/2013-09-10-11-55-21/cellstream-public-documents/wireshark-related/83-wireshark-display-filter-cheat-sheet/file) الملف ايضا يوجد فيه بإختصار اشهر ما قد نستخدمه

 

## البرمجية الخبيثة: 
بداية، احببت ان اتحدث بشكل مبسط عن نوع البرمجية الخبيثة التي اصابت الجهاز حتى نفهم الشيء الذي نبحث عنه بشكل أفضل وكما موضح ب الصورة التالية:


  !["Alerts "](https://i.ibb.co/hCLhSGv/2020-01-30-traffic-analysis-exercise-alerts.jpg)
  
  
  الصورة ل تنبيهات 
[snort](https://sarahaljaber.com/snort/)
وهو يعد
  IDS
  

> `   ETPRO TROJAN Tordal/Hancitor/Chanitor checkin  ` 
>

>  ` ETPRO TROJAN Hancitor encrypted playloade Jan 17 (1)   `
>

Hancitor هذا النوع من البرمجيات الخبيثة يعد  حصان طروادة (Trojan Horse)، والذي تكون جزء من برنامج اخر يتم تحمليه عن طريق الانترنت او ملف من مرفقات الإيميل. 

Hancitor يكون جزء من مستند Word يتم تحمليه عن طريق مرفقات البريد الالكتروني، في اللحظة التي يتم فيها تفعيل وتشغيل الملف يتم تمرير ملفات DLL مما يسمح للمخترق بسرقة بيانات الضحية، او الوصول  ل C2 server. 

هذا ب اختصار جدا انصح بزيارة  [هذا الموقع](https://blog.malwarebytes.com/threat-analysis/2018/03/hancitor-fileless-attack-with-a-copy-trick/) لمعرفة تفاصيل تحليل هذا النوع من البرمجيات الخبيثة. 

##   خطوات حل التحدي:
بداية عند البدء بالتحليل لابد من التنبه لأمرين مهمين  


<span style="color:DarkRed">أولا: قراءة التنبيهات بشكل جيد لمعرفة نقطة البداية.  </span>  


 !["Alerts "](https://i.ibb.co/ph4bWPr/2020-01-30-traffic-analysis-exercise-alerts.jpg)
 
 

<span style="color:DarkRed">ثانيا:  وجود أسئلة محددة للإجابة عنها - لا ينصح ب التحليل بدون وجود مجموعة من الاسئله نبحث عنها من خلال تحليلنا.  </span>  

 

1. مالذي حدث ؟   
2. إيجاد البرمجية الخبيثة وتحديد ما اذا كانت فعلاً برمجية خبيثه او مجرد إنذار خاطئ 
3.  معلومات الجهاز المصاب (Host name, User name, Host Mac Address And Host IP address  ) 
4.  Indicators Of Compromise والتي تتضمن بيانات Alert لكل برمجية خبيثة و معلومات الملف المصاب ب البرمجية الخبيثة ( File name,File location and description  And Hash of the files - MD5, SHA-1 AND SHA-265 )
 
<span style="color:DarkRed">الحل: </span>  

#### إجابة السؤال الأول:  

في يوم الخميس الموافق ٣٠/٢/٢٠٢٠ جهاز بنظام windows 10  اصيب ب برمجية خبيثة من نوع Hancitor
 
 هذه المعلومات تظهر بالعاده في IDS 
وكما ذكرت اننا نستخدم snort 
في هذا المثال عند الرجوع للصوره الأولى نلاحظ وجود التاريخ وايضا اسم البرمجيه الخبيثة كما اسلفت. 
اما الجهاز المصاب ونوعه سيتضح ذلك مع بقية الأسئله.

#### إجابة السؤال الثاني: 
يظهر لنا وجود تنبيهات بخصوص تحميل ملفات ب إمتداد exe  و dll  ولمعرفتنا ب طبيعة البرمجية الخبيثة من الجزء الثالث بهذه المقالة، من السهل ان نذهب مباشره لما نريد فحصه تحديدا وهي البرامج التي تم تنزليها بجهاز الضحيه. 
بالبحث عن البيانات المرتبطه ب http  لتحديد كل ما هو مرتبط ب الملف الذي تم تنزليه ل فحصه 
من العناوين التاليه كما ظهر ب alerts  
`49.51.133.162`  
`81.177.6.156`  
`148.66.137.40`  
`184.73.165.106 `

واعيد القول ان قراءة التنبيهات بشكل جيد مهم جداً لتحليل جيد، من القراءة الجيدة نستطيع التركيز فقط هنا 
IP لان المصادر عنواين
يتماثل مع المصادر ب ال
IDS 


![](https://i.ibb.co/qr4CFYy/Screen-Shot-1441-07-16-at-10-28-15-AM.png)

 
 
 نذهب ل File ثم Export Objects  ثم http 
نبحث عن الملفات المراد فحصها ونقوم ب تحميلها   
<span style="color:DarkRed"> يرجى التنويه لعدم تشغليها بجهازك ان لم يكن جهاز مخصص للفحص  فقط قم بالتحميل </span>  
ثم الذهاب ل virustotal 
ب الإمكان رفع الملف بشكل مباشر كما يمكن البحث ب الهاش الخاص ب الملف عن طريق الأمر  

`shasum` 

![](https://i.ibb.co/x7C9741/ezgif-com-video-to-gif-1.gif)

 
![](https://i.ibb.co/Qv0kNPh/22e.gif) 
 
 

كما يظهر لنا ان الملف باسم sv.exe يعد ملف برمجية خبيثة و بنفس الطريقه نقوم من التاكد من باقي الملفات في حال تم تحميل أكثر من ملف. 


#### إجابة السؤال الثالث: 
 نبحث على الأجوبة ك التالي:  
 
<span style="color:DarkRed"> Host mac address:  
 </span>  


نبحث عن عنوان الجهاز ب كتابة

`ip.src == 10.20.30.227` 
في خانة display filter 
بخانة ال frame نختار Ethernet II 
نجد قيمة MAC Address في خانة source 
 `58:94:6b:77:9b:3c ` 
 
![](https://i.ibb.co/pP3b2Lv/mac.gif) 
 
 

<span style="color:DarkRed"> Host IP address: </span>  

 يتضح لنا  من ال alert ان الجهاز المصاب يحمل عنوان 
 `10.20.30.227`

<span style="color:DarkRed"> Host name: </span>  



1.  من مربع كتابة الفلتر نقوم بكتابة kerberos.CNameString 
2. نقوم ب إختيارkerberos
3. نقوم ب إختيار as-req  
4. ثم إختيار  req-body 
5.  ثم إختيار cname
6. ثم cname-string 
7. الضغط بزر الفأرة الأيمن ثم اختيار Apply as a Column

هذا هو اسم الجهاز المصاب `DESKTOP-4C02EMG$`


![""](https://i.ibb.co/y8Fn9Sx/Screen-Shot-1441-07-16-at-10-00-55-AM.png)

 `بالإمكان ايضا ايجاد اسم المستخدم ب البحث عن `
 
 `dhcp أو smb`
 
<span style="color:DarkRed">User name:</span>  


بنفس طريقة إيجاد Host name  ولكن اسم الجهاز ينتهي دائما ب $ بينما اسم المستخدم لا ينتهي ب علامة $ 
وهذا اسم المستخدم ل الجهاز المصاب

`alejandrina.hogue`

 ![](https://i.ibb.co/H2d7zKZ/Screen-Shot-1441-07-16-at-10-02-13-AM.png)
 
 
#### إجابة السؤال الرابع: 
في هذا الجزء نحتاج لتحديد كافة التفاصيل حول الملف والمواقع التي تم زياراتها وعنوانيها ..الخ 


1. اسم الملف `sv.exe`

`sha: 8ffdeebf7b41fe65b2d92eed18ddd6c39eeea2d8
`
2. تحديد مصدر الملف` http://gengrasjeepram.com/sv.exe`
![](https://i.ibb.co/GcNBHy5/src.gif)

 
3.  تحديد http methods  
أ. Follow http stream   
ب. او من تفاصيل  Frame مباشرة 
`GET /sv.exe`. 
4.  عنوان مصدر الملف و ال port   
`49.51.133.162 port 80 `

ل تكن الإجابة كاملة   

 `49.51.133.162 port 80 - gengrasjeepram.com - GET /sv.exe. `


نكمل مع باقي ال Indicators   
 `184.73.165.106 port 80 - api.ipify.org - GET /. `
`81.177.6.156 port 80 - twereptale.com - POST /4/forum.php `
`81.177.6.156 port 80 - twereptale.com - POST /mlu/forum.php `
`81.177.6.156 port 80 - twereptale.com - POST /d2/about.php `
`148.66.137.40 port 80 - xolightfinance.com - GET /bhola/images/1 `
`148.66.137.40 port 80 - xolightfinance.com - GET /bhola/images/2 `


### ختاما
#### مراجعة 
بعد قراءة التنبيهات نقوم بفحص الملف للتأكد من الانذار صحيح وليس انذار خاطئ 

تحميل الملف وتحليله 

يوجد عده طرق للتحليل استعنت في هذا المثال ب viurstotal

بعد التاكد من ان الملف مصاب نقوم ب جمع باقي البيانات حول الملف والجهاز المصاب وكل ما هو مرتبط به 

اود التوضيح انه قد يختلف التحليل من ملف لأخر الى ان الأسلوب غالبًا متشابهه

ختاماً  

حقوق النسخ محفوظة 
 
شكرأ لقرائتك
تحياتي
سارة الجابر
