---
title: על חולשת KRACK
author: ים מסיקה
date: 2017-10-18T21:34:00+00:00
categories:
  - קריאה ארוכה
tags:
  - HTTPS
  - SSL
  - TLS
  - WEP
  - Wifi
  - WPA2
  - אבטחת מידע
  - תקשורת מאובטחת
---

בשנים האחרונות המודעות לעולם אבטחת המידע עולה באופן תדיר. העיסוק בתחום מתרחב, במיוחד לאור הטכנולוגיה שמתפתחת בקצב פסיכי וזולגת כמעט לכול חלקה שהכרנו עד היום – נוצר צורך ממשי באנשים שיבדקו את אמינות החידושים שיוצאים, אותם חידושים שלעיתים ינהלו אספקטים חשובים בחיינו (רכב אוטונומי) או ישמרו על החומרים הרגישים ביותר שלנו. האנשים שיבדקו את האמינות של אותם מוצרים מוגדרים כקבוצה מאוד גדולה, תחת השם "אנשי אבטחת מידע", והם עוסקים בעשרות תתי־תחומים מרתקים שאולי עוד יצא לי לסקור פה בעתיד.

בואו נדמיין שאתם חוזרים הביתה אחרי יום עמוס בעבודה, ומתים לדעת מה השתנה באתר החביב עליכם, "וואלה! כיף" (או בקיצור: "וואלאכ"). המחשב כבר דלוק, ואתם נכנסים לאינטרנט דרך ה־WiFi בבית, מתחברים לחשבון ומשחקים במשחק ההו־כה־ממכר "פלאפי בלתי אפשרי". כעבור 5 דק' אתם מקבלים הודעה להוטמייל שהחשבון שלכם בוואלאכ נפרץ וכל ה־Highscore שלכם נמחק.

הו לא.

ננסה לנתח מה קרה, ומה מנע מזה לקרות עד היום. כאמור, יש כמות עצומה של טכנולוגיות שם בחוץ, ביניהן כמה מגניבות שמספקות לכם שכבת הגנה מאנשים רעים שפורצים חשבונות רנדומליים. אני אנסה להתמקד בעיקר, ובפוסט הזה ניגע בקצה המזלג של 2 מהן.

&#8212;&#8211;

ברשותכם, אתפלצן לשנייה. אינטרנט אלחוטי זה דבר מדהים ומשוגע בעיניי, עם כמה שזה נראה לנו טריוויאלי היום. הרעיון הזה שאנחנו כבני אדם מנצלים את הידע שלנו כדי להעביר מידע מגה מורכב בכמויות משוגעות דרך גלי רדיו, זה לגמרי מסוג הקסמים האלו שאתם משתמשים בו ביום־יום ומדי פעם עוצרים לחשוב כמה מגניב העולם שיש לו פיצ'רים כאלו.[4]

לצד התמונה הפסטורלית והמחשבות הפילוסופיות בשקל הללו, דמיינו לרגע את המחשב שלכם כערס תימהוני שיכור וכעוס שצועק לכל עבר&nbsp; "איצקו, תתחבר לי ל־Facebook!!! השם משתמש שלי זה BarburMehatabur והסיסמה שלי זה Malgeza123".

