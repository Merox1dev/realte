# Ticket-Bot-V1


> 🎟️ بوت تذاكر احترافي. قائمة اختيار + نموذج، تحكّم كامل (Claim/Close/Reopen/Delete)، ماكروز، حالات (Waiting User/Staff)، ترانسكربت HTML، حدود ذكية، تقارير أسبوعية.

---

## ⚡️ لماذا هذا البوت؟
- سريع، واضح، يعتمد **SQLite** بلا تعقيد.
- واجهة حديثة: **Select + Modal + Buttons + Slash**.
- متوافق مع **Node 18.17+ / 20+** و **discord.js v14**.

> **تنبيه:** استخدام البوت للدعم والتنظيم داخل سيرفرك فقط.

---

## ✨ المميزات
- **أنواع التذاكر**: 🛠️ Support • 💳 Billing • 🖥️ Technical • 📢 Partnership • 🚨 Report  
- **تحكّم فوري**: Claim • Close (مع سبب) • Reopen (بحد) • Delete  
- **حالات**: Waiting on **User** / Waiting on **Staff**  
- **ماكروز**: ترحيب — طلب تفاصيل — تبديل حالة  
- **حدود وكوابح**: حدّ لكل نوع، حدّ عام، حدّ يومي، كولداون بين الفتحات  
- **إغلاق تلقائي**: Auto-Close للتذاكر الخاملة بعد N ساعات  
- **تقارير أسبوعية**: فتح/إغلاق/متوسط زمن المعالجة  
- **سجلات وترانسكربت**: HTML Transcript مرفق في قناة اللوق عند الإغلاق  
- **ترجمة (i18n)**: ملفات JSON (ar/en)

---

## 🧰 المتطلبات
- **Node.js**: 18.17+ أو 20+  
- صلاحيات البوت: Manage Channels, Read/Send, Read History, Attach Files, Use App Commands

---

> أول تشغيل: لو قناة اللوحة `panelChannelId` فاضية، ينشر البوت لوحة فتح التذاكر تلقائيًا.

---

## 🧑‍💻 الأوامر

### Slash

* `/add user:<عضو>` — إضافة عضو للتذكرة الحالية
* `/remove user:<عضو>` — إزالة عضو من التذكرة
* `/move type:<support|billing|technical|partnership|report>` — نقل نوع التذكرة (يعدل الاسم والصلاحيات)
* `/note text:<نص>` — ملاحظة للطاقم (Embed)
* `/stats` — إجمالي/مفتوح/مغلق/مستلم

### الأزرار والقوائم

* **Claim** — استلام التذكرة
* **Close** — إغلاق مع سبب (Modal)
* **Reopen** — إعادة فتح ضمن الحد
* **Delete** — حذف نهائي (بعد الإغلاق)
* **Waiting User / Waiting Staff** — تبديل الحالة
* **Macros** — (greet / more-info / switch-state)

> ردود الإدارة “خفية” باستخدام `MessageFlags.Ephemeral` (لا تزعج القناة).

---

## ⚙️ التكوين (config.json) — شرح مفصّل

> ضع **IDs صحيحة** للقنوات/التصنيفات/الأدوار. أي حقل ناقص = سلوك غير متوقع.

### 1) هوية وتشغيل

* `token`: توكن البوت من Developer Portal.
* `clientId`: Application (Client) ID.
* `guildId`: السيرفر الهدف لتسجيل أوامر السلاش.

### 2) اللغة

* `lang`: `"ar"` أو `"en"`. أضف ملفًا لـ `locales/en.json` لو حبيت.

### 3) القنوات والتصنيفات

* `panelChannelId`: القناة التي تُعرض فيها لوحة فتح التذاكر.
  **اقتراحي:** خصّص قناة ثابتة “🎟️・open-ticket”.
* `logChannelId`: قناة اللوق (فتح/إغلاق/حذف + ترانسكربت).
* `ticketsCategoryId`: تصنيف التذاكر المفتوحة.
* `archiveCategoryId`: تصنيف الأرشيف (اختياري؛ تُنقل له القنوات بعد الإغلاق).
* `weeklyReportChannelId`: قناة التقرير الأسبوعي.

### 4) الأدوار والصلاحيات

* `supportTeamRoleId`: رول افتراضي لو نوع التذكرة ما له رول خاص.
* `typeRoleIds`: ماب يربط كل نوع رول معيّن، مثال:

  ```json
  "typeRoleIds": {
    "support": "ROLE_SUPPORT",
    "billing": "ROLE_BILLING",
    "technical": "ROLE_TECH",
    "partnership": "ROLE_PARTNER",
    "report": "ROLE_REPORT"
  }
  ```
