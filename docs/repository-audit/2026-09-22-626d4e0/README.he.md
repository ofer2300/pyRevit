<div dir="rtl" align="right">

# נספחי ביקורת pyRevit — 22.09.2026

[הדוח המלא ליד README](../../../REPOSITORY_AUDIT.he.md) · [README הראשי](../../../README.md)

הביקורת מתייחסת לגרסה `626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f`. ההפניות והספירות מתארות את צילום המצב לפני הוספת דוח זה, בקשת השינוי והפצת הנספחים. זו מסירת תיעוד; הממצאים לא תוקנו בקוד המוצר.

## קריאה והורדה

- [החבילה המלאה, ZIP, כ־386 MB](https://github.com/ofer2300/pyRevit/releases/download/repository-audit-2026-09-22/pyRevit-audit.zip) — לחלץ ולפתוח `audit.html`; המלאי לחיפוש נמצא ב־`inventory.html`.
- [דוח HTML](audit.html) ו[מלאי HTML לחיפוש](inventory.html) — GitHub מציג קובצי HTML כמקור; להוריד ולפתוח מקומית, או להשתמש בחבילה המלאה.
- [כל קבצי ברירת המחדל](default-files.csv) — 3,777 רשומות, תפקיד, גודל, מזהי תוכן ופעולות שחולצו.
- [מפת 33 הענפים](branches.csv), [פירוט ההבדלים](branch-details.json), [156 תגי הגרסאות](tags.csv), [11,613 נקודות שמירה](commits.csv).
- [45,420 תכנים היסטוריים, CSV דחוס](historical-contents.csv.gz).
- [עצי כל 1,646 ההפניות, CSV דחוס](https://github.com/ofer2300/pyRevit/releases/download/repository-audit-2026-09-22/all-ref-files.csv.gz).
- [צילום מצב Git מלא, JSON דחוס](https://github.com/ofer2300/pyRevit/releases/download/repository-audit-2026-09-22/snapshot.json.gz).
- [הפניות שירות](service-refs.txt), [מאגרים חיצוניים](submodules.csv), [תלויות שחולצו](dependencies-static.json), [מטא־נתונים בינריים](binary-inspection.json).
- [עשרת הממצאים](findings.json), [משטחי GitHub](repository-surfaces.json), [ראיות משלימות](additional-evidence.json), [בדיקות תוצרי המקור](verification.json).
- [חתימות חבילת המקור](manifest-sha256.json). קובצי HTML שבמאגר הותאמו לקישורי הורדה; החתימות המקוריות מתייחסות לקבצים בחבילת ZIP.

## גבולות ומועד

`AUDIT_COVERAGE=PARTIAL`. התכנים נאספו מכנית ונבדקו מוקדי ליבה וסיכון; אין טענה לניתוח מלא של כל גרסה היסטורית, כל בינרי או כל מאגר חיצוני. Revit וקוד המוצר לא הופעלו. שדות `LOCAL_ONLY` בקובצי הצילום הם עובדות היסטוריות ממועד יצירתם ולא מצב הפרסום הנוכחי.

המידע הפרטי של קבלת שירות התזמור הוסר מהעותק הציבורי. לא פורסמו ערכי מפתחות, לוגים גולמיים או נתיבים מקומיים. החבילה המקורית המקומית נשמרה בנפרד; קובץ החתימות של החבילה הציבורית חושב מחדש לאחר הסינון.

</div>
