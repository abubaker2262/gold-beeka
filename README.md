# 💎 Golden ERP SaaS — نظام ERP متكامل للذهب والمجوهرات

نظام ERP + POS متكامل لمحلات الذهب والمجوهرات، مبني بـ Next.js 14 و Supabase.

## ✅ الوحدات المكتملة (100%)

| الوحدة | الحالة |
|--------|--------|
| Auth + RLS (102 سياسة) | ✅ |
| إدارة الفروع (Multi-branch SaaS) | ✅ |
| المستخدمون والأدوار (7 أدوار) | ✅ |
| إدارة المنتجات والمخزون | ✅ |
| نظام POS كامل مع دفع مختلط | ✅ |
| فواتير المبيعات + طباعة PDF | ✅ |
| إدارة الموردين | ✅ |
| **حساب ذهب المورد (Gold Current Account)** | ✅ ⭐ |
| معاملات تبادل الذهب (هالك + أجرة) | ✅ |
| إدارة العملاء + برنامج الولاء | ✅ |
| فواتير المشتريات | ✅ |
| المحاسبة (قيود + ميزان + أرباح) | ✅ |
| الخزينة (سندات قبض وصرف) | ✅ |
| التقارير (مبيعات + ذهب + موردون) | ✅ |
| إدارة أسعار الذهب | ✅ |
| المساعد الذكي (Claude AI) | ✅ |
| الإشعارات التلقائية | ✅ |
| سجل المراجعة الكامل | ✅ |
| إعدادات النظام | ✅ |
| تصدير CSV/Excel | ✅ |

---

## 🚀 البدء السريع

### 1. تثبيت وتشغيل

```bash
git clone https://github.com/YOUR_USERNAME/gold-erp-saas.git
cd gold-erp-saas
npm install
cp .env.example .env.local
npm run dev
```

### 2. إعداد Supabase

```bash
# CLI
supabase login
supabase link --project-ref YOUR_PROJECT_ID
supabase db push

# أو شغّل الملفات يدوياً بالترتيب في SQL Editor:
# 001 → 002 → 003 → 004 → 005 → 006
```

### 3. متغيرات البيئة (.env.local)

```env
NEXT_PUBLIC_SUPABASE_URL=https://YOUR_PROJECT.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
SUPABASE_SERVICE_ROLE_KEY=your-service-role-key
NEXT_PUBLIC_APP_URL=http://localhost:3000
ANTHROPIC_API_KEY=sk-ant-...   # اختياري للمساعد الذكي
```

### 4. إنشاء أول محل

```sql
-- في Supabase SQL Editor:
INSERT INTO tenants (store_name_ar, store_name_en, slug, currency, subscription_plan, max_branches)
VALUES ('محل النور للمجوهرات', 'Al-Nour Jewelry', 'al-nour', 'SDG', 'gold', 3)
RETURNING id;

-- تهيئة المحل بعد الحصول على الـ ID:
SELECT onboard_tenant('TENANT_ID'::UUID, 'محل النور للمجوهرات', 'Al-Nour Jewelry', 'SDG', 6240);
```

أنشئ مستخدماً في Supabase Auth Dashboard ثم:

```sql
INSERT INTO user_profiles (id, tenant_id, full_name, full_name_ar, role, is_active)
VALUES ('AUTH_USER_UUID', 'TENANT_ID', 'Ahmed Al-Rashidi', 'أحمد الرشيدي', 'owner', true);
```

---

## 🏗️ النشر على Vercel

```bash
npm i -g vercel && vercel --prod
```

---

## ⭐ نظام حساب ذهب المورد

```
محل يرسل 1000g 21K → ورشة التصنيع
    ↓
الورشة ترجع 950g مجوهرات + أجرة صنعة
    ↓
هالك = مرسَل (نقي) - مستلَم (نقي) - أجرة بالذهب
    ↓
رصيد الورشة = ما تبقى عند المورد
```

`supplier_gold_ledger` — دفتر يومية بالجرام، append-only، لا يُحذف.

---

## 🔐 الأمان

- Row Level Security على جميع الجداول
- Multi-tenant isolation كامل
- Audit log لا يُحذف
- 7 أدوار مع صلاحيات مفصّلة
