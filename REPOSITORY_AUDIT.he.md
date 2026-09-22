<div dir="rtl" align="right">

# pyRevit — ביקורת מאגר בעברית

> **מסמך קבוע במאגר:** הדוח להלן מתעד את הביקורת המקורית מ־22.09.2026 על `626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f`. תוספת התיעוד וההפצה נוצרה לאחר צילום המצב. הסימון `LOCAL_ONLY` בדוח מתאר את מועד האיסוף המקורי; המסמך מתפרסם כעת במאגר בעקבות בקשת הבעלים. כיסוי הביקורת נשאר `PARTIAL` — הפרסום אינו משלים בדיקות שלא בוצעו.

[חזרה ל־README](README.md) · [מדריך לנספחים](docs/repository-audit/2026-09-22-626d4e0/README.he.md) · [הורדת כל החבילה](https://github.com/ofer2300/pyRevit/releases/download/repository-audit-2026-09-22/pyRevit-audit.zip)


המאגר מאפשר לבנות ולהפעיל כלי אוטומציה בתוך Autodesk Revit: פקודות לעריכת מודלים, ניהול תכניות ונתונים, ממשקי משתמש והפצת הרחבות לצוותים. זהו בסיס שימושי לעבודת הנדסה, אך העותק האישי שנבדק אינו מוכיח התקנה שנבנתה ונבחנה אצלך.

**התוצאה המעשית:** לפני בניית סביבת עבודה מקצועית על המאגר, יש לבחור ענף וגרסה במפורש, לטפל בשמירת אסימוני גישה ובעדכונים הדורסים שינויים, ולהגדיר בדיקות על מודלי ניסוי. לא הוכחה תקינות תפעולית בתוך Revit.

`AUDIT_COVERAGE=PARTIAL` · `DELIVERY=LOCAL_ONLY`

הושלם מלאי מכני רחב של Git ושל משטחי GitHub הנגישים. ניתוח משמעות מעמיק נעשה ברכיבי ליבה ובמוקדי סיכון; לא בכל גרסה של כל קובץ. קבצים בינריים, מאגרים חיצוניים ומסכי שירות חסומים מונעים טענת כיסוי מלא.

<a id="baseline"></a>

## נקודת הייחוס והמסירה

| נתון | תוצאה |
| --- | --- |
| מאגר | [ofer2300/pyRevit](https://github.com/ofer2300/pyRevit) — ציבורי; fork=false לפי API |
| תחילת איסוף Git | 2026-09-22T02:26:27.850227+00:00 |
| סיום איסוף Git | 2026-09-22T02:37:02.456391+00:00 |
| התחלת המשימה | 22.09.2026 סביב 05:22, שעון ישראל; נתוני API נאספו בנפרד לפני סיום Git |
| ענף ברירת מחדל | copilot/explore-codebase-ofna-fix |
| SHA — מזהה מדויק של תוכן הגרסה | `626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f` |
| קבצים בברירת המחדל | 3777 קבצים ו־8 הפניות למאגרים חיצוניים |
| ענפים ותגים | 33 ענפים, 156 תגי גרסה, 1,455 הפניות github-services/pull ועוד 2 הפניות chunked-upload; סך הכול 1,646 הפניות |
| היסטוריה נגישה | 11,613 commits — נקודות שמירה; 45,420 תכנים ייחודיים |
| היקף תוכן לא דחוס | 13,578,428,097 בתים, סכום כל התכנים הייחודיים בהיסטוריה |
| בדיקת יציבות | רשימות הפניות השרת לפני ואחרי האיסוף זהות; snapshot מפנה למזהים קבועים |
| מסירה | דוח וקבצי ראיות מקומיים בלבד. לא בוצעו commit, push, PR, מיזוג או שינוי הגדרות. |

<a id="architecture"></a>

## איך החלקים מתחברים

הקלט הוא סביבת Revit ומודל פתוח, פקודת משתמש, הגדרות והרחבות מותקנות. Revit טוען קובץ `.addin` שמפנה לרכיב `pyRevitLoader.dll`. הרכיב מפעיל את מנהל ההפעלה, שמגלה הרחבות ובונה את הכפתורים. לחיצה על כפתור עוברת למבצע הפקודות, שבוחר מנוע Python, C# או סוג אחר. הפקודה קוראת לממשק התכנות של Revit — הפעולות שהתוכנה חושפת למפתחים — ומחזירה חלון, דוח, קובץ או שינוי במודל.

**זרימת עבודה:** מודל ופקודה ← טעינת pyRevit ← גילוי הרחבות ← בחירת מנוע ← פעולה דרך Revit ← שינוי/דוח/קובץ. זוהי מפת מימוש שנקראה בקוד; לא תיעוד של הרצה שבוצעה בביקורת.

| רכיב | מקור | תפקיד וקשר |
| --- | --- | --- |
| טוען ראשוני | [dev/pyRevitLoader/Source/PyRevitLoaderApplication.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/dev/pyRevitLoader/Source/PyRevitLoaderApplication.cs) | נקודת הכניסה של Revit, טעינת רכיבים ותחילת session. session הוא מחזור הפעלה. קיימים מסלולי טעינה ב־C# וב־Python. |
| מנהל הפעלה | [pyrevitlib/pyrevit/loader/sessionmgr.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/loader/sessionmgr.py) | מכין פלט, תצורה, הרחבות, פעולות המופעלות באירועי Revit וחיבורי שירות. עדכון אוטומטי ושרת Routes תלויים בהגדרות. |
| גילוי הרחבות | [pyrevitlib/pyrevit/extensions/extensionmgr.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/extensions/extensionmgr.py) | מאתר חבילות ומרכיב את מבנה הממשק; שמות תיקיות וסיומות כגון .pushbutton קובעים תפקיד. |
| מבצע פקודות | [dev/pyRevitLabs.PyRevit.Runtime/ScriptExecutor.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/dev/pyRevitLabs.PyRevit.Runtime/ScriptExecutor.cs) | בוחר מנוע לפי סוג הסקריפט, מטפל בקודי תוצאה ובבקשה להפעלה דרך מנגנון האירועים של Revit. |
| ספריית המשתמש | [pyrevitlib/pyrevit/__init__.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/__init__.py) | מספקת הקשר של Revit וקבועים; תתי־הספריות מטפלות במודל, חלונות, פלט, הגדרות ושירותים. |
| הרחבות | [extensions/extensions.json](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/extensions/extensions.json) | קטלוג הרחבות. שמונת אוספי ההרחבות הכלולים כוללים ליבה, כלים, תגיות, תבניות, לימוד ובדיקות. |
| כלי פקודות | [dev/pyRevitLabs/pyRevitCLI/PyRevitCLI.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/dev/pyRevitLabs/pyRevitCLI/PyRevitCLI.cs) | CLI הוא ממשק פקודות טקסט: התקנה, צירוף ל־Revit, עדכון וניהול עותקים. פעולות מסוימות משנות או מסירות קבצים. |
| בנייה והפצה | [build/README.md](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/build/README.md) | המרת מקור לרכיבים מוכנים, אריזה, חתימה ופרסום. רוב שלבי ההפצה מכוונים למיזם המקורי ולשירותי חתימה חיצוניים. |
| נתוני שימוש | [pyrevitlib/pyrevit/telemetry/__init__.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/telemetry/__init__.py) | Telemetry הוא איסוף נתוני שימוש; קיימות אפשרויות קובץ ושרת. לא נבדק שרת חי או מידע שנשלח בפועל. |

**ערך הנדסי אפשרי:** ספריות וכלים קיימים יכולים לקצר פיתוח פקודות לבחירת אלמנטים, ניהול תכניות, תיוג וייצוא. חיסכון כספי או שעות עבודה לא נמדד בביקורת. כל כלי שמשנה מודל מחייב בדיקת תוצאה מקצועית; קיומו של קוד אינו מוכיח נכונות של כמויות, סיווגים או מסמכי מכרז.

## מפת תיקיות וקבוצות קבצים

הטבלה מתייחסת לענף ברירת המחדל. המלאי המקושר מפרט כל תיקייה ותת־תיקייה וכל קובץ בענף זה. נספח כל ההפניות כולל גם עצי ענפים ותגים אחרים. תפקידים שמופקים לפי נתיב הם סיווג עזר, לא טענה שכל קובץ עבר קריאה אנושית מלאה.

| תיקייה | קבצים | מה נמצא בה |
| --- | --- | --- |
| .cursor | 1 | הגדרות סביבת עורך |
| .gitattributes | 1 | קובץ שורש; מפורט במלאי |
| .github | 15 | הגדרות אוטומציה, הנחיות סוכנים ודיווח תקלות |
| .gitignore | 1 | קובץ שורש; מפורט במלאי |
| .gitmodules | 1 | קובץ שורש; מפורט במלאי |
| .pyrevitargs | 1 | קובץ שורש; מפורט במלאי |
| .vscode | 3 | הגדרות סביבת פיתוח |
| AGENTS.md | 1 | קובץ שורש; מפורט במלאי |
| CLAUDE.md | 1 | קובץ שורש; מפורט במלאי |
| CODE_OF_CONDUCT.md | 1 | קובץ שורש; מפורט במלאי |
| CONTRIBUTING.md | 1 | קובץ שורש; מפורט במלאי |
| CREDITS.md | 1 | קובץ שורש; מפורט במלאי |
| LICENSE.rtf | 1 | קובץ שורש; מפורט במלאי |
| LICENSE.txt | 1 | קובץ שורש; מפורט במלאי |
| Pipfile | 1 | קובץ שורש; מפורט במלאי |
| Pipfile.lock | 1 | קובץ שורש; מפורט במלאי |
| README.md | 1 | קובץ שורש; מפורט במלאי |
| SECURITY.md | 1 | קובץ שורש; מפורט במלאי |
| build | 64 | בניית המוצר, חתימה, אריזה, פרסום ובדיקות עזר |
| dev | 384 | קוד C# המחבר ל־Revit, מנועי הפעלה, כלי פקודות ועזרי פיתוח |
| docs | 15 | תיעוד התקנה, ארכיטקטורה, פיתוח ותהליך הפצה |
| extensions | 1795 | פקודות משתמש, חלונות, אייקונים ודוגמאות בתוך Revit |
| extras | 27 | משאבי עזר לממשק ולהפקת נכסים |
| licenses | 50 | נוסחי רישוי של רכיבים כלולים |
| mkdocs.yml | 1 | קובץ שורש; מפורט במלאי |
| pyRevitfile | 1 | קובץ שורש; מפורט במלאי |
| pyproject.toml | 1 | קובץ שורש; מפורט במלאי |
| pyrevitlib | 223 | ספריות Python לגישה למודל, לממשק, לתצורה ולשירותי pyRevit |
| release | 81 | נתוני גרסה, תבניות התקנה ומשאבי הפצה |
| site-packages | 1101 | עותקים של ספריות Python חיצוניות המצורפים למוצר |

[מלאי אינטראקטיבי של כל הקבצים והתיקיות בברירת המחדל](docs/repository-audit/2026-09-22-626d4e0/inventory.html) · [טבלת קבצים מלאה](docs/repository-audit/2026-09-22-626d4e0/default-files.csv) · [עצי כל ההפניות, בקובץ דחוס](https://github.com/ofer2300/pyRevit/releases/download/repository-audit-2026-09-22/all-ref-files.csv.gz) · [כל התכנים הייחודיים ונתיביהם בהיסטוריה](docs/repository-audit/2026-09-22-626d4e0/historical-contents.csv.gz).

<a id="findings"></a>

## ממצאים מבוססי ראיות

החומרה היא הערכת תעדוף במסגרת סקירה זו, לא ציון אבטחה תקני. ממצאי סיכון מתארים תנאי הפעלה ונתיבי קוד; אין כאן טענה שנגרם נזק בפועל.

### F01 · ברירת המחדל מציגה קו עבודה ישן

חומרה: גבוהה · ביטחון: גבוהה; השוואת היסטוריה ותוכן ב־Git

**[עובדה]** ענף ברירת המחדל הוא copilot/explore-codebase-ofna-fix, בגרסה 626d4e0 מ־21.07.2026. develop מכיל את כל ההיסטוריה שלו ועוד 511 commits, ובין העצים שונים 1,073 קבצים. master מכיל עוד 166 commits ביחס לברירת המחדל. קובצי release/version מציגים בהתאמה 6.5.4, 7.0.0.26237 ו־6.5.5.26237.

**[מסקנה]** מי שנכנס למאגר או משכפל בלי בחירת ענף עשוי לעבוד על קוד ישן ולהחמיץ תיקונים. develop אינו בהכרח גרסה יציבה רק משום שהוא חדש יותר.

**[המלצה]** להחליט מהו קו השימוש האישי: גרסת הפצה מזוהה או קו פיתוח. רק לאחר בדיקות Revit מתאימות לשקול שינוי ברירת מחדל. לא בוצע שינוי כזה בביקורת.

**ראיות:** branches.csv; branch-details.json; repository-surfaces.json<br>[release/version](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/release/version)

### F02 · אסימון גישה להרחבה נשמר בהגדרות ללא הצפנה

חומרה: גבוהה · ביטחון: גבוהה במסלול הכתיבה הסטטי; שימוש בפועל לא נבדק

**[עובדה]** בחלון Extensions נשמר הערך token אל config.token, ובנתיב התקנה גם אל config.password. לאחר מכן נקראת save_changes. מנגנון configparser.save כותב את ההגדרות כטקסט UTF-8. דפוס השמירה קיים גם ב־develop. בענף claude/secure-github-token-storage-WpVP3 נמצא מימוש DPAPI — הצפנה הקשורה לחשבון Windows — שאינו קיים בנתיב המקביל ב־develop.

**[מסקנה]** כאשר משתמש מזין אסימון, מי שמקבל את קובץ ההגדרות או גיבוי שלו עלול לקבל גם את האסימון. לא נבדקו הגדרות מקומיות שלך ולא הוכח שאסימון שלך נשמר או נחשף.

**[המלצה]** לתכנן אחסון מאובטח והעברה מבוקרת של ערכים ישנים, כולל גיבוי והתנהגות במעבר משתמש. הענף הקיים הוא מועמד לבדיקה, לא תיקון שאושר לשילוב אוטומטי.

**ראיות:** כתיבת token בשורות 604–617 ו־713–719; configparser.py בשורות 260–265<br>[extensions/pyRevitCore.extension/pyRevit.tab/pyRevit.panel/Extensions.smartbutton/script.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/extensions/pyRevitCore.extension/pyRevit.tab/pyRevit.panel/Extensions.smartbutton/script.py) · [pyrevitlib/pyrevit/coreutils/configparser.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/coreutils/configparser.py)

### F03 · עדכון Clone עלול לדרוס שינויים מקומיים

חומרה: גבוהה · ביטחון: גבוהה במסלול הקוד; תרחיש כשל לא הופעל

**[עובדה]** GitInstaller.ForcedUpdate מבצע CheckoutModifiers.Force לפני Commands.Pull. PyRevitClones.Update קורא לו בעותק Git. במסלול שאינו Git, ReDeployClone קורא ל־Delete לפני DeployFromImage. Clone הוא עותק מקומי של התקנה או מאגר.

**[מסקנה]** התאמות שנעשו בתוך עותק ההתקנה עלולות להימחק בזמן עדכון. במסלול התקנה מחדש, כשל לאחר המחיקה עלול להשאיר את ההתקנה חסרה. לא הורצה אף פקודת עדכון.

**[המלצה]** לשמור הרחבות והתאמות במאגר נפרד; לפני שינוי התנהגות העדכון לבדוק גילוי שינויים מקומיים, גיבוי ושחזור. אין להשתמש בפקודת עדכון זו כתחליף לתהליך Git מבוקר.

**ראיות:** ForcedUpdate בשורות 135–156; ReDeployClone בשורות 622 ואילך<br>[dev/pyRevitLabs/pyRevitLabs.Common/GitInstaller.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/dev/pyRevitLabs/pyRevitLabs.Common/GitInstaller.cs) · [dev/pyRevitLabs/pyRevitLabs.PyRevit/PyRevitClones.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/dev/pyRevitLabs/pyRevitLabs.PyRevit/PyRevitClones.cs)

### F04 · הפעלת שרת Routes דורשת הגבלת חשיפה

חומרה: גבוהה מותנית · ביטחון: גבוהה לגבי ברירות המחדל והנתיב; חשיפה חיה לא נבדקה

**[עובדה]** Routes הוא ממשק HTTP שמאפשר לתוכנה אחרת לקרוא לפעולות רשומות בתוך Revit. ברירת המחדל היא disabled. כתובת ההאזנה המוגדרת כברירת מחדל היא מחרוזת ריקה, ו־RoutesServer מוסר אותה ל־HTTPServer; הקוד מציג מצב זה כ־0.0.0.0, כלומר כלל ממשקי הרשת. במסלול הבקשה הכללי שנבדק לא נמצאה בדיקת זהות מרכזית לפני הפעלת הפעולה.

**[מסקנה]** אם מפעילים את השירות עם הגדרות הכתובת הריקות ובלי הגנה חיצונית, היקף הגישה עשוי להיות רחב מהמחשב המקומי. החשיפה בפועל תלויה בחומת האש, בהגדרות ובפעולות שנרשמו; היא לא נבדקה.

**[המלצה]** לפני שימוש להגדיר כתובת מקומית מפורשת ולבחון אימות זהות, הרשאות וזמן המתנה מרבי. אין ראיה ששירות כזה פועל במחשב שלך.

**ראיות:** ConfigsRoutesServerDefault=false; ConfigsRoutesHostDefault=""; _handle_route ו־RoutesServer.__init__<br>[dev/pyRevitLabs/pyRevitLabs.PyRevit/PyRevitConsts.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/dev/pyRevitLabs/pyRevitLabs.PyRevit/PyRevitConsts.cs) · [pyrevitlib/pyrevit/routes/server/server.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/routes/server/server.py) · [pyrevitlib/pyrevit/userconfig.py](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyrevitlib/pyrevit/userconfig.py)

### F05 · שינוי נוסף בבקשת שינוי אינו מפעיל CI מחדש

חומרה: בינונית–גבוהה · ביטחון: גבוהה; נקרא תנאי ההפעלה בשני הענפים

**[עובדה]** CI — סדרת בנייה ובדיקות אוטומטית — מוגדרת ב־ci.yml עבור pull_request מסוג opened ו־reopened בלבד. האירוע synchronize, שמשמש להוספת commits ל־PR פתוח, אינו ברשימה. גם ב־develop התנאי הזה נשאר. push מוגבל ל־develop, master ותגים v*. PR הוא בקשה לשלב שינוי בענף אחר.

**[מסקנה]** אם יופעל התהליך בעתיד, ריצה על פתיחת PR לא תעיד על תקינות השינויים המאוחרים בו. במאגר שלך אין ריצות שמאפשרות לבדוק אם הפער כבר גרם לאירוע.

**[המלצה]** לפני הסתמכות על CI, להוסיף בדיקה על עדכון PR ולוודא שתוצאת הבדיקה קשורה ל־SHA האחרון — מזהה הגרסה המדויקת.

**ראיות:** ci.yml שורות 3–6<br>[.github/workflows/ci.yml](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.github/workflows/ci.yml) · [docs/ci-cd.md](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/docs/ci-cd.md)

### F06 · היסטוריית קוד עשירה ללא ראיות הפעלה במאגר האישי

חומרה: בינונית–גבוהה · ביטחון: גבוהה בתוצאות API בזמן הבדיקה; סיבת היעדר רישום workflows לא הוכחה

**[עובדה]** GitHub החזיר 0 PR, 0 Issues, 0 Releases, 0 workflows רשומים ו־0 ריצות. בעץ ברירת המחדל קיימים שבעה קובצי workflows ו־156 תגי גרסה. Actions מוגדר enabled=true. שלבי פרסום וחתימה רבים מוגבלים במפורש ל־pyrevitlabs/pyRevit.

**[מסקנה]** המאגר הוא מקור קוד נרחב, אך אינו מוכיח שהתקנה נבנתה או נבדקה תחת ofer2300. מסמכים ותגובות commits שמפנים ל־PR במיזם המקורי אינם ביקורות שאפשר לקרוא דרך ה־PR API של המאגר האישי.

**[המלצה]** להפריד בין קוד מיובא לבין סביבת הפצה מתפקדת; אם מתוכננת הפצה אישית, לתכנן בנייה ובדיקות עצמאיות ורק אחר כך הרשאות וחתימה.

**ראיות:** repository-surfaces.json<br>[.github/workflows/ci.yml](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.github/workflows/ci.yml) · [.github/workflows/release.yml](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.github/workflows/release.yml) · [.github/workflows/wip.yml](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.github/workflows/wip.yml)

### F07 · לא נמצאה הגנת ענפים מדווחת

חומרה: בינונית · ביטחון: גבוהה לגבי השדות שנחשפו; מדיניות שאינה נחשפת אינה מכוסה

**[עובדה]** כל 33 הענפים הוחזרו עם protected=false. רשימת rulesets — כללי הגנה החלים על המאגר — נקראה ונמצאה ריקה. קריאות הגנת ענף פרטניות החזירו 404; אינן הבסיס היחיד לממצא.

**[מסקנה]** אין ראיה לדרישה מובנית של ביקורת או בדיקה לפני שינוי ענפי העבודה. הדבר מגדיל את ההסתמכות על זהירות ידנית.

**[המלצה]** אם המאגר ישמש בסיס עבודה שוטף, לקבוע תחילה ענף מרכזי, בדיקות רלוונטיות ודרך שחזור, ואז לשקול הגנה מתאימה. לא שונו הרשאות או הגנות.

**ראיות:** repository-surfaces.json: branches, rulesets<br>

### F08 · מקור מלא אינו מבטיח חבילת התקנה עצמאית

חומרה: בינונית · ביטחון: גבוהה במלאי ובתיעוד; לא בוצעה בנייה או השוואת DLL למקור

**[עובדה]** בעץ ברירת המחדל אין תיקיית bin. pyRevitfile מגדיר מנועי IronPython 2.7.12, IronPython 3.4.2 ו־CPython 3.12.3. שמונה הפניות submodule מצביעות למאגרים אחרים; submodule הוא קישור לגרסה מדויקת של מאגר נוסף. קובצי DLL שמורים גם ב־dev/libs. התיעוד מתאר הורדת bin ממוצרי בנייה ב־GitHub וחלופות מהמיזם המקורי.

**[מסקנה]** שכפול המאגר האישי אינו לבדו התקנה שניתן להפעיל. זמינות רכיבים חיצוניים והתאמה בין מקור ל־DLL משפיעות על שחזור ההתקנה.

**[המלצה]** להגדיר חבילת התקנה שניתן לזהות ולשחזר, עם גרסאות ותכולה מוסכמות. לא להציג חתימת קובץ או שם tag כהוכחת התאמה למקור.

**ראיות:** submodules.csv; default-files.csv<br>[.gitmodules](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.gitmodules) · [pyRevitfile](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyRevitfile) · [docs/ci-cd.md](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/docs/ci-cd.md) · [build/README.md](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/build/README.md)

### F09 · הבדיקות שנמצאו אינן הוכחת עבודה בתוך Revit

חומרה: בינונית · ביטחון: גבוהה לגבי תכולת קבצי הבדיקה; אין תוצאות הרצה

**[עובדה]** ci.yml מפעיל dotnet test על build/tests/Build.Tests.csproj. שם נמצאו בדיקות לעזרי גרסאות, מטא־נתונים, הודעות, נעילת קבצים וסיכומי חתימה. לדוגמה SigningHelperTests בודק ספירת סיומות קבצים ולא מאמת חתימה דיגיטלית. בדיקות וכלי דוגמה נוספים קיימים ב־pyRevitDevTools, וב־develop נוספו גם tests/test_runtime_logger.py.

**[מסקנה]** גם הצלחה בבדיקות הבנייה לא תוכיח שכלי עריכת מודל, Keynotes, Dynamo או מנועי Python עובדים בכל גרסת Revit.

**[המלצה]** להגדיר מטריצת בדיקה קטנה של גרסאות Revit, מנועים ופקודות הנדסיות נדרשות; להפעיל על מודלי ניסוי עם השוואת תוצאה לפני/אחרי. זו המלצה להמשך, לא בדיקה שבוצעה.

**ראיות:** קריאת קובצי הבדיקה ותנאי CI<br>[.github/workflows/ci.yml](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.github/workflows/ci.yml) · [build/tests/Build.Tests.csproj](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/build/tests/Build.Tests.csproj) · [build/tests/SigningHelperTests.cs](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/build/tests/SigningHelperTests.cs)

### F10 · תיעוד הארכיטקטורה אינו מסונכרן בכל פרט

חומרה: נמוכה–בינונית · ביטחון: גבוהה; הצלבה בין מסמכים לעץ

**[עובדה]** docs/architecture.md מזכיר IronPython 3.4.0 ונתיב dev/extensions/extensions.json. pyRevitfile מציג IPY342, והקטלוג נמצא ב־extensions/extensions.json. הוראות הסוכנים מתארות תיקיות bin ו־static שאינן קיימות בעץ ברירת המחדל שנבדק.

**[מסקנה]** מפתח או סוכן עלולים לאתר רכיב במקום לא נכון או להניח גרסת מנוע שונה. אלה פערי תיעוד, לא הוכחה לכשל הפעלה.

**[המלצה]** לעדכן את דף הארכיטקטורה והוראות הסוכנים מתוך הגרסה שנבחרה כבסיס עבודה.

**ראיות:** עץ הקבצים וערכי מנועים בנקודת הייחוס<br>[docs/architecture.md](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/docs/architecture.md) · [.github/copilot-instructions.md](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/.github/copilot-instructions.md) · [pyRevitfile](https://github.com/ofer2300/pyRevit/blob/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f/pyRevitfile)

<a id="branches"></a>

## ענפים, גרסאות והיסטוריה

ענף הוא קו עבודה; תג הוא שם שמסמן נקודת גרסה; commit הוא נקודת שמירה. מספר commits אינו מספר תכונות או תקלות. ההפרשים להלן מחושבים מול ענף ברירת המחדל. בעמודת develop מופיע מספר נקודות שמירה של הענף שאינן נגישות מהראש של develop; ההיסטוריה יכולה לכלול מיזוגים ושינויים שקולים ולכן אין להסיק מכך לבדו שקוד חסר.

| ענף | גרסה | ייחודי לענף מול ברירת מחדל | ייחודי לברירת מחדל | קבצים שונים | commits שאינם ב־develop |
| --- | --- | --- | --- | --- | --- |
| add-blendit-extension | [`eb2a137ae6`](https://github.com/ofer2300/pyRevit/tree/eb2a137ae659072d910798b4e2115372dcccb4c8) | 2 | 32 | 27 | 2 |
| claude/secure-github-token-storage-WpVP3 | [`dd0301fe2e`](https://github.com/ofer2300/pyRevit/tree/dd0301fe2e4ec9ff6ab256cb84209eb1cd95c17c) | 12 | 116 | 125 | 12 |
| copilot/add-solution-proposal-for-issue-3303 | [`b6f9b547b8`](https://github.com/ofer2300/pyRevit/tree/b6f9b547b8063cb0cf7ec8e6566cb54ce936b77f) | 0 | 634 | 1308 | 0 |
| copilot/check-dll-signing-status | [`6081462c62`](https://github.com/ofer2300/pyRevit/tree/6081462c62f64a5cb30ca60cbc7857d4834dcae2) | 1 | 1106 | 1434 | 1 |
| copilot/explore-codebase-ofna-fix | [`626d4e0348`](https://github.com/ofer2300/pyRevit/tree/626d4e0348f5e1aa6d2be4d91876ba74cbf39c1f) | 0 | 0 | 0 | 0 |
| copilot/fix-3619-issue-with-x | [`fa16d0315c`](https://github.com/ofer2300/pyRevit/tree/fa16d0315c9dc6c9f25887b498c02e9fef60429f) | 395 | 0 | 450 | 0 |
| copilot/fix-custom-tools-dynamo-scripts | [`3e57d7bb83`](https://github.com/ofer2300/pyRevit/tree/3e57d7bb83e1a6444baf15f33a70093f662150d1) | 398 | 0 | 451 | 3 |
| copilot/fix-extension-startup-py-scripts | [`c06c6a77a7`](https://github.com/ofer2300/pyRevit/tree/c06c6a77a7a4ae817cd06f7b77dc83593b71ab1f) | 369 | 0 | 413 | 0 |
| copilot/git-tag-v6-5-5 | [`d9ea5286ae`](https://github.com/ofer2300/pyRevit/tree/d9ea5286aeeea61a1e858e9be87881f62df7963a) | 231 | 0 | 320 | 0 |
| copilot/handle-comments-from-romans-review | [`b7c4380c4e`](https://github.com/ofer2300/pyRevit/tree/b7c4380c4edddca5ada7fda42853baf76cc28359) | 272 | 0 | 363 | 0 |
| copilot/review-dependabot-vulnerability-68 | [`6e25554682`](https://github.com/ofer2300/pyRevit/tree/6e2555468214a2c36bd439516551099dac7441e4) | 0 | 451 | 1221 | 0 |
| copilot/review-latest-pr | [`70524a36b4`](https://github.com/ofer2300/pyRevit/tree/70524a36b4cdb5bcfa00e5696583f994d227b194) | 1 | 2129 | 1731 | 1 |
| cursor/bump-654-d042 | [`91e21a48ca`](https://github.com/ofer2300/pyRevit/tree/91e21a48ca67cc4d678abce95eee4c48aad58b7a) | 3 | 34 | 32 | 3 |
| cursor/fix-admin-install-config-save-9130 | [`1b64be9292`](https://github.com/ofer2300/pyRevit/tree/1b64be9292b4433da19dacea20b7ccfbdbab3a6f) | 71 | 0 | 47 | 0 |
| cursor/fix-ci-binaries-prune-d938 | [`d8eb6cc7c7`](https://github.com/ofer2300/pyRevit/tree/d8eb6cc7c7377add4dc528187f2b75b817eaf6a2) | 178 | 0 | 138 | 0 |
| cursor/persist-extension-credentials-6417 | [`11f79e85d8`](https://github.com/ofer2300/pyRevit/tree/11f79e85d875bbf4e456d5cbe3fd911289cd7680) | 64 | 0 | 44 | 0 |
| cursor/release-653-d042 | [`90e57d4776`](https://github.com/ofer2300/pyRevit/tree/90e57d477660938fff37129f51e4152c2ac8ba43) | 1 | 35 | 34 | 1 |
| cursor/release-654-a0d3 | [`54fb4a9f7f`](https://github.com/ofer2300/pyRevit/tree/54fb4a9f7f44eb2ee5175320689c097519c20bc4) | 159 | 0 | 100 | 1 |
| cursor/release-654-to-master-a0d3 | [`54fb4a9f7f`](https://github.com/ofer2300/pyRevit/tree/54fb4a9f7f44eb2ee5175320689c097519c20bc4) | 159 | 0 | 100 | 1 |
| develop | [`7782e50268`](https://github.com/ofer2300/pyRevit/tree/7782e50268c9ee12fe0dacda32de92b8e5f0ec1f) | 511 | 0 | 1073 | 0 |
| docs | [`866e5df7cd`](https://github.com/ofer2300/pyRevit/tree/866e5df7cd9be01327fdf413392faa775711c7b0) | 2 | 54 | 71 | 2 |
| feat/keynote-multi-project-and-clipboard | [`c50eab1a31`](https://github.com/ofer2300/pyRevit/tree/c50eab1a31538f215e0fd276f3c6406faab7e189) | 472 | 0 | 491 | 6 |
| feature/universal-ui-host-poc | [`ac2cf2dcad`](https://github.com/ofer2300/pyRevit/tree/ac2cf2dcadecd384f3fac51c013f4438f3aae4c3) | 389 | 0 | 439 | 9 |
| features/configuration | [`a36e75569a`](https://github.com/ofer2300/pyRevit/tree/a36e75569af91c6e42b80d76cd007d2cfb4df52a) | 54 | 2327 | 1803 | 54 |
| fix/config-read-telemetry-path | [`f48b3be012`](https://github.com/ofer2300/pyRevit/tree/f48b3be01253e93b08b13cf1c736d64d9073a572) | 0 | 8 | 16 | 0 |
| fix/keynote-modeless-bundle-and-close-reentrancy | [`43c8f7ebb0`](https://github.com/ofer2300/pyRevit/tree/43c8f7ebb0a1829b44a25bf8fb2c6d772ef6133b) | 512 | 0 | 1082 | 8 |
| fix/normalize-revit-uiapplication-builtin | [`169819d8a2`](https://github.com/ofer2300/pyRevit/tree/169819d8a2ac471ebef1a0f7a7503d1a40137e99) | 470 | 0 | 488 | 4 |
| fix/output-window-regressions | [`27187f7164`](https://github.com/ofer2300/pyRevit/tree/27187f7164c2a22f1eef3c2cceeef55d264e3711) | 278 | 0 | 369 | 0 |
| fix/routes | [`c7d90cb4b8`](https://github.com/ofer2300/pyRevit/tree/c7d90cb4b8dc16d11cfcf71adee1125faac09c89) | 404 | 0 | 453 | 0 |
| fix/winget-exe-only | [`baddc92f34`](https://github.com/ofer2300/pyRevit/tree/baddc92f34ab3e8ab060443b4b0544d641eed90b) | 165 | 0 | 103 | 7 |
| gh-pages | [`efeed2dd32`](https://github.com/ofer2300/pyRevit/tree/efeed2dd32f1816cc26da7ff9f23380cd606bc5d) | 1 | 9724 | 3995 | 1 |
| master | [`b4bcea3508`](https://github.com/ofer2300/pyRevit/tree/b4bcea35088a28e8b824aa302d9aa50b4ff0c41f) | 166 | 0 | 103 | 8 |
| tay0thman-KM-1 | [`239da03e5b`](https://github.com/ofer2300/pyRevit/tree/239da03e5bb9c02c1623f2702d1984ac2d2bb9c7) | 378 | 0 | 414 | 0 |

ב־`gh-pages` נמצאה היסטוריה עצמאית ללא אב משותף ל־develop, המיועדת לאתר שנוצר אוטומטית; עצם קיום הענף אינו הוכחה ש־Pages פעיל במאגר זה. שני ענפי release-654 מצביעים לאותו commit. ענפי Initial plan מסוימים מכילים commit בלי שינוי קבצים מול האב המשותף — שמם אינו הוכחה למימוש.

ענפים שראויים לבחינה ממוקדת: אחסון אסימונים מוצפן; תיקון Dynamo המחליף יצירה לפי שם Assembly באיתור רכיב שכבר נטען; הרחבת Keynotes לעבודה בין מסמכים והעתקה; אבטיפוס ממשק בחלון מבודד; ואחידות הידית `__revit__` שהסקריפטים מקבלים. שמות ותיאורי commits מוצלבים עם רשימות שינויי קבצים בנספח; לא בוצעה בדיקת התנהגות של המימושים.

במלאי נמצאו גם 1,455 הפניות תחת `refs/github-services/pull` ושתי הפניות `chunked-upload`. הן שונות מהמרחב הרגיל `refs/pull`, ונקראו כולן על ידי הסורק. ענפים ותגים בלבד מובילים ל־10,440 commits; הוספת כל הפניות השירות מרחיבה את ההיסטוריה הנגישה ל־11,613 commits. רשימת PR בממשק GitHub של המאגר האישי עדיין ריקה: זמינות מקור היסטורי אינה זמינות דיונים, ביקורות או אישורים. [מלאי הפניות השירות](docs/repository-audit/2026-09-22-626d4e0/service-refs.txt).

ההיסטוריה כוללת קוד שקדם ליצירת המאגר האישי ב־13.09.2026. GitHub מדווח `fork=false`; לכן אין לתאר אותו כ־Fork רשום. דמיון לשמות ולזרימות של pyrevitlabs והיסטוריית commits מעידים על קשר למיזם המקורי, אך דרך העתקת המאגר לא הוכחה.

[מפת ענפים](docs/repository-audit/2026-09-22-626d4e0/branches.csv) · [commits ושינויים ייחודיים לכל ענף](docs/repository-audit/2026-09-22-626d4e0/branch-details.json) · [156 תגי הגרסאות](docs/repository-audit/2026-09-22-626d4e0/tags.csv) · [היסטוריית נקודות השמירה](docs/repository-audit/2026-09-22-626d4e0/commits.csv).

<a id="coverage"></a>

## כיסוי GitHub ופערי גישה

API הוא ממשק לקריאת נתוני שירות. בקשות הרשימות נאספו עם עימוד — המשך לעמודים הבאים עד לסיום. 403 מציין חסימת הרשאה או מוצר; 404 יכול לציין משאב שאינו קיים או שאינו נחשף. שניהם אינם שקולים ל״אין בעיות״.

| תחום | תוצאה | היקף/הסתייגות |
| --- | --- | --- |
| משימות (Issues) | נבדק וריק | 0 |
| בקשות שינוי (PR) | נבדק וריק | 0 |
| תגובות למשימות | נבדק וריק | 0 |
| תגובות סקירת קוד | נבדק וריק | 0 |
| הפצות להורדה | נבדק וריק | 0 |
| תהליכים הרשומים ב־Actions | נבדק וריק | 0 |
| ריצות תהליכים | נבדק וריק | 0 |
| תוצרי ריצות | נבדק וריק | 0 |
| מטמונים — נתונים זמניים להאצת בנייה | נבדק וריק | 0 |
| סביבות הפעלה | נבדק וריק | 0 |
| פריסות | נבדק וריק | 0 |
| כללי הגנה | נבדק וריק | 0 |
| חיבורים לאירועים חיצוניים | נבדק וריק | 0 |
| מפתחות פריסה — מטא־נתונים בלבד | נבדק וריק | 0 |
| משתמשים בעלי גישה | נבדק ומכיל פריטים | 1 |
| צוותים | נבדק וריק | 0 |
| התראות תלויות | לא נגיש: HTTP 403 | לא ידוע |
| התראות סריקת קוד | לא נגיש: HTTP 404 | לא ידוע |
| התראות סודות | נבדק וריק | 0 |
| רשומות סודות Actions — ללא ערכים | נבדק וריק | 0 |
| משתני Actions — ללא ערכים | נבדק וריק | 0 |
| רשומות סודות עדכון תלויות — ללא ערכים | נבדק וריק | 0 |
| Pages — אתר מהמאגר | כבוי לפי has_pages=false; API החזיר 404 | אין מסקנה על האתר במיזם המקורי |
| Wiki — מאגר דפי ידע | מופעל בהגדרות, אך כתובת Git נפרדת החזירה Repository not found | התוכן לא נגיש או לא נוצר; אין הכרעה |
| Discussions — דיונים | כבוי לפי has_discussions=false | לא נאספו דיונים |
| Projects — לוחות משימות | לא נגיש; חסרה הרשאת read:project | לא שונו הרשאות |
| Packages — חבילות הפצה | רשימת NuGet לא נגישה; חסרה read:packages | בדף המאגר לא מוצגות חבילות, אך אין אימות מלא |
| הגנת ענפים | 33 שדות protected=false; רשימת rulesets ריקה | 404 נפרד בכל קריאת protection |
| הרשאות תהליכים | Actions מופעל; ברירת מחדל read; אישור PR אוטומטי כבוי | allowed_actions=all; sha_pinning_required=false |

בנוסף נקראו 10 תוויות ו־0 אבני דרך. [ראיות משלימות ופערי גישה](docs/repository-audit/2026-09-22-626d4e0/additional-evidence.json).

## מה נבדק ומה לא

**[עובדה]** נוצר mirror — עותק Git מלא ללא פתיחת סביבת עבודה להרצה. נקראו כל אובייקטי התוכן הנגישים מההפניות שנאספו, חושבו חתימות, נבנו עצי קבצים לענפים ולתגים ומיפוי נתיבים היסטוריים. הסורק חילץ טקסט, כותרות, פעולות וייבואי Python כאשר ניתן לפענח אותם. בדיקת `git fsck --full --no-reflogs` הסתיימה בקוד 0 וללא הודעות. בדיקה זו מאמתת את מבנה אובייקטי Git, לא את איכות התוכנה.

| קטגוריית תוכן היסטורי | תכנים ייחודיים |
| --- | --- |
| text | 28016 |
| binary | 17402 |
| not_decoded_size_limit | 2 |

סריקת הדפוסים המצומצמת סימנה 1 תכנים לבדיקה אפשרית של סוד; הפריט שסומן נמצא בנתיב ההיסטורי `misclib/cherrypy/test/test.pem`, בתוך בדיקות של ספריית צד שלישי. לא הודפס תוכן המפתח ולא נבדקה תקפותו מול שירות; אין בכך הוכחה לסוד פעיל שלך או להעדר סודות אחרים. נמצאו 0 מצביעי LFS — קישורים לקבצים גדולים הנשמרים מחוץ לאובייקטי Git הרגילים. הפניות כאלה, אם קיימות, אינן נחשבות קריאת הקובץ החיצוני.

**[לא נבדק]** לא הופעלו קוד המאגר, Revit, מתקינים, בדיקות תוכנה, חיבורים לשרתים או הרחבות. לא בוצע אימות חתימות DLL, התאמת בינרי למקור, פירוש כל תמונה, פתיחת מודלי Revit או קריאת כל שמונת המאגרים החיצוניים. לא בוצעה סקירה סמנטית ידנית של כל גרסה היסטורית. היסטוריה שנמחקה ואינה נגישה מהפניות השרת אינה ניתנת להוכחה כאן.

נתוני גודל וכותרת או חתימה דיגיטלית של תוכן אינם תחליף להבנתו. טבלת ברירת המחדל מסווגת תפקידים לפי מיקום וסוג ומצרפת סמלים שחולצו; שיוך מלא של ״מי קורא למי״ בזמן הרצה לא חושב. קבצי Python 2 שאינם ניתנים לפענוח באמצעות מנתח Python 3 אינם נחשבים לקוד פגום מסיבה זו בלבד.

הכספת המשותפת לא הייתה נגישה. ההסקות מבוססות על Git ו־GitHub. שכבת Jev הופעלה על תקציר מותר של תכנון הבדיקה וגם על תקציר התוצאות והפערים, במצב ייעוץ בלבד. היא לא קראה את הקוד המלא, לא העניקה הרשאה ואינה תחליף לאימות הממצאים.

## המשך מומלץ לפי תועלת

- להכריע איזו גרסה משרתת את עבודת ההנדסה הנדרשת. הבחירה מונעת השקעת זמן בתיקונים שכבר קיימים בענף אחר.

- לפני שימוש בהרחבות פרטיות, לטפל בשמירת אסימונים ובמעבר בטוח של הגדרות קיימות.

- להפריד התאמות אישיות מעותק ההתקנה ולבדוק שחזור לפני עדכון.

- להקים בדיקה על כל עדכון PR ועל גרסת הקוד האחרונה, ולא רק על פתיחת הבקשה.

- לבחור שלושה תרחישים הנדסיים ממשיים ולהשוות תוצאות על מודלי ניסוי; להוסיף שילובי Revit ומנועי Python הנדרשים לעבודה.

- להשלים פענוח בינריים, בדיקת המאגרים החיצוניים ומשטחי GitHub החסומים רק אם הם נדרשים לרמת האמון המבוקשת.

המלצות אלה לא בוצעו כפעולות שינוי במסגרת הביקורת.

## מילון וקובצי ראיות

| מושג | פירוש |
| --- | --- |
| Git | מערכת לשמירת גרסאות והיסטוריית שינויים |
| SHA / hash | מזהה מחושב של תוכן; מאפשר לקשר לגרסה קבועה |
| mirror | עותק של ההפניות וההיסטוריה הנגישות בשרת, בלי checkout |
| checkout | פריסת גרסה לקבצי עבודה; לא נעשתה לצורך הרצת המקור |
| DLL | ספריית קוד בינרית ש־Windows או .NET טוענים |
| API | ממשק שתוכנה חושפת לקריאת מידע או להפעלת פעולות |
| dependency | רכיב חיצוני שהתוכנה מסתמכת עליו |
| artifact | קובץ תוצר של ריצת בנייה או בדיקה |
| static analysis | בדיקת קוד בלי להפעילו |
| CSV / JSON | פורמטים לטבלאות ולנתונים; מיועדים לבדיקה ושחזור המלאי |
| gzip / ZIP | דחיסת קבצים; הקבצים הדחוסים בנספחים מכילים נתונים, לא מתקין |

[צילום מצב Git מלא, דחוס](https://github.com/ofer2300/pyRevit/releases/download/repository-audit-2026-09-22/snapshot.json.gz) · [תוצאות GitHub מסוננות](docs/repository-audit/2026-09-22-626d4e0/repository-surfaces.json) · [הפניות למאגרים חיצוניים](docs/repository-audit/2026-09-22-626d4e0/submodules.csv) · [ייבואים סטטיים בברירת המחדל](docs/repository-audit/2026-09-22-626d4e0/dependencies-static.json) · [בדיקת מטא־נתונים בינריים](docs/repository-audit/2026-09-22-626d4e0/binary-inspection.json) · [חתימות קובצי התוצר](docs/repository-audit/2026-09-22-626d4e0/manifest-sha256.json).

הדוח מתייחס לצילום מצב מסוים. התוצרים עצמם נוצרו לאחר הצילום ואינם חלק מהמאגר שנבדק. מצב המסירה בעת צילום הביקורת: מקומי בלבד; הפרסום הנוכחי מתועד בראש הדוח. כיסוי: חלקי ומפורט, ללא טענת ״הכול תקין״.

</div>
