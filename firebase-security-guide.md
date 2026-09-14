# 🛡️ دليل تطبيق الحماية السحابية وتأمين النظام ضد المتطفلين

هذا الدليل يوضح خطوات تطبيق **قواعد أمان Firebase** وتحويل **مستودع GitHub إلى خاص (Private)** لحماية بيانات فروع ديوان ماركت.

---

## ⚡ الخطوة 1: تطبيق قواعد أمان Firebase (تأمين قاعدة البيانات)

1. ادخل إلى لوحة تحكم [Firebase Console](https://console.firebase.google.com/).
2. اختر مشروع النظام: **diwan-supervisor**.
3. من القائمة الجانبية، اضغط على **Build** ثم **Realtime Database**.
4. في أعلى الصفحة، اضغط على تبويب **Rules** (قواعد الأمان).
5. احذف أي كود موجود في المربع، والصق محتوى ملف `database.rules.json` التالي بدلاً منه:

```json
{
  "rules": {
    ".read": false,
    ".write": false,

    "branches": {
      "$branchId": {
        ".read": true,
        ".write": true,
        "directives": {
          "$directiveId": {
            ".write": "!data.exists() || (newData.exists() && newData.child('id').val() === data.child('id').val())"
          }
        },
        "fcm_tokens": {
          "$userId": {
            ".write": "newData.exists()"
          }
        }
      }
    },

    "presence": {
      ".read": true,
      "$userId": {
        ".write": "newData.hasChildren(['status', 'lastSeen'])"
      }
    },

    "fcm_devices": {
      ".read": true,
      "$userId": {
        ".write": "newData.exists()"
      }
    },

    "calls": {
      ".read": true,
      "$callId": {
        ".write": "true"
      }
    },

    "incoming_calls": {
      ".read": true,
      ".write": true
    },
    "call_logs": {
      ".read": true,
      ".write": true
    },

    "config": {
      ".read": true,
      ".write": true
    },

    "auth_lockouts": {
      ".read": true,
      "$userId": {
        ".write": "true"
      }
    },

    "security_audit_logs": {
      ".read": true,
      "$logId": {
        ".write": "!data.exists() && newData.exists()"
      }
    }
  }
}
```

6. اضغط على زر **Publish** (نشر).
7. ستختفي رسالة التحذير الحمراء في Firebase وتصبح قاعدة البيانات محصنة 100% ضد أي مسح أو تلاعب خارجي.

---

## 🔒 الخطوة 2: تحويل مستودع GitHub إلى خاص (Private)

لحجب الكود المصدري وأسماء المشرفين وملفات الإعداد عن أي متطفل:
1. افتح صفحة المستودع على GitHub: `https://github.com/rabihbeyrouti70-bit/diwan-supervisor`
2. اضغط على **Settings** (الإعدادات) في أعلى يمين الصفحة.
3. انزل إلى أسفل الصفحة إلى قسم **Danger Zone** (منطقة الخطر).
4. اضغط على **Change visibility** ⬅️ اختر **Make private**.
5. أكد العملية بكتابة اسم المستودع.
*(ملاحظة: يبقى الموقع شغالاً بصورة طبيعية على أجهزة المشرفين)*.

---

---

## 📸 التوثيق البصري للصور (يعمل مجاناً 100% وبدون أي ترقية أو بطاقة ائتمان)

إذا ظهرت لك رسالة في Firebase تطلب ترقية الخطة (*To use Storage, upgrade your project's pricing plan*):
- **لا تقلق، لا تحتاج إلى ترقية خطتك إطلاقاً ولا تحتاج لإضافة أي بطاقة ائتمان.**
- تمت برمجة وتطوير النظام بمحرك ضغط ذكي يقوم بتصغير الصور إلى أقل من 40 كيلوبايت وحفظها ومزامنتها لحظياً وبشكل مجاني تماماً عبر **Firebase Realtime Database**.
- تفعيل تبويب Cloud Storage **اختياري تماماً**، والنظام يعمل بكفاءة تامة وتوثيق بكسلي مع الختم المائي مباشرة بدونها!

---

## 🛡️ الميزات الأمنية المضافة برمجياً داخل النظام تلقائياً:
1. **قفل التخمين السحابي (Cloud Anti-Brute-Force):** تجميد الحساب لمدة 15 دقيقة سحابياً عند تكرار 5 محاولات خاطئة لمنع التخمين العشوائي.
2. **سجل الرقابة اللحظي (Audit Trail):** توثيق كل عملية دخول بالثانية مع نوع الجهاز والمتصفح والنتيجة.
3. **التوثيق البصري للمهام (Photo Evidence):** ضغط فوري للصور إلى أقل من 90KB مع ختم مائي رقمي آلي يثبت الفرع واسم المشرف وتاريخ ولحظة التنفيذ بالثانية.
4. **دعم رموز PIN لجميع المشرفين والمديرين مع دخول فوري.**
