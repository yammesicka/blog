---
title: נמצאה התנגשות באלגוריתם הגיבוב SHA-1
author: ים מסיקה
date: 2017-02-24T22:12:00+00:00
categories:
  - חדשות טכנולוגיות
tags:
  - collision
  - attack
  - hash
  - SHA-1
  - אלגוריתמי
  - גיבוב
  - גיבוב
  - הסבר
  - חדשות
  - חולשות
  - מה
  - זה
direction: rtl
---
אז שוב הגיע היום הזה בחודש בו כל החברים החנונים שלכם מפרסמים פוסט בשפה שלהם, הפעם משהו על "קוליז'ן בשה וואן", שנשמע כמו מנה מתפריט תיירים של מסעדה יפנית. מה זה אומר בכלל, ולמה זה כזה חשוב?

נסו להיזכר במספר תעודת הזהות שלכם. זוכרים שיש שם איזושהי "ספרת ביקורת"? אי פעם תהיתם מה זה אומר? Breaking news, למי שחי בחלל: זו ספרה שנמצאת שם רק כדי לוודא שכל שאר הספרות הוזנו נכון. אם תשנו ספרה אחת בתעודת הזהות, גם ספרת הביקורת תשתנה. אפילו אפשר לחשב אותה די בקלות[^1]!

יום שישי בבוקר, השעה 11:00 לפנות בוקר ואתם עוד לפני כוס התה שלכם. אתם שומעים נקישות על דלת ביתכם, ומגלים על מפתן הדלת רוכל זקן שמעוניין למכור לכם ציורים. לא הספקתם להגיד לו שאתם לא מעוניינים, וכבר הוא מספיק לשלוף מידיו את... המונה ליזה בכבודה ובעצמה!

אמנם אתם רואים בבירור שמדובר בלוח עץ צפצפה, ושמדובר בצבעי שמן ולא באיזה הדפס זול. אתם מתקשרים ללובר, כמובן (פעמיים! קצת קשה לתפוס אותם, הם בהיסטריה שנגנב להם איזשהו ציור, פף), ומבקשים מהם לעזור לכם לוודא שמדובר בדבר האמיתי. איך אפשר לוודא דבר שכזה?

וכאן אנחנו מגיעים לרעיון של "גיבוב", שבתכל'ס עלול להיות לפעמים קצת יותר מורכב מספרת הביקורת בתעודת הזהות: יש לנו המון המון מידע, ואנחנו רוצים לוודא שהוא "הדבר האמיתי". שימוש בעולם האמיתי, מחוץ לעולם גנבת התמונות, הוא נניח לוודא ששליחת קובץ כלשהו התבצעה בהצלחה, בלי שקרה לאותו קובץ משהו בדרך (נניח: מישהו ערך אותו בצורה זדונית).

איך זה עובד? אלגוריתמי גיבוב הם דבר מעניין. הם מקבלים קלט מסוים (נניח, קובץ), ומחזירים מעין "מחרוזת מייצגת" עבור אותו קלט (דוגמה לאחת כזו: `f39eb18687c261277ffac55422ea493448e8a801`), שמשמשת הרבה פעמים ל"וידוא", ממש כמו ספרת הביקורת בתעודת הזהות. לאלגוריתמי גיבוב יש כמה "תכונות" משותפות:

  * **דחיסה** – אנחנו רוצים שהפלט שיחזור יהיה קצר יחסית. לפעמים נרצה שהוא יהיה בגודל קבוע.
  * "**החלטיות**" (דטרמיניזם) – גם אם נריץ מיליארד פעם את אלגוריתם הגיבוב שלנו על הקובץ, אנחנו מצפים לקבל כל פעם את אותה מחרוזת שמייצגת אותו.
  * **התפלגות** – נניח ש"המחרוזת המייצגת" שלי היא בת 3 ספרות, משמע: בהינתן קובץ, הוא ייוצג על ידי מספר בין 0 ל־999. העיקרון השלישי אומר שאני מעוניין שבהינתן 1,000 קבצים, אני רוצה שיהיה כמה שיותר סיכוי שלכל אחד מהם יוקצה מספר אחר, שונה, בין 0 ל־999.
  * (לפעמים) **חסינות מיצירת התנגשויות**: או, כאן אנחנו מתחילים לדבר על דברים מעניינים. העיקרון הזה הוא מעין "שכלול" של רעיון ההתפלגות. הוא אומר שגם אם ממש בא לי, בהינתן "מחרוזת מייצגת", יהיה לי קשה מאוד ליצור קובץ (את המקורי או אחר) שיחזיר לי את אותה מחרוזת מייצגת. זאת אומרת: התהליך הוא חד כיווני. אני יכול לקבל "מחרוזת מייצגת" בקלות, אבל לעשות את התהליך ההפוך (לייצר קובץ, נניח, ממחרוזת מייצגת) יהיה לי קשה מאוד.

למה העיקרון הרביעי חשוב? נניח שהגנב של מונה ליזה מתוחכם במיוחד, והיה יודע אילו סימנים הלובר מנחים אתכם לבדוק כדי לוודא שהציור אמיתי. אם אותם סימנים היו קלים לחיקוי, מה היה עושה אותו גנב? כמו שאמרו הגשש, "פרצה קוראת לגנב, וגנב, כשקוראים לו, בא".

