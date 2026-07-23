# אתר שי פאר · פיזיותרפיה

אתר תדמית לקליניקה פרטית לפיזיותרפיה בנהריה.
עמוד אחד, עברית מלאה (RTL), בלי שלב בנייה.

**Stack:** HTML + CSS + JavaScript. הכל בקובץ `index.html` אחד.
בלי React, בלי Tailwind, בלי npm. ההסבר למה — ב-`CLAUDE.md`.

---

## מבנה

```
index.html      האתר כולו
robots.txt
sitemap.xml
vercel.json     כותרות אבטחה, קאשינג, clean URLs
CLAUDE.md       הקשר לסשן Claude Code — לקרוא ראשון
docs/
  DESIGN.md     מערכת עיצוב וכללי טיפוגרפיה עברית
  CONTENT.md    מה עדיין חסר משי + placeholders להחלפה
  ROADMAP.md    תוכנית SEO ושלב ב'
```

---

## הרצה מקומית

אין שלב בנייה. אפשר פשוט לפתוח את `index.html` בדפדפן, או:

```bash
python3 -m http.server 8000
# ואז localhost:8000
```

---

## העלאה ל-Git

```bash
cd shaypeer-pt
git init
git add .
git commit -m "Initial commit: Hebrew RTL clinic site"
git branch -M main
```

פותחים ריפו ריק ב-GitHub (בלי README, בלי .gitignore — כבר יש), ואז:

```bash
git remote add origin https://github.com/<username>/shaypeer-pt.git
git push -u origin main
```

---

## פריסה ל-Vercel

1. [vercel.com](https://vercel.com) → **Add New → Project → Import Git Repository**
2. בוחרים את הריפו.
3. **Framework Preset: Other.** Build Command ריק. Output Directory ריק.
   (Vercel יזהה אתר סטטי לבד — לא לשנות כלום.)
4. **Deploy.** תוך פחות מדקה יש כתובת `xxx.vercel.app`.

מכאן כל `git push` ל-`main` מפרסם אוטומטית.
כל pull request מקבל כתובת preview משלו — שימושי להראות לשי לפני שעולה לאוויר.

### דומיין

**Settings → Domains → Add.** מוסיפים `shaypeer-pt.co.il` (או כל דומיין אחר),
ו-Vercel יראה בדיוק אילו רשומות DNS להזין אצל הרשם. תעודת SSL נוצרת אוטומטית.

אחרי חיבור הדומיין — לעדכן את הכתובת ב-`index.html` (canonical, og:url, JSON-LD),
ב-`robots.txt` וב-`sitemap.xml`.

---

## לפני שעולים לאוויר

יש placeholders בקובץ. הרשימה המלאה עם מחרוזות מדויקות להחלפה נמצאת ב-`docs/CONTENT.md`.
**מספר הטלפון והכתובת שבקובץ מומצאים** — אסור לפרסם ככה.
