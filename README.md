# 📁 FileSystemManager for ASP.NET Core

مكتبة متكاملة لإدارة الملفات في مشاريع ASP.NET Core، تم تصميمها لتسهل التعامل مع نظام الملفات من خلال API شاملة تدعم رفع، حذف، نسخ، نقل، ضغط، وتحويل الملفات بجودة عالية.

## الميزات

- رفع ملف واحد أو عدة ملفات دفعة واحدة.
- حذف ملفات مفردة أو متعددة.
- ضغط مجموعة من الملفات داخل ملف ZIP.
- دعم نظام الملفات المؤقتة (مجلد Temp).
- نقل ونسخ الملفات من مجلد إلى آخر.
- تحويل صور متعددة إلى ملف PDF.
- واجهات API مرنة وسهلة التوصيل مع أي واجهة مستخدم.

## بنية ملفات المشروع المقترحة

/Uploads 
/Temp
/Zipped
/Pdf


## المتطلبات

- .NET 6 أو أعلى
- الحزم المستخدمة:
  - `PdfSharpCore` لتحويل الصور إلى PDF
  - `Microsoft.AspNetCore.Http` للتعامل مع الملفات المرفوعة

## دليل الاستخدام

### رفع الملفات

- **رفع ملف واحد** عبر endpoint:
POST /upload
Content-Type: multipart/form-data
Field: file


- **رفع عدة ملفات دفعة واحدة**:
POST /upload-multiple
Content-Type: multipart/form-data
Field: files[]


### حذف الملفات

- **حذف ملف واحد**:
DELETE /delete?fileName=example.pdf

- **حذف عدة ملفات دفعة واحدة**:
POST /delete-multiple
{
"fileNames": ["file1.pdf", "file2.docx"]
}


### نقل أو نسخ الملفات

- **نسخ ملفات من مجلد إلى آخر**:
POST /copy-multiple
{
"fileNames": ["doc1.pdf", "doc2.pdf"],
"sourceFolder": "Temp",
"destinationFolder": "Permanent"
}


- **نقل ملفات من مجلد إلى آخر**:
POST /move-multiple
{
"fileNames": ["doc1.pdf"],
"sourceFolder": "Temp",
"destinationFolder": "Permanent"
}


### ضغط الملفات

- **ضغط ملفات داخل ملف ZIP**:

POST /zip-files
{
"fileNames": ["a.pdf", "b.pdf"],
"sourceFolder": "Temp"
}


يتم حفظ الملف المضغوط داخل مجلد `/Zipped`.

### الملفات المؤقتة

- يتم حفظ الملفات مؤقتًا داخل مجلد `Temp`.
- ينصح بإضافة BackgroundService لحذف الملفات المؤقتة القديمة كل فترة زمنية.
- يتم نقل الملفات إلى مجلد دائم (مثل `Permanent`) بعد المعالجة أو الاستخدام.
  

### تحويل الصور إلى PDF

- **إرسال عدة صور لدمجها في ملف PDF**:

POST /images-to-pdf
Content-Type: multipart/form-data
Field: images[]


الصور تُدمج في ملف PDF واحد بالحجم الكامل لكل صورة، ويتم إرجاع الملف الناتج للمستخدم.

## ملاحظات أمنية

- يجب التحقق من امتدادات الملفات المرفوعة والتأكد من السماح فقط بـ (PDF, JPG, PNG, DOCX...) لتفادي رفع ملفات ضارة.
- استخدم `Path.GetFileName()` لعزل اسم الملف ومنع هجمات المسارات.
- حدّد الحجم الأقصى للملفات في إعدادات `IFormFile` عبر `RequestSizeLimit`.
- تأكد من صلاحيات الكتابة للمجلدات المستخدمة على السيرفر أو الاستضافة.

## المساهمة

- مرحّب بالمساهمات في المشروع من خلال Fork → Branch → Pull Request.
- تأكد من كتابة كود نظيف، واستخدام نمط واضح في التسمية وتنظيم الملفات.

## المؤلف

تم تطوير هذه المكتبة بواسطة Yousef Ibrahim بهدف توفير حل شامل ومرن لإدارة الملفات في تطبيقات ASP.NET Core.  
للتواصل أو الدعم، لا تتردد في فتح Issue أو المساهمة مباشرة على GitHub.


