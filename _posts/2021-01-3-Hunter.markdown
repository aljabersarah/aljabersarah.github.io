--- 
layout: post
title: حل تحدي  Hunter من  منصة Cyberdefenders  
---

![](https://isolutions.com.ua/wp-content/uploads/2020/04/DigitalForensics.png)
السلام عليكم ورحمة الله وبركاته 
 

## مقدمة 

هذه المقالة ستكون مخصصة لحل تحدي  Hunter من منصة cyberdefenders.org 
التحدي عبارة عن 30 سوال للاجابة على هذه الأسئلة يتطلب منا تحليل كامل ل Windows Image  
ب البداية نستعين ب أداة AccessData FTK Imager لفتح image  والبدء ب استخراج Artifacts 

رابط التحدي

 https://cyberdefenders.org/labs/32 


## الحل 
من السؤال الأول حتى السؤال الثامن سوف نجد الأجوبة من خلال  تحليل ملفات Windows Registry (سجل النظام)  
ب الاستعانة ب أداة  `(Registry Explorer )` و أداة `(Reg Ripper)`

أولا من خلال تحليل Image  بإستخدام FTK Imager  سوف نجد ملفات Windows Registry ب المسارين 
`(%Windir%\System32\Config)` و `(%UserProfile%\)`

 
1- `(What is the computer name of the suspect machine)`

 يمكننا استخراج اسم الكمبيوتر من خلال  البحث ب هذا المسار   
`(HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\ComputerName\ComputerName)`

لنجد الإجابة كما هو موضح بالصورة  `4ORENSICS`
![](https://i.ibb.co/hWLn0B8/q1.png)

2- `(What is the computer IP)`

 لحل هذا السؤال يمكننا ايضًا أن نستعين ب Windows Registry (سجل النظام)  
هذه المرة سوف نبحث بهذا المسار 
`(HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\{***********})`

![](https://i.ibb.co/PW0wVgk/Q2.png)

3-  `(What was the DHCP LeaseObtainedTime)`

لحل هذه السؤال هذه المرة  أيضًا سنجد الإجابة في هذا المسار

`(HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Tcpip\Parameters\Interfaces\{****************})`
![](https://i.ibb.co/LPDQ6CJ/Q3.png)

نجد الإجابة 
`(1466475852)`
ومن ثم نستعين ب

 https://www.epochconverter.com/ 
للتحويل ل قيمة مقرؤة 

![](https://i.ibb.co/mR95B30/q3-con.png)


4- `(How many times did this user log on to the computer)`

لحل هذا السؤال سوف أقوم بالاستعانة بأداة `(Reg Ripper)` 

وسوف نقوم تحليل مفتاح SAM  تحديدًا 
![](https://i.ibb.co/kmsVjcB/q4-reg.png)

لنجد الإجابة عدد مرات الدخول 3 مرات 

![](https://i.ibb.co/kgPYYwr/q4.png)

5- `(What is the computer SID)`

نستطيع استخراج computer SID من خلال البحث في مسار 
`(HKEY_LOCAL_MACHINESAM\SAM\Domains\Account)`

![](https://i.ibb.co/4gfYbb8/Q5.png)

هنا سوف نجد قيمتين F و V 
يهمنا في هذه الحالة قيمة V  

نقوم ب أخذ اخر 96 بت

2E-D9-61-94-33-5A-2B-A4-80-82-5C-2A
 ثم نقوم بحساب SID  بإتباع الخطوات التالية 


أولًا: نقوم بتقسيمها الى 3 أجزاء

2E-D9-61-94 33-5A-2B-A4 80-82-5C-2A


ثانيًا: نقوم بترتيب البايت بالعكس ل كل قسم

`(2E-D9-61-94  >> 94-61-D9-2E )`

`(33-5A-2B-A4 >> A4-2B-5A-33 )`

`(80-82-5C-2A >> 2A-5C-82-80 )`

`(94,61,D9,2E-A4,2B,5A,33-2A,5C,82,80 )`


ثالثًا: نقوم بتحويل كل قسم الى نظام العد العشري (decimal)

`(2489440558-2754304563-710705792)`

ثم نقوم بإضافة machine SID prefix

`(S-1-5-21-2489440558-2754304563-710705792)`


6- `(What is the Operating System(OS) version)`

نجد هذه الإجابة في هذا المسار
`(HKEY_LOCAL_MACHINESAM\SOFTWARE\Microsoft\Windows NT\CurrentVersion)`

![](https://i.ibb.co/kKNTjZh/q6.png)

7- `(When was the last login time for the discovered account)`

لحل هذا السؤال سوف أقوم بالاستعانة بأداة `(Reg Ripper)`  
وسوف نقوم تحليل مفتاح SAM  تحديدًا 
![](https://i.ibb.co/zQtYVRw/q8.png)

8- `(There was a “Network Scanner” running on this computer, what was it And when was the last time the suspect used it)`

لحل هذا السؤال استعنت ب 
ملفات `(Prefetch)`  حيث قمت بالبداية باستخراجها من 
مجلد `(windows/Prefetch)` باستخدام أداة `(FTK Imager)` ومن ثم استعنت بأداة `(winprefetchview)` للتحليل 
لنجد أن الإجابة  `(zenmap.exe)`

![](https://i.ibb.co/tDGwnNx/q9.png)



9- `(When did the port scan start and end)`

من المعروف أن من خلال التقرير الناتج عن أداة  `(zenmap)` سوف نجد وقت بدء عملية المسح و كم المدة المستغرقة في البحث. 
 وعند البحث في ملفات المستخدم `(Hunter)` تحديدًا في `(Desktop)` 
نجد ملف بإسم `(nmapscan.xml)` وهو ملف التقرير الناتج عن المسح  
نقوم بإستخراج الملف لقراءة محتواة 
![](https://i.ibb.co/jZR8pFx/q10.png)

سوف نجد وقت بدء عملية المسح, المدة المستغرقة و وقت انتهاء عملية المسح 

وقت البدء: 
![](https://i.ibb.co/ypKjqVv/Q10-1.png)

وقت الإنتهاء:
![](https://i.ibb.co/ypKjqVv/Q10-1.png)

10- `(How many ports were scanned)`

في نفس الملف المذكور ب السؤال السابق سنجد كم `(Ports)` تم مسحه 
![](https://i.ibb.co/7JkQxT7/q11.png)

11- `(What ports were found “open”)`

أيضا بنفس الملف سوف نجد  عدد `(Ports)` تم  ايجادة في حالة `(Open)` 
![](https://i.ibb.co/ZRrcHPT/q12.png)
 
12- `(The employee engaged in a Skype conversation with someone. What is the skype username of the other party)`

لحل هذا السؤال قمت بالاستعانة بأداة `(skyperious)`
لكن قبل ذلك قمت ب إستخراج ملف `(skype)` 
من `(users\hunter\AppData\Roaming)` 
![](https://i.ibb.co/CW9xT08/q13-1.png)


ومن ثم  قمت بفتح ملفات `(skype database)` ب استخدام أداة `(skyperious)`
![](https://i.ibb.co/Cbvn3fL/Q13.png)

لتكون الإجابة على هذا السؤال `(linux rul3z)` 

13-  `(What is the name of the application both parties agreed to use to exfiltrate data and provide remote access for the external attacker in their Skype conversation? And when did the suspect run it)`

ب إستخدام نفس الأداة `(skyperious)` نستطيع الوصول للمحادثات التي تمت من خلال 
`(skype)` وبالتالي سوف نجد إجابة هذا السوال والبرامج الذي اتفق عليه الطرفين 
لنقل الملفات هو  `(team viewer)`
![](https://i.ibb.co/HrKktJ9/Q14.png)

14-  `(What is the Gmail email address of the suspect employee)`

ايضا لحل هذا السؤال استعنت ب اداة `(skyperious)` 
من خيار `(information)` سوف نجد بيانات المستخدمين متضمنة عنوان البريد الإالكتروني 
![](https://i.ibb.co/nmXc7QD/Q15.png)

15- `(It looks like the suspect user deleted an important diagram after his conversation with the external attacker. What is the file name of the deleted diagram)`

استعنت لحل هذه السؤال ب أداة `(Stellar Repair for Outlook)`  لتحليل ملفات البريد الالكتروني  `(PTS / Outlook)`
ب البداية قمت بإستخراج ملفات  `(pts)`  من `(users\hunter\Documents)`
![](https://i.ibb.co/Y8xGTBz/q16-1.png)

سوف نجد الإجابة في ملف الايميلات المحذوفة 
![](https://i.ibb.co/6ZZfs8x/q16.png)


16- `(The user Documents’ directory contained a PDF file discussing data exfiltration techniques. What is the name of the file)`

من السؤال المطلوب البحث في مجلد `(Documents)` عند استخراج كل ملفات `(pdf)` من المجلد وفتحها واحد تلو الاخر سنجد أن الملف المقصود هو كما موضح بالصورة 
![](https://i.ibb.co/z72LR2Y/q17.png)

17- `(The suspect user downloaded a Nmap installer. What version did he download)` 

عند تحليل ملف `(nmapscan.xml)`  المذكور بالسؤال التاسع سوف نجد ان الملف يشير الى نسخة 
`(nmap)`  المستخدمة 
![](https://i.ibb.co/5xH1HVh/q18.png)

18-  `(What are the serial numbers of the two identified USB storage)` 

لحل هذا السؤال سوف نستعين بملفات  `( Windows Registry)`
تحديدًا ب المسار التالي  `(HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Enum\USB\)` 
![](https://i.ibb.co/9YkrjrH/q20.png)

19- `(How many prefetch files were discovered on the system)`

لحل هذا السؤال سوف استعين ب أداة `(winprefetchview)`
من ثم بعد رفع ملفات `(Prefetch)`  وحذف الملفات ب الحجم صفر سيتبقى لنا العدد 174 
![](https://i.ibb.co/YcL39JL/q22.png)

20- `(The suspect employee tried to exfiltrate data by sending it as an email attachment. What is the name of the suspected attachment)`

كما في السؤال الخامس عشر استعنت لحل هذه السؤال ب أداة `(Stellar Repair for Outlook)`  لتحليل ملفات البريد الالكتروني   
لتكون الإجابة  `(Pictures.7z)`
![](https://i.ibb.co/KztQTRC/q26.png)

21- `(Shellbags shows that the employee created a folder to include all the data he will exfiltrate. What is the full path of that folder)`

لحل هذا السؤال قمت بإستخراج  مفتاح  `(USRClass.dat )`  
من `(%UserProfile%\)` 
من ثم الاستعانة بأداة  `(ShellBagsExplorer)`  لفتح الملفات و تحليلها 
لنجد الإجابة `(Exfil)`
![](https://i.ibb.co/2YcScsF/q27.png)


22- `(Provide the name of the directory where information about jump lists items (created automatically by the system) is stored?)`

يمكننا أن نجدها في المسار 
`(%USERPROFILE%\AppData\Roaming\Microsoftg\Windows\Recent\)`
لتكون  الإجابة   `(AutomaticDestinations-ms)`
![](https://i.ibb.co/wJyzB8K/q29.png)


23-  `(Using JUMP LIST analysis, provide the full path of the application with the AppID of “aa28770954eaeaaa” used to bypass network security monitoring controls.)`


أخيرا لتحليل ملفات `( JUMP LIST)` سوف اقوم باستخراجها من   
`(%USERPROFILE%\AppData\Roaming\Microsoftg\Windows\Recent\)`
من ثم سوف نستعين بأداة `( JumpListExplorer)`  لتحليل  سوف نقوم ب البحث عن AppId كما هو مذكور ب السوال لتكون الإجابة 

![](https://i.ibb.co/P9ZhPZz/q30.png)



الى هنا نكون وصلنا لنهاية المقال أرجو أن أكون وفقت في كتابته 

كل الشكر ل الدكتور علي هادي و لمنصة cyberdefenders.org
لمثل هذه المبادرات التي من شأنها المساهمة في تطوير مهارات التحليل. 


 

 
