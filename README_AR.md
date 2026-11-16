<style>body{direction:rtl;text-align:right}pre,code{direction:ltr;unicode-bidi:embed}table{direction:rtl;width:100%}</style>
<div dir="rtl">

# سكرتير العيادة على واتساب — C06 `0.1.0-review`

حزمة بروفة تركيب ببيانات وهمية. السكرتير بيرد على الأسعار والمواعيد وبيسجّل زيارة ليوم مفتوح. تأكيد الزيارة يطلع بعد حفظ الصف وقراءته. Telegram اختياري، وEvolution هنا وصلة واتساب غير رسمية. نسخة المراجعة لسه محتاجة تجربة فعلية من الصفر قبل التوزيع.

لإنشاء جدول الحالة والنسخ بسهولة، افتح `START_HERE_AR.html` في Chrome أو Edge. الصفحة بتعرض كل اسم وقيمة بزر نسخ، وبتشتغل محليًا من غير اتصال.

عندك n8n وEvolution جاهزين؟ ابدأ من الخطوات تحت. لو واحدة ناقصة، افتح [تجهيز البيئة](BOOTSTRAP_AR.md). لو عايز مساعد يمشي معاك، افتح [برومبت التركيب](INSTALL_ASSISTANT_AR.md). الحزمة محتاجة Google Sheets وGemini API، وبتستخدم n8n فيه Data tables. إصدار n8n المعتمد هيتحدد من البروفة.

## ١. n8n — استيراد وتهيئة مرة واحدة

1. في n8n التجريبي، اختار **Import from File** واستورد `01-state-engine.json` ثم `02-clinic-whatsapp.json` في نفس المشروع. خليهُم غير نشطين أثناء الربط.
2. من **Data tables** اضغط **Create data table** واختار **From scratch** وسمّي الجدول `C06 Public State`. اضغط **Add Column** لإضافة `revision` بنوع Number، و`payload` و`mutation` بنوع String. بعدها اضغط **Add Row** واملأ القيم الجاهزة للنسخ تحت. بنية الجدول وصف البداية موجودين كمان في `state-seed.json`. اتأكد إن n8n أعطى الصف الأول `id=1`. التهيئة مرة واحدة لجدول جديد، والشغل الحالي يستكمل من جدوله القائم.
3. في المحرك اختار جدول `C06 Public State` في **Read State 1–3** و**Save State 1–3**. في الرئيسية اربط الـWorkflow الداخلي في **Begin Direct State** و**Prepare Direct Reply** و**Confirm Visit Sheet Save** و**Claim Telegram** و**Confirm Telegram Delivery** و**Claim WhatsApp** و**Confirm WhatsApp Delivery** و**Release Failed Conversation**. لو المساعد عنده وصول مباشر يقدر يضبط كل المراجع دي دفعة واحدة.

### صف البداية — انسخ من هنا

الخطوات والنصوص الثابتة للتركيب موجودة في الدليل ده داخل ZIP وعلى GitHub. افتح `INSTALL_ASSISTANT_AR.md` لو بتستخدم مساعد، وافتح `state-seed.json` لو بتراجع البنية. ملفا `01-state-engine.json` و`02-clinic-whatsapp.json` للاستيراد في n8n. القيم التالية تتلصق في خلايا جدول الحالة.

| العمود | النوع | قيمة صف البداية |
|---|---|---|
| `revision` | Number | `0` |
| `payload` | String | انسخ سطر JSON التالي كاملًا في خلية واحدة. |
| `mutation` | String | `initialize` |

```json
{"schema":1,"counters":{},"sessions":{},"events":{},"visits":{},"next_sheet_row":2}
```

ده هو **نص payload وحده**. لو فتحت `state-seed.json` هتلاقي ملفًا أكبر بيشرح الجدول وصف البداية، فانسخ قيمة الخلية من السطر الجاهز هنا. عمود `id` المتوقع قيمته `1`، و`id` و`createdAt` و`updatedAt` بيجهزهم n8n تلقائيًا.

