# proxy-downloader

Backend workflow for the `proxy-downloader-simple.html` dashboard.

## מבנה
```
.github/workflows/download.yml   <- ה-workflow שרץ ב-Actions
README.md
```
(שימי/שים לב: קובץ ה-HTML עצמו לא צריך להיות בריפו הזה בכלל — הוא רץ אצלך בדפדפן
בלבד ומדבר עם ה-API של GitHub. אפשר להעלות אותו לריפו רק אם נוח לך, זה לא הכרחי.)

## הקמה
1. צרי/צור ריפו חדש (יכול להיות **פרטי**) והעלי/העלה אליו את `.github/workflows/download.yml`.
2. ב-Settings → Developer settings → Personal access tokens צרי/צור **Classic token** עם הרשאת `repo` (מלאה).
   - זה הטוקן שמוזן בטאב "הגדרות" בדשבורד.
3. (אופציונלי, למידע/וידאו שדורש חשבון מחובר) הוסיפי/הוסף Secret בשם `YT_COOKIES`
   ב-Settings → Secrets and variables → Actions, עם תוכן קובץ cookies.txt בפורמט Netscape.
4. פתחי/פתח את `proxy-downloader-simple.html` בדפדפן, מלאי/מלא owner/repo/token בטאב הגדרות, ולחצי/לחץ "בדיקת חיבור".

## איך זה עובד
1. הדשבורד שולח `repository_dispatch` (`event_type: download`) עם `client_payload`
   שמכיל `request_id`, `mode` (`video`/`file`), `url`, ו-`quality`.
2. ה-workflow למעלה נדלק, מוריד עם `yt-dlp` (מצב וידאו) או `curl` (מצב קובץ) לתוך `out/`.
3. הוא מעלה את מה שהוריד כ-artifact בשם `result-<request_id>`.
4. הדשבורד מאתר את ה-run לפי `run-name` (שחייב להתחיל ב-`dl-<request_id>` — כבר מוגדר בקובץ), ממתין לסיום, ואז מציע לך להוריד את ה-artifact (או לפתוח את דף הריצה ולהוריד ידנית).

## ⚠️ שימי/שים לב
- כל מה שהופך לזמין ב-Actions runner הוא **ציבורי בפועל** אם הריפו ציבורי — שימי/שים לב אם URL-ים או תוכן רגישים.
- אחריות שימוש: תורידי/תוריד רק תוכן שמותר לך להוריד (התוכן שלך, רישיון פתוח, וכו') — הורדת חומר מוגן בזכויות יוצרים ללא רשות עלולה להפר תנאי שימוש/חוק.
- ל-Artifacts יש ברירת מחדל של `retention-days: 1` כדי לא להצטבר על שטח אחסון — אפשר לשנות בקובץ.
