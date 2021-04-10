--- 
layout: post
title: Windows Examination (تحليل النظام)
---





![](https://mk0resourcesinf5fwsf.kinstacdn.com/wp-content/uploads/2020/10/forensic-windows12302013.jpg
)
أهلا بكم في هذه المقالة سوف نتحدث عن Windows Examination  (تحليل نظام الوندوز)


## عناصر المقالة 
- مقدمة 
- التحليل

## مقدمة 
تحليل الوندوز من أهم الخطوات في عملية الإستجابة للحوادث الرقمية بعملية تحليل النظام بشكل سليم سوف نحصل على إجاب لعدة أسئلة أهمها التأكد من الحادثة وما إذا كان الجهاز مصاب أو لا.  
يمكننا  تحليل النظام بعدة طرق مختلفة لكن في هذه المقالة سوف أستعين بعدة أوامر مهمة تمكننا من الحصول على إجابات وافية وتحليل مبدئي سليم.   

## التحليل 
## <span style="color:red"> Network</span>


 نبدأ عملية التحليل ب البحث عن Listener بإستخدام أداة netstat 
 
بكتاب الأمر التالي سوف نحصل على Active connection سوأ على UDP port أو TCP port   وعنوان الانترنت (IP) المرتبط فيه والحالة 
- Listening
- Established
-  Close_wait 
- Time_wait
- .... الخ  

`netstat -na`   
 
![](https://i.ibb.co/QK8bppM/1.png) 

 يمكننا معرفة تفاصيل الحالة  [من هنا ](https://docs.oracle.com/cd/E88353_01/html/E72487/netstat-8.html)

يمكننا أيضا إستخادم الأمر   
`netstat -nao`   

![](https://i.ibb.co/3BRjVX8/2.png)
يمكننا بإستخدام الأمر التالي معرفة ملفات exe, dll  المرتبطة بأي Listening port  
`netstat -naob`   

![](https://i.ibb.co/26MGxKT/3.png)
## <span style="color:red"> Process</span>

من الممكن معرفة Process التي تعمل في النظام بعدة طرق من الممكن نبدأ بإستخدام الأمر  
`taskmgr`   
 
![](https://i.ibb.co/wrY689w/4.png)

أيضًا نستخدم الأمر بإستخادم هذا الأمر سوف نحصل على معلومات مهمة مثل: 
- Image name 
- PID 
- user name   
`tasklist /v `   

 
![](https://i.ibb.co/fq4bqz4/5.png)


ممكن أيضا الاستعانة ب استخدام wmic للحصول على معلومات أكثر ومهمة ل كل process  
`wmic process list full`   
   
![](https://i.ibb.co/fk0ht9x/6.png)

وسوف نحصل على المعلومات التالية لكل process 

-  CommandLine   
-  CSName   
-  Description   
-  ExecutablePath   
-  ExecutionState   
-  Handle   
-  HandleCount   
-  InstallDate   
-  KernelModeTime   
-  MaximumWorkingSetSize   
-  MinimumWorkingSetSize   
-  Name   
-  OSName   
-  OtherOperationCount   
-  OtherTransferCount   
-  PageFaults   
-  PageFileUsage   
-  ParentProcessId   
-  PeakPageFileUsage   
-  PeakVirtualSize   
-  PeakWorkingSetSize   
-  Priority   
-  PrivatePageCount   
-  ProcessId   
-  QuotaNonPagedPoolUsage   
-  QuotaPagedPoolUsage   
-  QuotaPeakNonPagedPoolUsage   
-  QuotaPeakPagedPoolUsage   
-  ReadOperationCount   
-  ReadTransferCount   
-  SessionId   
-  Status   
-  TerminationDate   
-  ThreadCount   
-  UserModeTime   
-  VirtualSize   
-  WindowsVersion   
-  WorkingSetSize   
-  WriteOperationCount   
-  WriteTransferCount   

يمكن أيضا إستخدام wmic بشكل محدد أصر كأن نبحث عن تفأصيل process معينة  وتحديد قيم المخرجات التي نريد النظر فيها 
بكتابة الاوامر التالية 
- بالبحث عن الأسم:   
`wmic process where name="svchost.exe" list full`   

![](https://i.ibb.co/YQd7nRG/7.png)

- بالبحث عن رقم process:  
`wmic process where parentprocessid=8 list full`    

8

![](https://i.ibb.co/kghSTCZ/8.png)

- بالبحث عن parentprocess:    

`wmic process where processid=10452 get name,commandline,processid,parentprocessid`


 10   

![](https://i.ibb.co/Sr6NHhm/10.png)

 
 يمكن أيضا تحديدد المخرجات المراد النظر فيها بشكل خاص بإستخدام get ومن ثم كتابة المخرجات يفصل بينها , 

` wmic process where processid=23580 get name,commandline,processid,parentprocessid`    

9 
![](https://i.ibb.co/XxdztP7/9.png)



 

## <span style="color:red"> Accounts  </span>
يمكن البحث عن الحسابات المرتبطة ب الجهاز تحديداً ك administrators بإستخدام هذا الأمر, هذا الامر يساعدنا في ما اذا كان هنا اثر لحساب مشبوه لم يتم إنشاءة من قبل المستخدم أو Admin 
`net localgroup administrators`
13
![](https://i.ibb.co/7CzMTWd/13.png)



استعرضت في هذه المقالة بعض الأوامر التي من الممكن الاستفادة منها في عملية تحليل مبدية لا يمكن أن تكون هذه الأوامر كافية في   بل قد تساعدننا من معرفة بعض الأمور المتعلقة ب الحادثة وبنأ تصور مبدأي للحادثة.

قرأة وتعلم ممتع و ل السؤال والاستفسار يمكنكم إيجادي بتويتر على الحساب @s4o_o

جميع الحقوق محفوظة

سارة آل جابر
