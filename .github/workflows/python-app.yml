main.py - Bot Links Manager (single-file, Replit-ready, auto-alive)

import os
import time
import json
import requests
from threading import Thread
from flask import Flask
from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
from telegram.ext import Application, CommandHandler, CallbackQueryHandler, ContextTypes

----------------- CONFIG -----------------

TOKEN = os.environ.get("TOKEN", "")

You can set REPL_URL in Replit Secrets to e.g. "https://yourname.replit.dev"

or REPL_SLUG and REPL_OWNER will be used if present.

----------------- DATA (embedded) -----------------

DATA = {
"إحصاء واحتمالات": {
"مراجع": [
"Applied-statistics and probability: https://t.me/c/2805935068/4/81"
],
"ملخصات": [
"المحاضرة الاولى: https://t.me/c/2805935068/4/126",
"المحاضرة الثانية: https://t.me/c/2805935068/4/124",
"المحاضرة الثالتة: https://t.me/c/2805935068/4/127",
"المحاضرة الرابعة: https://t.me/c/2805935068/4/131",
"المحاضرة الخامسة: https://t.me/c/2805935068/4/132",
"المحاضرة السادسة: https://t.me/c/2805935068/4/130",
"المحاضرة السابعة: https://t.me/c/2805935068/4/129",
"المحاضرة الثامنة: https://t.me/c/2805935068/4/128",
"المحاضرة التاسعة: https://t.me/c/2805935068/4/125",
"محاضرات من 1 الى 5: https://t.me/c/2805935068/4/134",
"دفتر محاضرات من 1 الى 4: https://t.me/c/2805935068/4/133",
"تلخيص طالبة للمقرر كامل للدكتور عبدالله مدهش :https://t.me/c/2805935068/4/1834",
"قوانين الاحتمالات : https://t.me/c/2805935068/4/1973?single","قوانين الاحصاء :https://t.me/c/2805935068/4/1980?single","نظريات وتمارين محلول في الاحصاء واحتمالات+ مقدمة في علم الاحصاء واحتمالات:https://t.me/c/2805935068/4/2083?single","التوزيعات الاحتماليه:https://t.me/c/2805935068/4/2712 & https://t.me/c/2805935068/4/2714"
],
"صور محاضرات": [
"المحاضرة الأولى: https://t.me/c/2805935068/56/244",
"المحاضرة الثانية: https://t.me/c/2805935068/56/759",
"تمارين احصاء المحاضرة الأولى: https://t.me/c/2805935068/56/808",
"المحاضرة الثالثة: https://t.me/c/2805935068/56/885",
"المحاضرة الرابعة: https://t.me/c/2805935068/56/1783",
"المحاضرات5&6&7:https://t.me/c/2805935068/56/1837?single",
"صور محاضرات من 7_9:https://t.me/c/2805935068/56/1883",
"تمارين احصاء واحتمالات المحاضرة الخمسة⬆️ : https://t.me/c/2805935068/56/1901",
"صور تمارين الاحتمالات:https://t.me/c/2805935068/56/1957","صور تمارين احصاء واحتمالات:https://t.me/c/2805935068/56/2054?single","ما تم تصويره من قبل استاذة الاحصاء واحتمالات:https://t.me/c/2805935068/56/2067","محاضرات الاستاذه 👆:https://t.me/c/2805935068/56/2159","توزيع الاحتمالات:https://t.me/c/2805935068/56/2170","التوزيع المنتظم:https://t.me/c/2805935068/56/2312","توزيع الاحتمالات:https://t.me/c/2805935068/56/2854?single"
],
"نماذج امتحانات": ["نماذج احصاء واحتمالات: https://t.me/c/2805935068/2826/2827?single","نموذج السنة السابقة للدكتور شوقي العليمي : https://t.me/c/2805935068/2826/2837.  &. https://t.me/c/2805935068/2826/2838"]
},

"تصميم تجريبي": {"مراجع": [], "ملخصات": [], "صور محاضرات": [], "نماذج امتحانات": []},

"عمليات صناعية 2": {
"مراجع": [
"Publication: https://t.me/c/2805935068/6/1768",
"شرح آلات التشغيل: https://t.me/c/2805935068/6/1776",
"من الدكتور محمود سعيد: https://t.me/c/2805935068/6/1818"
],
"ملخصات": ["أسأله محلوله من المهندس محمود سعيد: https://t.me/c/2805935068/6/2305"],
"صور محاضرات": [
"المحاضرة الأولى 1&2&3: https://t.me/c/2805935068/56/249",
"صور نموذج اتخاذ القرار 2: https://t.me/c/2805935068/56/883"
],
"نماذج امتحانات": [
"نماذج الدكتور محمود سعيد:https://t.me/c/2805935068/6/1869?single",
"نماذج عمليات صناعية2 :https://t.me/c/2805935068/6/1869?single",
"نماذج عمليات صناعية2:https://t.me/c/2805935068/6/2001","نماذج عمليات صناعية 2: https://t.me/c/2805935068/6/2478?single"
]
},

"بحوث عمليات 2": {
"مراجع": [
"الذي اخذ منه الدكتور محمود سعيد فيبه تمارين : https://t.me/c/2805935068/7/1929",
"بحوث عمليات2:https://t.me/c/2805935068/7/1933"
],
"ملخصات": [
"الواجب الأول 4 مسائل: https://t.me/c/2805935068/7/402?single",
"الواجب الثاني: https://t.me/c/2805935068/7/1815?single"
],
"صور محاضرات": [
"المحاضرة الرابعة: https://t.me/c/2805935068/56/1810",
"تمارين فيها اخطاء (تسلسل صحيح): https://t.me/c/2805935068/56/1826",
"تمارين بعد التصحيح: https://t.me/c/2805935068/56/1827",
"صور تمارين:https://t.me/c/2805935068/56/1914?single","مخطط حق المثال صفحة رقم 89:https://t.me/c/2805935068/56/2056","المحاضرة 11: https://t.me/c/2805935068/56/2594"
],
"نماذج امتحانات": [
"اختبار المهندس محمود سعيد: https://t.me/c/2805935068/7/83","نماذج بحوث عمليات 2: https://t.me/c/2805935068/2826/2835?single","نماذج مستوى ثالث:https://t.me/c/2805935068/7/2254"

]

},

"تخطيط الانتاج والتحكم بالمخازن": {
"مراجع": [
"مرجع الدكتور عبدالغني النقيب: https://t.me/c/2805935068/8/84",
"مراجع متنوعة: https://t.me/c/2805935068/8/840?single",
"Production: https://t.me/c/2805935068/8/1779",
"مراجع إضافية: https://t.me/c/2805935068/8/1811?single",
"ملف pdf: https://t.me/c/2805935068/8/1919",
"ملف pdf مهم جداً:https://t.me/c/2805935068/8/1920",
"ملف pdf الانتاج : https://t.me/c/2805935068/8/1937?single",
"ملفات pdf:https://t.me/c/2805935068/8/1941?single","كتاب حلو للتنبؤ :https://t.me/c/2805935068/8/2592?single"
],
"ملخصات": ["سبع خطوات لعملية التخطيط:https://t.me/c/2805935068/8/1880"],
"صور محاضرات": [
"المحاضرة الثالثة:https://t.me/c/2805935068/56/1833?single",
"صور من الملزمه حق المهندسة لمسائل disseasonal: https://t.me/c/2805935068/56/1872?single",
"مسأله المهندسة اسماء تمارين تخطيط :https://t.me/c/2805935068/56/1902","الاسراع في المشروع تخطيط الانتاج: https://t.me/c/2805935068/56/2419","مسائل النقل : https://t.me/c/2805935068/56/2595"
],
"نماذج امتحانات": [
"اختبار سابقة للدكتور عبدالغني النقيب: https://t.me/c/2805935068/8/87?single","نماذج بالنقل والتخصيص :https://t.me/c/2805935068/2826/2829","نماذج المشروع:https://t.me/c/2805935068/2826/2830","نماذج الاكراش والنقل والتخصيص:https://t.me/c/2805935068/2826/2831","نماذج التنبؤ والنقل والتخصيص:https://t.me/c/2805935068/2826/2832","نماذج المشروع والموسميه:https://t.me/c/2805935068/2826/2833"
,"نماذج النقل والتخصيص: https://t.me/c/2805935068/2826/2834","نماذج للدكتور عبدالغني النقيب والهندسة اوميله نصفي ونهائي :https://t.me/c/2805935068/8/2103?single","نموذج سيطرة وتخطيط:https://t.me/c/2805935068/8/2591"
]
},

"ادارة التسويق والمبيعات": {
"مراجع": ["كتاب الاستاذ خالد: https://t.me/c/2805935068/24/120"],
"ملخصات": ["المحاضرة الاولى:https://t.me/c/2805935068/24/123","ملخص سياسات التسويق الذي بالملزمه:https://t.me/c/2805935068/24/1961?single","مخلص تحليل البيئة والنشاط التسويقي:https://t.me/c/2805935068/24/1997?single","ملخصات لجميع المحاصرات: https://t.me/c/2805935068/24/2019?single","عرض تقديمي التسويق باستخدام الواقع الافتراضي والمواقع المعزز : https://t.me/c/2805935068/24/2246","عرض تقديمي PPT التسويق الابداعي :https://t.me/c/2805935068/24/2247"
],
"صور محاضرات": [
"صور ادارة التسويق والمبيعات: https://t.me/c/2805935068/56/358?single",
"المحاضرة 8:https://t.me/c/2805935068/56/1968"
],
"نماذج امتحانات": [
"نماذج ادارة التسويق والمبيوعات علوم ادارية: https://t.me/c/2805935068/24/1907?single"
]
}
}

