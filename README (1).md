# 🔷 مساعد أبشر - نظام RAG للخدمات الحكومية السعودية

> نظام ذكاء اصطناعي متكامل لأتمتة الخدمات الحكومية عبر منصة أبشر

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![Python](https://img.shields.io/badge/python-3.10+-green)
![Ollama](https://img.shields.io/badge/ollama-qwen2.5:7b-orange)
![License](https://img.shields.io/badge/license-MIT-purple)

---

## 📋 نظرة عامة

نظام RAG (Retrieval-Augmented Generation) متقدم يوفر:

- 🪪 **تجديد المستندات** - الهوية، الجواز، الرخصة، الاستمارة
- 📝 **مستشار الوكالات** - تحليل المخاطر ونماذج آمنة
- 🔍 **OCR للمستندات** - استخراج البيانات من الصور
- 🗣️ **التعرف على اللهجات السعودية** - نجدي، حجازي، شرقي، جنوبي
- 💬 **محادثة ذكية بالعربية** - واجهة دردشة طبيعية

---

## 🏗️ هيكل المشروع

```
rag-absher/
├── app.py                 # واجهة المستخدم (Streamlit)
├── rag_system.py          # النظام الرئيسي المتكامل
├── ollama_client.py       # عميل Ollama للـ LLM
├── db_manager.py          # إدارة قاعدة البيانات
├── knowledge_base.py      # قاعدة المعرفة (9 خدمات)
├── wakala_advisor.py      # مستشار الوكالات (10 أنماط خطر)
├── retrieval_engine.py    # محرك الاسترجاع (ChromaDB)
├── ocr_engine.py          # محرك OCR (6 أنواع مستندات)
├── speech_engine.py       # محرك الصوت (4 لهجات)
├── schemas.py             # مخططات JSON للتكامل
├── tests.py               # اختبارات شاملة
├── setup.sh               # سكربت التثبيت
└── citizens.db            # قاعدة بيانات المواطنين
```

---

## 🚀 التثبيت والتشغيل

### المتطلبات

- Python 3.10+
- Ollama
- RTX GPU (16GB VRAM موصى به)

### التثبيت

```bash
# 1. استنساخ المشروع
git clone <repo-url>
cd rag-absher

# 2. تثبيت المتطلبات
pip install streamlit chromadb sentence-transformers

# 3. تثبيت نموذج Ollama
ollama pull qwen2.5:7b

# 4. تشغيل التطبيق
streamlit run app.py
```

### أو استخدم سكربت التثبيت:

```bash
chmod +x setup.sh
./setup.sh
```

---

## 🧪 بيانات الاختبار

### أرقام الهوية للاختبار

| رقم الهوية | الاسم | الحالة |
|------------|-------|--------|
| `1087654321` | محمد الغامدي | هوية + استمارة منتهية ❌ |
| `2098765432` | سارة الغامدي | جواز منتهي ❌ |
| `1122334455` | خالد العتيبي | جواز + رخصة قاربت ⚠️ |
| `5544332211` | نورة العتيبي | كل شيء ساري ✅ |
| `6677889900` | فهد القحطاني | جواز + رخصة منتهية ❌ |
| `7788990011` | عبدالرحمن الشهري | كل شيء منتهي ❌ |

---

## 💡 حالات الاستخدام

### 1️⃣ تجديد الهوية الوطنية

```
👤: 1087654321
🔷: أهلاً محمد! [عرض الملف الشخصي]
   ⚠️ الهوية منتهية

👤: أبغى أجدد هويتي
🔷: 📸 مطلوب صورة الهوية

👤: [رفع صورة] رفعتها
🔷: ✅ تم التجديد بنجاح
   رقم الطلب: REQ-xxx
```

### 2️⃣ وكالة بيع سيارة

```
👤: أبغى توكيل لأخوي يبيع سيارتي
🔷: ✅ أنصحك بوكالة خاصة لـ بيع سيارة
   📝 نموذج الوكالة:
   "أوكل السيد/ة [الاسم] ببيع السيارة..."
   ⚠️ نصائح: حدد المدة، لا تتجاوز سنة
```

### 3️⃣ تحذير من الوكالة العامة

```
👤: أبغى وكالة عامة
🔷: ⛔ تحذير! الوكالة العامة خطرة جداً
   تمنح صلاحيات واسعة قد تُستغل
   أنصحك بوكالة خاصة بدلاً منها
```

---

## 🔧 المكونات التقنية

### 1. Ollama Client (`ollama_client.py`)

```python
# تهيئة العميل
client = OllamaClient(model="qwen2.5:7b")

# محادثة
response = client.chat([
    {"role": "user", "content": "السلام عليكم"}
])

# تحليل صورة (OCR)
text = client.vision(image_base64, "استخرج النص")
```

### 2. Wakala Advisor (`wakala_advisor.py`)

```python
advisor = WakalaAdvisor()

# تحليل نص وكالة
analysis = advisor.analyze("وكالة عامة للتصرف في كافة الأملاك")
# Returns: risk_level=CRITICAL, warnings=[...]

# اقتراح وكالة آمنة
template = advisor.suggest_safe_wakala("بيع سيارة")
```

### 3. Knowledge Base (`knowledge_base.py`)

```python
kb = AbsherKnowledgeBase()

# البحث عن خدمة
results = kb.search("تجديد الهوية", top_k=3)

# الحصول على متطلبات
reqs = kb.get_service_requirements("national_id_renewal")
```

### 4. Database Manager (`db_manager.py`)

```python
db = get_db()

# الحصول على مواطن
citizen = db.get_citizen("1087654321")

# تجديد مستند
result = db.renew_document("1087654321", "national_id")
# Returns: {success: True, request_id: "REQ-xxx", new_expiry: "2035-xx-xx"}
```

---

## 🛡️ الأمان والمخاطر

### أنماط الخطر في الوكالات

| النمط | مستوى الخطر | التحذير |
|-------|-------------|---------|
| التنازل عن كافة الحقوق | 🔴 حرج | تسمح بالتنازل عن أي حق |
| بيع كافة الممتلكات | 🔴 حرج | تشمل جميع أملاكك |
| التصرف المطلق | 🔴 حرج | بدون قيود |
| دون الرجوع للموكل | 🟠 عالي | قرارات بدون موافقتك |
| لمدة غير محددة | 🟠 عالي | سارية للأبد |
| كافة الصلاحيات | 🟠 عالي | صلاحيات واسعة |

---

## 🔌 التكامل مع Module 3

### مخططات JSON (`schemas.py`)

```python
# مستند مستخرج
ExtractedDocument = {
    "doc_type": "national_id",
    "extracted_data": {...},
    "confidence": 0.95,
    "ocr_text": "..."
}

# إدخال المستخدم
UserInput = {
    "text": "أبغى أجدد هويتي",
    "intent": "renewal",
    "entities": {"doc_type": "national_id"}
}

# مخرج للتكامل
IntegrationOutput = {
    "action": "renew_document",
    "parameters": {...},
    "citizen_id": "1087654321"
}
```

---

## 📈 الاختبارات

```bash
# تشغيل الاختبارات
python tests.py
```

### نتائج الاختبارات

```
✅ test_ollama_client: PASSED
✅ test_knowledge_base: PASSED
✅ test_wakala_advisor: PASSED
✅ test_retrieval_engine: PASSED
✅ test_ocr_engine: PASSED
✅ test_speech_engine: PASSED
✅ test_db_manager: PASSED
✅ test_rag_system: PASSED

Total: 8/8 (100%)
```

---

## 🎨 واجهة المستخدم

### الألوان

| العنصر | اللون |
|--------|-------|
| الخلفية | `#1a1a2e` |
| البطاقات | `#16162a` |
| رسائل المستخدم | `#667eea` |
| رسائل البوت | `#2d2d44` |
| النجاح | `#4ade80` |
| التحذير | `#fbbf24` |
| الخطأ | `#f87171` |

---

## 👥 الفريق

| الدور | المسؤولية |
|-------|-----------|
| **Module 1-2** | RAG Infrastructure & Retrieval |
| **Module 3** | AI Agent & Form Filling |

---

## 📝 الترخيص

MIT License - للاستخدام التعليمي والتطويري

---

## 🔗 روابط مفيدة

- [Ollama Documentation](https://ollama.ai/docs)
- [ChromaDB](https://www.trychroma.com/)
- [Streamlit](https://streamlit.io/)
- [Sentence Transformers](https://www.sbert.net/)

---

<div align="center">

**صُنع بـ ❤️ لخدمة المواطن السعودي**

</div>