אותו דבר עם אלגוריתמי גיבוב: נניח שחבר מתנדב לצרוב לנו את המשחק היוקרתי DragonBallZ.exe, ואנחנו רוצים לוודא שהצריבה עברה בלי הפתעות (נניח: מישהו "התעסק" עם המשחק והוסיף לו קוד מדביק בתהליך הצריבה). אנחנו מפעילים את אלגוריתם הגיבוב שיביא לנו "מחרוזת מייצגת" של קובץ המשחק על המחשב של החבר, לפני הצריבה. אם אותו אלגוריתם גיבוב יחזיר את אותה "מחרוזת מייצגת" עבור אותו קובץ אחרי הצריבה, נדע בוודאות שאף אחד לא "טיפל" במשחק בדרך. אם הבחור הלא חביב שרוצה להכניס לנו לאותו משחק קוד מדביק ידע באיזה אלגוריתם גיבוב אנחנו משתמשים, הוא יוכל, לכאורה, אחרי שהוא מוסיף את הקוד המדביק למשחק, לנסות לגרום לקובץ החדש לייצר את אותה "מחרוזת מייצגת", וכך אי־אפשר יהיה לדעת שהוא שינה את הקוד המקורי של המשחק.

וכאן נכנס לפעולה העיקרון הרביעי. נזכיר: בהינתן אותה מחרוזת מייצגת, ממש, ממש, ממש!@ קשה לייצר קובץ שייצור אותה. מגניב, לא? כזה מגניב שאפילו נשקיע וניתן לזה שם: כשהעיקרון הרביעי ממומש בפונקציית הגיבוב, אלגוריתם הגיבוב נקרא "אלגוריתם גיבוב קריפטוגרפי".

יש מספר רב של שימושים מוכרים ופופולריים עבור אותם אלגוריתמי גיבוב קריפטוגרפיים. נבחן 2 מהם:

  1. כמו שכבר ציינו, אנחנו רוצים לוודא שדברים (קבצים, הודעות) הגיעו ממקום אחד למקום שני בלי שאף אחד "טיפל" בהם.
  2. הגנה על סיסמאות[^2]\: נדמיין שאתם רוצים להירשם לאתר מסוים ולהתחבר אליו בעתיד. זה יהיה מאוד לא אחראי מצד האתר לשמור את הסיסמה שלכם כמו שהיא, מכיוון שיש סיכוי סביר שאתם משתמשים באותה סיסמה לכמה אתרים, ואז אם יפרצו לאתר וישיגו את מסד־הנתונים שלו אתם בצרות. מה שרוב האתרים עושים זה לבצע אלגוריתם גיבוב קריפטוגרפי על הסיסמה שלכם בעת ההרשמה, ולהכניס את התוצאה למסד־הנתונים. כשתנסו להתחבר, האתר יבצע גיבוב על הסיסמה שהזנתם בטופס ההתחברות וישווה אותה עם תוצאת הגיבוב הקודמת שקיימת במסד־הנתונים.

אז בהינתן שאתם לא עומדים לגנוב את המונה־ליזה בקרוב, למה זה טוב, כל השטויות האלו, ומה הקשר למנה היפנית ההיא שהוזכרה בתחילת הפוסט?

ראשית נבין מה היא "מתקפת התנגשות" (Collision Attack). עכשיו כשיש לנו את כל הידע הזה, זה די פשוט להבין: זה אומר שמצאתי דרך ש"תעזור" לי, בהינתן "מחרוזת מייצגת" מסוימת, לרמות את המערכת. משמע: לייצר \*בקלות\* משהו מסוים, שאם אתן אותו לאותו אלגוריתם גיבוב קריפטוגרפי הוא יחזיר לי את אותה מחרוזת מייצגת.

האלגוריתם "SHA-1" (או Secure Hash Algorithm 1) הוא אלגוריתם גיבוב קריפטוגרפי שהומצא בשנת 1993. הוא היה בשימוש שנים רבות במגוון משוגע של מערכות והייתה בו תועלת רבה, שכן אם היינו רוצים ליצור התנגשות שכזו היינו צריכים כוח חישובי משוגע, שמקביל לכוח החישוב של 12 מיליון כרטיסי מסך במשך שנה שלמה(!). למרבה הצער, כבר בשנת 2005 חוקרים הכריזו שהם מצאו "מתקפת התנגשות" שמצמצמת מעט את האפקטיביות של האלגוריתם. לאט לאט התחום התפתח, ובשנת 2014 ארגונים שונים הכריזו[^4] שהם לא יעבדו יותר עם SHA-1 לצורכי אבטחה.

והנה השורה התחתונה: אתמול, גוגל הכריזו[^3] על גילוי די משוגע – מצאנו מתקפת התנגשות מושלמת על SHA-1. מה זה אומר? זוכרים את ה־12 מיליון כרטיסי מסך במשך שנה? ובכן, בעזרת השיטה החדשה גוגל הפחיתה את המספר הזה ל... 110.

ואלו היו 10 דקות על קוליז'נים ופונקציות גיבוב. למי שרוצה לקרוא עוד (משוגעים, לכו לעשות דברים כייפים), כתבתי[^5] על זה בעבר ב־DigitalWhisper. נתראה ברשומה הבאה ;) 

 [^1]: [קריאה נוספת בנושא](https://he.wikipedia.org/wiki/%D7%A1%D7%A4%D7%A8%D7%AA_%D7%91%D7%99%D7%A7%D7%95%D7%A8%D7%AA) מוקדשת לכל מי שהיה בקורסים שלי ורוצה לשחזר חוויות.
 [^2]: כתבתי בעבר על [פונקציות גיבוב בהקשרי הגנה על סיסמאות](https://www.facebook.com/Yam.Mesicka/posts/10154008891258297)
 [^3]: גוגל הכריזו על [מציאת collision](https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html)
 [^4]: יש לכולם סיבות די טובות [לרצות להרוג את SHA-1](https://konklone.com/post/why-google-is-hurrying-the-web-to-kill-sha-1)
 [^5]: ב־[DigitalWhisper](https://www.digitalwhisper.co.il/files/Zines/0x42/DigitalWhisper66.pdf) תחת "שה־1 תמים"