----------------- prepare indices -----------------

ID_TO_NAME = {str(i + 1): name for i, name in enumerate(DATA.keys())}
CATS = {"a": "مراجع", "b": "ملخصات", "c": "صور محاضرات", "d": "نماذج امتحانات"}

----------------- UI helpers -----------------

def kb_links_view(sid, cat):
keyboard = [
[InlineKeyboardButton("🔄 تحديث", callback_data=f"upd:{sid}:{cat}")],
[InlineKeyboardButton("⬅️ رجوع", callback_data="back:cats:" + sid)],
[InlineKeyboardButton("🏠 القائمة الرئيسية", callback_data="back:menu")]
]
return InlineKeyboardMarkup(keyboard)

def kb_categories(sid):
keyboard = [
[InlineKeyboardButton("📚 مراجع", callback_data=f"view:{sid}:a")],
[InlineKeyboardButton("🧾 ملخصات", callback_data=f"view:{sid}:b")],
[InlineKeyboardButton("🖼️ صور محاضرات", callback_data=f"view:{sid}:c")],
[InlineKeyboardButton("🧪 نماذج امتحانات", callback_data=f"view:{sid}:d")],
[InlineKeyboardButton("⬅️ رجوع", callback_data="back:menu")]
]
return InlineKeyboardMarkup(keyboard)

