<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/Fastsend-sa/.github/main/profile/logo-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Fastsend-sa/.github/main/profile/logo.svg">
    <img src="https://raw.githubusercontent.com/Fastsend-sa/.github/main/profile/logo.svg" alt="Fastsend" width="96">
  </picture>
</p>

# Fastsend

**Email sending API for AI agents, running locally in Saudi Arabia.**

Fastsend is agentic email infrastructure for AI. An app, or the AI agent that builds it, hands over its sign-in codes, confirmations and receipts, and Fastsend sends them from Riyadh, from the app's own domain. It is for people who build apps with AI agents, and for the agents themselves.

[Website](https://fastsend.sa) · [hello@fastsend.sa](mailto:hello@fastsend.sa) · [العربية](#العربية)

## What Fastsend does

| Part | What it does |
|---|---|
| Email API | One `POST /emails` call per email, in JSON over HTTPS: sender, recipients, subject, HTML or plain text. Up to 100 emails in one `POST /emails/batch` call. |
| Scheduled sending | `scheduled_at` sends an email later, up to 30 days ahead. Until it goes, its time can be moved or it can be cancelled. |
| Idempotency keys | With an `Idempotency-Key` header, a repeated call within 24 hours sends nothing new. |
| Your own domain | Fastsend lists the DNS records a domain needs and checks them. |
| Signed webhooks | Each email's events reach your address, signed in the Standard Webhooks format. |
| API keys | Full access, or sending only for one domain or all of them. A key is shown once; Fastsend keeps only its SHA-256 hash and its last four characters. |
| Suppression list | An address that bounced or complained is not emailed again. |
| Sign-in emails | An Auth server hands its sign-in emails to Fastsend through its Send Email hook, written in English or Arabic. |
| Dashboard | Emails, domains, keys, webhooks and the members of your organization in a web browser, in English and Arabic. |
| AI connector (MCP) | An AI agent sends email, reads each email's result and manages domains in plain words. |

Fastsend answers the same sending calls as a widely used email API, so existing email code keeps working: only the base address and the key change. It is tested with that API's own Node.js SDK.

## Send an email

An API key is made in the dashboard, under **API keys**.

```bash
curl -X POST "https://api.fastsend.sa/functions/v1/api/emails" \
  -H "Authorization: Bearer $FASTSEND_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: order-1042-confirmation" \
  -d '{
    "from": "Orders <orders@notify.example.sa>",
    "to": ["owner@example.sa"],
    "subject": "Your order is confirmed",
    "text": "Thank you. Your order 1042 is confirmed."
  }'
```

The answer is `{"id": "…"}`, and `GET /emails/{id}` reads the email back. Every error has one shape: `{ "statusCode", "name", "message" }`.

## AI agents and the connector

- The AI connector is a remote MCP server that Fastsend runs, on the Streamable HTTP transport. Nothing is installed on your machine.
- Its address ends in `/functions/v1/mcp`, and the **Integrations** page of the dashboard shows it in full.
- AI apps that add remote connectors sign in with OAuth: the app asks for sending only or full access, and an owner or administrator of your organization approves it in the dashboard.
- Agents that set a request header use an API key instead, as `Authorization: Bearer`.
- Its tools send one email or a batch, list and read emails, move or cancel a scheduled email, read and verify domains, and read webhooks and the suppression list. With an API key, they also make and delete API keys and add or change webhooks.
- What an AI app reads through the connector reaches that app's maker, which can be outside the Kingdom.

## Security and data

- Sending, logs and lists stay in Riyadh.
- Mail to an inbox abroad, such as Gmail or Outlook, leaves the Kingdom when it is delivered, as all email does.
- An email and its events are deleted 30 days after it is sent.
- Webhook signing secrets are encrypted at rest with AES-256-GCM.

## Contact

[hello@fastsend.sa](mailto:hello@fastsend.sa) · [fastsend.sa](https://fastsend.sa)

---

A Fast Solutions product · Riyadh, Saudi Arabia

---

<div dir="rtl">

## العربية

**واجهة برمجية لإرسال البريد الإلكتروني لوكلاء الذكاء الاصطناعي، تعمل محلياً في السعودية.**

بنية تحتية للبريد الإلكتروني لوكلاء الذكاء الاصطناعي: يسلم التطبيق، أو وكيل الذكاء الاصطناعي الذي يبنيه، رموز الدخول والتأكيدات والإيصالات إلى Fastsend، فيرسلها من الرياض ومن نطاق التطبيق نفسه. وهو موجه لمن يبنون التطبيقات مع وكلاء الذكاء الاصطناعي، وللوكلاء أنفسهم.

[الموقع](https://fastsend.sa/ar/) · [hello@fastsend.sa](mailto:hello@fastsend.sa) · [English](#fastsend)

### ما يقدمه Fastsend

| المكون | وظيفته |
|---|---|
| واجهة إرسال البريد | طلب `POST /emails` واحد لكل رسالة، بصيغة JSON عبر HTTPS: المرسل والمستلمون والموضوع، بنص HTML أو نص عادي. وحتى 100 رسالة في طلب `POST /emails/batch` واحد. |
| الإرسال المجدول | يحدد الحقل `scheduled_at` وقتا لاحقا للإرسال، حتى 30 يوما. وقبل أن تخرج الرسالة يمكن تغيير وقتها أو إلغاؤها. |
| مفاتيح منع التكرار | مع ترويسة `Idempotency-Key` لا يرسل الطلب المكرر خلال 24 ساعة شيئا جديدا. |
| نطاقك أنت | يعرض Fastsend سجلات DNS التي يحتاجها النطاق ويتحقق منها. |
| أحداث موقعة إلى webhook | تصل أحداث كل رسالة إلى عنوانك، موقعة بصيغة Standard Webhooks. |
| مفاتيح API | صلاحية كاملة، أو إرسال فقط لنطاق واحد أو لكل النطاقات. يظهر المفتاح مرة واحدة، ولا يحفظ Fastsend إلا بصمته (SHA-256) وآخر أربعة أحرف منه. |
| قائمة الاستبعاد | العنوان الذي ارتدت إليه رسالة أو اشتكى منها لا نراسله مجددا. |
| رسائل الدخول | يسلم خادم المصادقة رسائل الدخول إلى Fastsend عبر خطاف إرسال البريد (Send Email hook)، وتكتب بالعربية أو بالإنجليزية. |
| لوحة التحكم | الرسائل والنطاقات والمفاتيح و webhooks وأعضاء المؤسسة في متصفح الويب، بالعربية والإنجليزية. |
| موصل الذكاء الاصطناعي (MCP) | يرسل وكيل الذكاء الاصطناعي البريد ويقرأ نتيجة كل رسالة ويدير النطاقات بكلام عادي. |

يجيب Fastsend عن طلبات الإرسال نفسها لواجهة بريد شائعة، فتبقى شيفرة البريد الحالية تعمل: لا يتغير إلا العنوان الأساسي والمفتاح. وقد اختبر ذلك بحزمة Node.js الرسمية لتلك الواجهة.

### إرسال رسالة

ينشأ مفتاح API من لوحة التحكم، من صفحة **مفاتيح API**.

<div dir="ltr">

```bash
curl -X POST "https://api.fastsend.sa/functions/v1/api/emails" \
  -H "Authorization: Bearer $FASTSEND_KEY" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: order-1042-confirmation" \
  -d '{
    "from": "Orders <orders@notify.example.sa>",
    "to": ["owner@example.sa"],
    "subject": "Your order is confirmed",
    "text": "Thank you. Your order 1042 is confirmed."
  }'
```

</div>

يكون الرد ‎`{"id": "…"}`‎، ويقرأ الطلب ‎`GET /emails/{id}`‎ الرسالة لاحقا. ولكل خطأ شكل واحد: ‎`{ "statusCode", "name", "message" }`‎.

### وكلاء الذكاء الاصطناعي والموصل

- موصل الذكاء الاصطناعي خادم MCP عن بعد يشغله Fastsend، بطريقة النقل Streamable HTTP. ولا شيء تثبته على جهازك.
- ينتهي عنوانه بالمسار ‎`/functions/v1/mcp`‎، وتعرضه صفحة **التكاملات** في لوحة التحكم كاملا.
- تطبيقات الذكاء الاصطناعي التي تضيف موصلات عن بعد تسجل الدخول عبر OAuth: يطلب التطبيق صلاحية الإرسال فقط أو الصلاحية الكاملة، ويوافق عليه مالك مؤسستك أو أحد مشرفيها من لوحة التحكم.
- والوكلاء الذين يضبطون ترويسة الطلب يستخدمون مفتاح API بدلا من ذلك، في الترويسة ‎`Authorization: Bearer`‎.
- أدواته ترسل رسالة واحدة أو دفعة، وتعرض الرسائل وتقرؤها، وتغير وقت رسالة مجدولة أو تلغيها، وتقرأ النطاقات وتتحقق منها، وتقرأ webhooks وقائمة الاستبعاد. ومع مفتاح API تنشئ أيضا مفاتيح API وتحذفها، وتضيف webhooks أو تغيرها.
- ما يقرؤه تطبيق ذكاء اصطناعي عبر الموصل يصل إلى الشركة المطورة لذلك التطبيق، وقد تكون خارج المملكة.

### الأمان والبيانات

- الإرسال والسجلات والقوائم تبقى في الرياض.
- أما الرسائل إلى صندوق بريد في الخارج، مثل Gmail أو Outlook، فتغادر المملكة عند تسليمها، كما هو حال كل بريد إلكتروني.
- تحذف الرسالة وأحداثها بعد 30 يوما من إرسالها.
- أسرار توقيع webhooks مشفرة عند حفظها بخوارزمية AES-256-GCM.

### التواصل

[hello@fastsend.sa](mailto:hello@fastsend.sa) · [fastsend.sa](https://fastsend.sa/ar/)

---

أحد منتجات Fast Solutions · الرياض، المملكة العربية السعودية

</div>