* أدوار الإدارة:

  * `claimRoleId`: من يقدر يستلم/يدير التذاكر.
  * `closeRoleId`: من يقدر يغلق.
  * `reopenRoleId`: من يقدر يعيد فتح.
  * `deleteRoleId`: من يقدر يحذف نهائي.
  * `bypassLimitRoleId`: يتجاوز حدود الفتح/الكولداون.

> **اقتراحي:** خلي رول “Management” يمتلك كل الأذونات أعلاه، و“Moderation” يمتلك Claim/Close فقط.

### 5) الحدود والسياسات

* `perTypeLimit`: أقصى تذاكر **مفتوحة** للمستخدم الواحد **لكل نوع** (افترح: 1).
* `globalOpenLimit`: أقصى تذاكر **مفتوحة** للمستخدم الواحد على كل الأنواع (افترح: 2).
* `dailyOpenLimit`: أقصى تذاكر **جديدة** باليوم للمستخدم (افترح: 3).
* `openCooldownSec`: كولداون بين عمليات الفتح (بالثواني) (افترح: 120).
* `reopenLimit`: مرات إعادة الفتح لنفس التذكرة (افترح: 1).
* `idleAutoCloseHours`: إغلاق تلقائي للتذاكر الخاملة بعد ساعات (افترح: 48).
* `dmOnClose`: إرسال رسالة خاص للمستخدم عند الإغلاق (true/false).

### 6) التقارير

* `weeklyReport`: كائن باليوم/الساعة (UTC):

  ```json
  "weeklyReport": { "day": 0, "hour": 20 }  // الأحد 20:00 UTC
  ```

### 7) مثال كامل

```json
{
  "token": "BOT_TOKEN",
  "clientId": "APP_ID",
  "guildId": "GUILD_ID",

  "lang": "ar",

  "panelChannelId": "CHANNEL_ID",
  "logChannelId": "CHANNEL_ID",
  "ticketsCategoryId": "CATEGORY_ID",
  "archiveCategoryId": "CATEGORY_ID",
  "weeklyReportChannelId": "CHANNEL_ID",

  "supportTeamRoleId": "ROLE_FALLBACK_ID",
  "typeRoleIds": {
    "support": "ROLE_ID",
    "billing": "ROLE_ID",
    "technical": "ROLE_ID",
    "partnership": "ROLE_ID",
    "report": "ROLE_ID"
  },

  "claimRoleId": "ROLE_ID",
  "closeRoleId": "ROLE_ID",
  "reopenRoleId": "ROLE_ID",
  "deleteRoleId": "ROLE_ID",
  "bypassLimitRoleId": "ROLE_ID",

  "perTypeLimit": 1,
  "globalOpenLimit": 2,
  "dailyOpenLimit": 3,
  "openCooldownSec": 120,
  "reopenLimit": 1,
  "idleAutoCloseHours": 48,
  "dmOnClose": true,

  "weeklyReport": { "day": 0, "hour": 20 }
}
```

**اقتراحاتي العملية:**

* استخدم تصنيفين واضحين: `🎫 Tickets` و `🗄️ Tickets Archive`.
* اجعل اللوحة مقفلة للكتابة (View فقط) حتى تكون نظيفة.
* ضع البوت **أعلى** من رتب الطاقم التي يحتاج يمنحها.
* في اللوق: فعّل **Slowmode** بسيط (5s) حتى لا تتكدس رسائل الماكروز.

---

## 🗃️ قاعدة البيانات

* **SQLite** بملف `tickets.db` (WAL مفعّل للأداء).
* الجداول:

  * `tickets(id, channelId, userId, type, status, state, createdAt, lastActivityAt, claimedBy, closedAt, reopenedCount)`
  * `actions(id, ticketId, at, action, byUser, data)`
* **الترانسكربت**: يتم توليد `transcript-<ID>.html` عند الإغلاق ويُرسل لقناة اللوق كمرفق.

**نصائح:**

* نسخ احتياطي دوري لملف `tickets.db`.
* عند النقل لخادم جديد: أوقف البوت → انسخ `tickets.db` والريبو → شغّل.

---

## 🤝 دعم وتواصل

* تحقّق من:

  * نسخة Node **18.17+ / 20+**
  * صلاحيات البوت في القنوات
  * IDs صحيحة في `config.json`
  * أن تصنيف التذاكر موجود وله صلاحيات إنشاء قنوات

> لو احتجت مراجعة إعداداتك، أرسل لقطة من `config.json` (مع إخفاء التوكن).

---