def kb_subjects():
keyboard = []
for sid, name in ID_TO_NAME.items():
keyboard.append([InlineKeyboardButton(f"📖 {name}", callback_data=f"cats:{sid}")])
return InlineKeyboardMarkup(keyboard)

----------------- Handlers -----------------

async def start_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
await update.message.reply_text("📚 مرحباً! اختر المادة:", reply_markup=kb_subjects())

async def menu_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
await update.message.reply_text("📖 اختر المادة من القائمة:", reply_markup=kb_subjects())

async def callback_handler(update: Update, context: ContextTypes.DEFAULT_TYPE):
q = update.callback_query
await q.answer()
data = q.data

if data.startswith("cats:"):  
    _, sid = data.split(":")  
    await q.edit_message_text(f"📘 اختر القسم ({ID_TO_NAME[sid]}):", reply_markup=kb_categories(sid))  
    return  

if data.startswith("view:"):  
    _, sid, cat = data.split(":")  
    subject_name = ID_TO_NAME[sid]  
    cat_name = CATS[cat]  
    links = DATA.get(subject_name, {}).get(cat_name, [])  

    text = f"🔗 الروابط ({subject_name} → {cat_name}):\n\n"  
    if links:  
        for i, link in enumerate(links, 1):  
            text += f"{i}- {link}\n"  
    else:  
        text += "🚫 لا توجد روابط حالياً."  

    await q.edit_message_text(text=text, reply_markup=kb_links_view(sid, cat))  
    return  

