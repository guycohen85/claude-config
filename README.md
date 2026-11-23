# claude-config

ההגדרות וה־skills שלי ל־[Claude Code](https://claude.com/claude-code).
אתם מוזמנים לקחת מה שמתאים לכם.

## ה־skills

### `client-website-workflow`
תהליך עבודה מסודר לבניית אתר ללקוח (או לחבר), מהבריף ועד המסירה. Claude כותב את הקוד, ואתם מחליטים ומאשרים בכל שלב.

- **שלבים עם אישור:** קליטת הבריף, שאלות ללקוח, בחירת טכנולוגיה, היקף ומחיר, עיצוב ותוכנית, בנייה באבני דרך, QA ומסירה.
- **אין קוד לפני אישור:** Claude לא כותב קוד לפני שאישרתם את התוכנית.
- **מעקב:** כל החלטה נרשמת בקובץ `decisions.md` בפרויקט. בכל שיחה חדשה Claude קורא אותו וממשיך מאיפה שהפסקתם.
- **מסלול מהיר כברירת מחדל,** שמעדיף את הפתרון הקל ביותר לתחזוקה: אתר סטטי לפני שרת, ושירות מוכן לפני פיתוח.
- **מסמכים ללקוח בעברית:** שאלון (גם בגרסה קצרה לוואטסאפ), היקף עבודה, רשימת תוכן ומסמך מסירה.
- **רשימות בדיקה:** RTL, נגישות לפי ת"י 5568, פרטיות, SEO.

הפעלה: `/client-website-workflow`, או פשוט לכתוב ל־Claude שיש לכם בריף לאתר.

### `website-assets`
החלטות על הנכסים הוויזואליים של אתר: לוגו, תמונות, אייקונים, פונטים בעברית, favicon ותמונת שיתוף (Open Graph).
לכל סוג נכס Claude מציג אפשרויות עם עלות ורישיון, וממליץ על אחת. אחרי שבחרתם, הוא משיג או יוצר את הנכסים ומתעד את המקור והרישיון של כל קובץ.

אפשר להשתמש בו לבד, ו־`client-website-workflow` מפעיל אותו בשלב העיצוב.

## התקנה

מעתיקים את התיקייה של ה־skill שרוצים אל `~/.claude/skills/`:

```bash
git clone https://github.com/guycohen85/claude-config.git /tmp/claude-config
mkdir -p ~/.claude/skills
cp -r /tmp/claude-config/skills/client-website-workflow ~/.claude/skills/
cp -r /tmp/claude-config/skills/website-assets ~/.claude/skills/
```

ב־Windows (בלי WSL) התיקייה היא `%USERPROFILE%\.claude\skills\`.

אחרי ההעתקה פותחים שיחה חדשה ב־Claude Code. ה־skills יופיעו ב־`/skills`.

> אל תעשו clone לכל ה־repo ישירות לתוך `~/.claude`. זה עלול לדרוס את ההגדרות שלכם (`settings.json`).

### התאמה אישית
ה־skills הם קבצי Markdown רגילים (`SKILL.md`), ואפשר לערוך אותם בחופשיות. כמה דברים שכדאי לבדוק:
- **שפה:** ה־skills מנחים את Claude לדבר איתכם בעברית ולכתוב קוד ו־`CLAUDE.md` באנגלית.
- **תיקיית הלקוחות:** ברירת המחדל היא `~/clients/`. Claude ישאל בפעם הראשונה.
- **טכנולוגיות:** הכלים והספריות שמוזכרים ב־skills מתיישנים. לכן ה־skills מנחים את Claude לבדוק ברשת מה עדכני לפני שהוא ממליץ.

### skills משלימים (לא חובה)
`client-website-workflow` משתמש בהם אם הם מותקנים, וממשיך בלעדיהם אם לא:
- `frontend-design`: עיצוב ה־mockup וה־UI. מתקינים עם `/plugin install frontend-design@claude-plugins-official`.
- `code-review`: בדיקת קוד לפני כל אבן דרך. מובנה ב־Claude Code.
- `webapp-testing`: בדיקות RTL, מובייל וטפסים בדפדפן. נמצא ב־[anthropics/skills](https://github.com/anthropics/skills).

## איך ה־repo בנוי

ה־repo הזה הוא התיקייה `~/.claude` שלי. ה־`.gitignore` עובד כרשימה לבנה: הוא מתעלם מהכל, ומכניס רק קבצים שנכתבו ביד. ככה טוקנים, היסטוריית שיחות ו־cache לא נכנסים ל־git בטעות.
אם תרצו לנהל ככה גם את ה־`~/.claude` שלכם, אפשר להעתיק את ה־[`.gitignore`](.gitignore).