זה בערך מה שקורה בתקשורת אלחוטית סטנדרטית, כשאתם מתחברים לרשת אלחוטית פתוחה (החינמיות האלו, אלו שמוגדרות כ־Open כשאתם בוחרים לאיזו רשת להתחבר באמצע הביס להמבורגר במקדונלד'ס). המחשב שלכם מפיץ את המידע לכל מי שרק מוכן לשמוע, ואם מישהו רק ינסה להאזין, הוא יוכל לקבל את ההודעה שהפצתם לכל עבר. בהחלט לא סימפטי, ולא מה שהתכוונתם לעשות, כנראה.

מהסיבות הללו בדיוק פותחו במהלך השנים דרכים שונות ליצור סביבה בטוחה לתקשורת עם המקור שמפיץ את הרשת האלחוטית. יתכן שכבר שמעתם קללות כמו WEP, שהייתה לאחת משיטות האבטחה הראשונות להצפין תקשורת אלחוטית, WPA שהחליפה אותה לזמן קצר כשהתגלו ב־WEP פרצות משמעותיות או WPA2 שנמצאת איתנו (עם שינויים קלים) מאז 2004. הבעיה בכל השיטות האלו – האנשים הרעים מחפשים דרכים לעקוף אותן, ולהשיג את המידע שלכם בכל זאת<img loading="lazy" alt="🙂" src="https://lh3.googleusercontent.com/5QXa4VyESIXovPFpmM7xQALK10jK9NJ2EW-cyaQ9-dwCVXQpzOG2IbzoxWexnHkX5BW53m7RjiiA9m1ui4nLkvi7mVfAJS4vo4QGrDg5j4Fo-JLOVe7toTqWieX_PO74T_1ScqzJ" width="16" height="16" /> 

כדי לשבור קצת את המתח, אגיד שאם אתם מתחברים לרשת WEP, כבר היום יש דרך פשוטה ממש למצוא את הסיסמה ולהאזין לכל התעבורה שלכם. ב־WPA2, לעומת זאת, יוכלו להאזין לתעבורה שלכם רק אם יש לתוקף את הסיסמה של הרשת האלחוטית שלכם[5].

&#8212;&#8211;

נשים בצד את מה שלמדנו עד עכשיו, ונעבור לגזרה אחרת לרגע. בואו נדבר על כניסה "מאובטחת" לאתרים, או בשם הכאילו־מפחיד־וטכני: TLS/SSL.

נדמיין שאתם מנסים לשלוח מכתב אובר־מסווג מכם לחברה שלכם שגרה ביפן. אתם כותבים את המכתב, סוגרים את העטיפה, מדביקים בול ומשלשלים לתיבת הדואר. יש לכם מזל שמדובר בדואר ישראל וכנראה זה לעולם לא יגיע לשום מקום! אחרת אתם יודעים כמה ידיים היו יכולות לקרוא את המכתב המסווג בדרך?! משיגינע. ממייני מכתבים מרחבי הגלובוס, מפיצים, מחלקים&#8230;

נדמיין שאתם מנסים להתחבר לחשבון ה־Facebook שלכם מהעבודה. מאחורי הקלעים, אחרי שלחצתם על כפתור "התחבר", המחשב שלכם ינסח הודעה שמיועדת לשרתי Facebook עם כל פרטי ההתחברות שלכם (כולל סיסמה), בתקן כלשהו שהשרתים שלהם מסוגלים להבין, וישלח אותה אליהם. תחשבו כמה ידיים ההודעה הזו, שמיועדת ל־Facebook במקור, הולכת לעבור: המחשב שלכם יעביר אותה לנתב (ראוטר) המקומי של המשרד, שבתורו יעביר אותה לספקית שלכם, ורק אז הספקית שלכם תעביר אותה לשרתים של Facebook‏[1].

יש מצב שכבר הצלחתם לזהות למה אני לא מרוצה: כל אחד בשרשרת הזו יכול להציץ בהודעה, ולראות להנאתו את הסיסמה המביכה שלי ל־Facebook! אם מישהו יפרוץ לנתב ויראה את ההודעה? יותר גרוע – אם גורם כזה או אחר יפרוץ לספקית שלי, הוא יוכל לראות את סיסמאות כל הלקוחות של אותה ספקית?

הפתרון, שהוחל לראשונה בדפדפנים על־ידי Netscape בשנת 1994, נקרא פרוטוקול HTTPS. מדובר סך הכל על המנעול החביב שמופיע לכם לעתים בשורת הכתובות, ליד אתרים שהכתובת אליהם מתחילה ב־https. הוא מציין שהתקשורת שלכם לאתר בטוחה, ברוב המקרים (אם תציצו עכשיו בשורת הכתובות, תגלו מנעול ירוק שכזה. אני מקווה), ולמעשה נותן לכם בקצרה הבטחה די מדהימה: אתם והאתר שאתם מדברים איתו הגדרתם ביניכם "מילת קוד" שאף אחד לא יודע, ובעזרתה אתם מצפינים את ההודעות ביניכם. אף אחד חוץ ממך ומשרתי אתר היעד לא יכול לקרוא אותן, גם אם הן עברו דרכו.

הפרטים של הרעיון סבוכים לאללה, אבל הוא כל־כך גאוני שאני עוד אכתוב על זה יום אחד פוסט מפורט[2]. עד אז, פשוט תאמינו לי שיש דרך מופלאה, ששולי דף זה צרים מלהכילה, לדבר בצורה מוצפנת עם אתרים ברחבי האינטרנט, באופן שגם מאמת את זהותם וגם מאפשר לכם לתקשר בצורה שאף אחד אחר לא יוכל להאזין בדרך. זו בחירה של האתרים האם להשתמש או לא להשתמש ב־HTTPS, ולמרבה הצער חלק גדול מהאתרים הישראליים מיושנים ומעאפנים ובוחרים שלא להשתמש ב־HTTPS, בגלל בורות טכנולוגית, עצלנות או ניסיון לחסוך מעט כסף.

ראיתי את המבט הסקרן הזה. אז לא, "וואלה! כיף" לא משתמש ב־HTTPS כברירת מחדל.

&#8212;&#8211;

סיכום ביניים! (אומג אתם עדיין פה)

נניח שאתם אנשים חביבים ויחידי סגולה שהצליחו לשרוד עד פסקה שכוחת אל זו. הייתם רציניים, עשיתם שיעורי בית, התקנתם את התוסף HTTPS Everywhere‏[6] בדפדפן שלכם ואתם גולשים רק ברשתות WPA2. האם אפשר לקפל את קפוצ'ון הסייבר וללכת הביתה?

לא, כמובן שלא. לכל הגנה שצוינה פה יש פרצות, שלחלק גדול מהן יש תיקונים, וזה יכול להמשך לנצח. אחרי כל זה, זה בדיוק הזמן לדבר על מה שכולנו התכנסנו כאן בשבילו: KRACK! (לא כזה, אמא, לא לדאוג)

&#8212;&#8211;

KRACK היא חולשה שנמצאה אי שם בשלהי 2016, אבל פורסמה וגררה הדים רק לפני ימים אחדים, באוקטובר 2017. זו פרצה די פסיכית שמאפשרת לאנשים לראות ולהתערב בתעבורה בכל רשת WPA2 שהיא – משמע: לקרוא את הנתונים ששלחתם, סיסמאות, פרטי חיוב וסתם כל דבר שגלשתם אליו, או לשלוח דברים בשמכם. הפסקאות הקרובות תכלולנה מעט פרטים טכניים. מי שיש לו חשק בלתי נשלט להבין את הפירצה מוזמן, ומי שלא – תודה שקראתם, היה כיף<img loading="lazy" alt="🙂" src="https://lh6.googleusercontent.com/rEpXuaSfTqo8WRXxaHcqqwJ4LIpkEYA96HXqGqaVWTZOZ6lKRjwhYJ9am-wbkPV-RmcOLRNrEkIPz-QQHP8FDVggHuyx8fkrFEL9309L5_qQ8rm0CndbmCrgWQoAHPgcRl1A_2i8" width="16" height="16" /> 

החלק הראשון שאתם צריכים להכיר כדי להבין את המתקפה, הוא עצם הרעיון שאנחנו מצפינים כל הודעה לפני שהיא יוצאת מכם לכיוון מקור הרשת האלחוטית (נניח מעכשיו שמדובר בנתב). במקרה שלנו, יש חוק אחד חשוב: אסור להשתמש באותו מפתח עבור הצפנת מספר הודעות שונות, שכן זה מאפשר לקורא צד שלישי לגלות את המפתח.[7]

החלק השני נקרא "Four way handshake". כשלקוח כלשהו מנסה להצטרף לרשת אלחוטית עם WPA2 פעיל, הוא מבצע שיחה בת ארבעה שלבים עם הנתב, שמטרתה לבדוק שהסיסמה שהוא הזין נכונה, ולבחור מפתח להצפנת השיחה בין הלקוח לנתב. השיחה הולכת (בערך) ככה:

  1. "היוש אני נתב. בוא נסכים על מפתח להצפין איתו. קח מידע אקראי שהגרלתי ואמור לעזור לך לחולל מפתח."
  2. "היוש אני לקוח. תודה לך אדון נתב, הנה מידע אקראי שהגרלתי ואמור לעזור לך לחולל מפתח."
  3. הצדדים עכשיו מערבבים את 2 חלקי המידע יחד עם סיסמת ההתחברות לנתב, ויוצרים מזה מפתח חד פעמי.
  4. "כנתב, אני מאשר שהסכמנו על מספיק מידע כדי ליצור את המפתח הסודי עבור שיחה בינינו."
  5. "תודה שפיץ. יאללה, נדבר."

ההודעה השלישית (שכתוב לידה 4) מגיעה מהנתב ללקוח, גורמת לו לשמור אצלו את המפתח, ומאותו רגע אותו מפתח ישמש להצפנת הודעות בינו ובין הנתב. הטריק המגניב הוא שהאנשים הרעים שמנסים לתקוף את החיבור שלנו, יכולים לאסוף את ההודעה השלישית ולשלוח אותה שוב ושוב ללקוח, מה שיגרום לו לאתחל את המפתח שלו כל פעם \*לאותו מפתח\*. אבל רגע! אסור להצפין כמה הודעות עם אותו מפתח!&#8230;

&#8212;&#8212;

אז מה, אכלנו אותה? אפשר לקרוא את כל מה שאנחנו מעבירים בכל WiFi בעולם?! האנושות אבודה לנצח?!?!

למזלנו הגדול לא, מכמה סיבות. הסיבה הראשונה היא שאתם ככל הנראה מוגנים: המתקפה מתבצעת על מחשב קצה ולא על נקודות הגישה שלכם (נניח: על מערכת ההפעלה שלכם, ולא על הנתב שלכם), ועבורם כבר ככל הנראה יצא עדכון שתוכלו להתקין ממש בקלות, [אם הוא לא מותקן אוטומטית][1]. הסיבה השנייה היא זו שלמדנו עליה ממש כמה פסקאות למעלה – גלישה באתר ב־HTTPS מאפשרת לכם גלישה נוחה, ותקשורת שמוצפנת קצה לקצה ביניכם לבין אתר היעד&#8230; לרוב.

אם מצאתם טעויות ואי־דיוקים, אשמח לשמוע.

&#8212;&#8212;-

בונוס למתעניינים: גם HTTPS לאו־דווקא בטוח. יש אתרים שמשתמשים ב־HTTPS, וכשמשתמש נכנס בעזרת HTTP הם מנסים להפנות אותו לתקשורת באמצעות HTTPS. הבעיה מתחילה כשתוקף מצליח להתערב בתקשורת שבין המשתמש לאתר, ורואה שהמשתמש נכנס ב־HTTP. במקרה הזה הנתקף יחשוב שהוא מדבר עם האתר, בזמן שהוא בפועל מדבר עם התוקף שמתחזה לאתר. התוקף יגיד למשתמש להמשיך לדבר איתו ב־HTTP (למרות שהאתר ביקש לדבר ב־HTTPS), בזמן שהתוקף עצמו "יתרגם" את הבקשות מהלקוח ל־HTTPS, ישלח אותן בצורה מוצפנת לאתר ויעביר את התשובות ללקוח. כך המשתמש יחשוב שהוא מדבר עם האתר, אבל בפועל כל התקשורת שלו תעבור לא מוצפנת דרך התוקף.

&#8212;&#8211;

[1] מצטער על הפישוט, אבל אם נכנס למצב מדעי גרידא ולא נתמקד במה שמעניין זה לא יגמר. במחילה.

[2] ממליץ בנתיים על [מאמר ב־DigitlaWhisper][2] לטכנולוגים, או לסקרנים באמת, חפשו באינטרנט, יש המון חומר.

[3] זה לא אומר שאין חורי אבטחה בשיטת התקשורת הזו. כתבתי בעבר [פוסט][3] על הנושא.

[4] נכון, אתם יכולים להגיד שהיה לפני זה רדיו ובלה בלה בלה. בואו נזכור שגם זו טכנולוגיה שקיימת כולה קצת יותר מ־100 שנה.

[5] הסתייגות קלה: בד"כ אתם משתמשים ב־WPA2-PSK, שפשוט מבקש מכם סיסמה. מפתח ההצפנה של התקשורת בינכם ובין מקור האינטרנט מבוסס על הסיסמה, ולכן אם לעוד מישהו יש את הסיסמה הוא יכול לדעת מה המפתח ולהאזין לתקשורת שלכם. יש פרוטוקול נוסף שנקרא WPA2-Enterprise שהרבה פחות נוח להתקין (ולא מיועד למשתמשים ביתיים), שמאפשר לכם להגדיר שמות־משתמשים וסיסמאות, ואז מן הסתם המצב הוא שונה.

[6] [תוסף][4] שמשתדל להפנות אתכם  ל־HTTPS, כשאפשר.

[7] אוקיי, אני קצת משקר בהסבר הזה ומאחד מושגים כדי להוריד סיבוכיות. זה יהיה מגה מסורבל לדחוף בפוסט הזה גם הסבר על IV ועל CCM ועל Stream Ciphers, למרות שממש הייתי רוצה לעשות את זה. זה מעביר את הקונספט בצורה סבירה ומשיג את המטרה שלו. אל תשנאו אותי.

 [1]: https://www.zdnet.com/article/here-is-every-patch-for-krack-wi-fi-attack-available-right-now/?fbclid=IwAR2RgTClHTuoYtUugPLaGiT9uFVe5nn4WY45B5J1qwdUOiN26-djUrInDlE
 [2]: https://www.digitalwhisper.co.il/files/Zines/0x02/DW2-1-SSL.pdf?fbclid=IwAR3nvVkRevhYM7vTWafrLGfiXaLG960984ARrYQOHNmlwQBOsomkYppU93A
 [3]: https://www.facebook.com/Yam.Mesicka/posts/10153454204383297
 [4]: https://www.eff.org/https-everywhere