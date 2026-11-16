<style>body{direction:rtl;text-align:right}pre,code{direction:ltr;unicode-bidi:embed}</style>
<div dir="rtl">

# سكرتير العيادة على واتساب

حزمة تعليمية مجانية ببيانات عيادة وهمية. تستورد ملفين على n8n، وتربط Google Sheets وGemini Developer API وEvolution الموجود عندك. تنبيه Telegram اختياري.

**الإصدار الحالي `0.1.0-review` نسخة مراجعة قبل القبول.** اتراجعت الملفات محليًا، وأول تركيب فعلي ورد Gemini ورحلة الزيارة كاملة لسه منتظرين التجربة. الإصدار ظاهر كـPre-release.

## تنزيل الحزمة

من [صفحة الإصدارات](https://github.com/islamelwazzer-7/clinic-whatsapp-n8n/releases) افتح `v0.1.0-review`، وتحت **Assets** نزّل الملف المرفق `C06-public-setup-v0.1.0-review.zip`. فك الضغط، وبعدها افتح `README_AR.md` وابدأ استيراد `01-state-engine.json` ثم `02-clinic-whatsapp.json`.

## محتويات ZIP

| الملف | وظيفته |
|---|---|
| `01-state-engine.json` | محرك الحالة والعداد ومنع التكرار. |
| `02-clinic-whatsapp.json` | استقبال الرسائل وفهمها وقراءة الشيت وحفظ الزيارة وإرسال التأكيد. |
| `clinic-sheet.xlsx` | قالب شيت بخمس تابات وبيانات وهمية. الزيارات هيدر فقط. |
| `state-seed.json` | أعمدة جدول الحالة وصف البداية. |
| `README_AR.md` | دليل التركيب. |
| `INSTALL_ASSISTANT_AR.md` | برومبت المساعد، ويغطي Evolution الموجود أو المتعطل أو الناقص. |
| `BOOTSTRAP_AR.md` | تجهيز الخدمات الناقصة عند الاحتياج. |
| `FIRST_TEST_AR.md` | اختبارات قبول أول تشغيل. |

ينفع تستخدم n8n وEvolution الموجودين عندك. نسخة n8n لازم تدعم Data tables. ملفات الـworkflow بتبدأ غير نشطة، وتضيف اعتمادات حساباتك من داخل n8n أثناء التركيب. أدخل المفاتيح ومسح QR وموافقات الحساب خارج تسجيل الشاشة.

[دليل التركيب](README_AR.md) · [برومبت المساعد](INSTALL_ASSISTANT_AR.md) · [أول تجربة](FIRST_TEST_AR.md)

</div>