[استيراد ملفات n8n](https://docs.n8n.io/workflows/export-import/) · [جداول n8n](https://docs.n8n.io/data/data-tables/)

## ٢. الشيت

حوّل `clinic-sheet.xlsx` إلى Google Sheets في حساب التجربة. عدّل ملفك وحافظ على أسماء التابات والهيدر. `clinic-sheet.xlsx` بيبدأ بتاب الزيارات فيه الهيدر فقط، والصفوف الجديدة تظهر مباشرة.

| التاب | اللي بيتعدل |
|---|---|
| بيانات العيادة | العمود `B` قدام `name` و`address` و`location_url`. سيب `timezone` على `Africa/Cairo` في نسخة الديمو. اللوكيشن اختياري. |
| الخدمات | معرف خدمة فريد، الاسم، سعر رقمي لو متاح، عملة `EGP`، وصف مختصر، و`active` للتفعيل. عمود المدة اختياري. |
| ساعات العمل | اسم اليوم، `opens` و`closes` كنص مثل `17:00`، و`open` أو `closed`. |
| الإجازات | لو فيه استثناء أضف تاريخًا كنص `YYYY-MM-DD` ومواعيد الاستثناء أو `closed`. الاستثناء له أولوية. |
| الزيارات | سجلات بتتكتب تلقائيًا. خلي الصفوف مرئية وسيب الترتيب الأصلي للصفوف قبل وأثناء التشغيل. |

حط معرّف نسخة الشيت في `Settings → spreadsheet_id`. اربط **Google Sheets OAuth2 API** في **Read Clinic Sheets Direct** و**Save Visit To Google Sheets** و**Read Saved Visit Back** و**Save Notification Status**. حساب Google المفوِّض يحتاج صلاحية تعديل النسخة التجريبية. في الاستضافة الذاتية استخدم اعتماد Google خاص بالبيئة حسب دليل n8n. تأكد من طريقة ربط الاعتماد بعقد HTTP Request على إصدارك قبل تفعيل الديمو. [اعتماد Google](https://docs.n8n.io/integrations/builtin/credentials/google/oauth-single-service/)

## ٣. الاعتمادات وتوجيه واتساب

| الجزء | في n8n | صاحب الحساب يعمل إيه |
|---|---|---|
| Gemini | في **Google Gemini API** اختار Header Auth واسمه `x-goog-api-key`. الموديل المقترح في `Settings → google_model` هو `gemini-3.5-flash-lite`. | يحط مفتاح Gemini Developer API جوه الـCredential. |
| Evolution إرسال | في **Send WhatsApp** استخدم Header Auth اسمه `apikey`. في Settings اضبط `evolution_base_url` و`evolution_instance`. | يربط جلسة اختبار من الموبايل ويحط مفتاح Evolution جوه الـCredential. |
| Evolution استقبال | في **Evolution Webhook** اربط Header Auth خاص بالاستقبال. Settings فيها `allowlist` بصيغة `digits@s.whatsapp.net` لأرقام الاختبار. | يدخل قيمة الهيدر في n8n وEvolution. جلسة الاختبار تستخدم عنوان Webhook واحد لحدث `MESSAGES_UPSERT`. |
| Telegram | اختياري. اربط اعتماده في **Send Telegram** و**Send Technical Alert**. اضبط `telegram.enabled` و`telegram.chat_id` عند الاستخدام. | يجهز البوت ووجهة اختبار ويدخل التوكن في الـCredential. |

التعاملات الحية تستخدم بيانات وهمية. استخدم نسخة Workflow جديدة وجدول حالة وشيت وجلسة واتساب مخصصين للتجربة. ينفع تستورد الملفين على n8n الموجود عندك وتربط Evolution الموجود. عند اكتمال الربط، فعّل المحرك ثم الرئيسية بعد فتح `demo_enabled` للرقم المحدد، واضبط في Evolution عنوان **Production Webhook** للجلسة المخصصة. تحقق إن `byEvents` مقفول لو Evolution عندك بيضيف اسم الحدث لمسار الرابط عند تفعيله. [Gemini API](https://ai.google.dev/gemini-api/docs/api-key) · [Evolution](https://github.com/evolution-foundation/evolution-api)

## ٤. أول تجربة

اتبع [خطوات قبول أول زيارة](FIRST_TEST_AR.md). كارت الزيارة بيعرض رقم اليوم وساعات الاستقبال. توقيت الكشف بيتنظم في العيادة. لإيقاف استقبال تجربة واتساب، قفل الـworkflow الرئيسي في n8n، وبعد التأكد إن آخر تنفيذ خلص أوقف توجيه الجلسة التجريبية عند الحاجة.

</div>