if data.startswith("upd:"):  
    parts = data.split(":")  
    if len(parts) == 3:  
        _, sid, cat = parts  
        subject_name = ID_TO_NAME[sid]  
        cat_name = CATS[cat]  
        links = DATA.get(subject_name, {}).get(cat_name, [])  
        text = f"✅ تم التحديث.\n\n🔗 الروابط ({subject_name} → {cat_name}):\n\n"  
        if links:  
            for i, link in enumerate(links, 1):  
                text += f"{i}- {link}\n"  
        else:  
            text += "🚫 لا توجد روابط حالياً."  

        await q.edit_message_text(text=text, reply_markup=kb_links_view(sid, cat))  
    return  

if data.startswith("back:cats:"):  
    _, _, sid = data.split(":")  
    await q.edit_message_text(f"📘 اختر القسم ({ID_TO_NAME[sid]}):", reply_markup=kb_categories(sid))  
    return  

if data == "back:menu":  
    await q.edit_message_text("📚 اختر المادة:", reply_markup=kb_subjects())  
    return

async def error_handler(update: object, context: ContextTypes.DEFAULT_TYPE):
print(f"⚠️ ERROR: {context.error}")
try:
if update and isinstance(update, Update) and update.effective_message:
await update.effective_message.reply_text("حدث خطأ غير متوقع. جرّب مرة ثانية.")
except:
pass

----------------- Keep-alive (Flask) -----------------

app_flask = Flask('')
@app_flask.route('/')
def home():
return "🤖 البوت شغال على Replit!"

def run_flask():
port = int(os.environ.get("PORT", 8080))
app_flask.run(host="0.0.0.0", port=port)

def keep_alive():
Thread(target=run_flask, daemon=True).start()

----------------- auto-ping (optional) -----------------

def auto_ping_loop():
while True:
try:
# use REPL_URL if provided (recommended)
url = os.environ.get("REPL_URL")
if not url:
slug = os.environ.get("REPL_SLUG")
owner = os.environ.get("REPL_OWNER")
if slug and owner:
url = f"https://{slug}.{owner}.repl.co"
if url:
try:
requests.get(url, timeout=10)
print("✅ Auto-ping sent to keep alive:", url)
except Exception as e:
print("⚠️ Auto-ping request failed:", e)
else:
# no public URL known; skip
print("ℹ️ No REPL_URL / REPL_SLUG+REPL_OWNER set; skipping auto-ping")
# wait 10 minutes
except Exception as e:
print("⚠️ Auto-ping error:", e)
time.sleep(600)

----------------- Build and run bot -----------------

def build_app():
app = Application.builder().token(TOKEN).build()
app.add_handler(CommandHandler("start", start_handler))
app.add_handler(CommandHandler("menu", menu_handler))
app.add_handler(CallbackQueryHandler(callback_handler))
app.add_error_handler(error_handler)
return app

def main():
# start flask to keep repl awake
keep_alive()
# start auto-ping thread (will use REPL_URL if you set it in Secrets)
Thread(target=auto_ping_loop, daemon=True).start()

app = build_app()  
print("🤖 البوت شغال! - Starting polling...")  

# robust polling loop: إذا صار استثناء يعيد المحاولة بعد تأخير  
while True:  
    try:  
        app.run_polling()  
    except Exception as e:  
        print("⚠️ Polling crashed:", e)  
        print("⏳ إعادة محاولة بعد 5 ثواني...")  
        time.sleep(5)  
        # ثم إعادة بناء التطبيق (في حالات نادرة قد يحتاج إعادة إنشاء)  
        try:  
            app = build_app()  
        except Exception as e2:  
            print("⚠️ Failed to rebuild app:", e2)  
            time.sleep(5)  
            continue

if name == "main":
main()
