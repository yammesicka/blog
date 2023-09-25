---
title: 'הסבר פשוט: DDoS על שרתי DNS'
author: ים מסיקה
date: 2016-10-22T22:32:00+00:00
categories:
  - חדשות טכנולוגיות
  - קריאה ארוכה
tags:
  - DDoS
  - DNS
  - IOT
  - Mirai
  - אבטחת מידע
  - משל הבירה
  - מתקפת מניעת שירות מבוזרת
  - סייבר
---

מה זו המתקפה המדוברת שקרתה אתמול? מה זה DNS, מה זה DDoS (מתקפת מניעת שירות מבוזרת), מה הקשר ביניהם ולמה זה מנע ממני לראות את Black Mirror :@?

בהמשך המאמר גם המתקדמים יוכלו להצטרף אלינו עם תאוריות קונספירציה מגניבות על מה הסיבה למתקפה, ועל מומחי אבטחה שטוענים ש"מישהו מנסה להוריד את האינטרנט" (WTF; אבל מבטיח שיהיו יותר אינטליגנטיות מתגובות הזויות בוואלה).

~

אמ;לק לטכנולוגים: ספקית שירותי DNS מהמובילות בעולם, Dyn, הייתה תחת מתקפת DDoS אימתנית, שנוצרה ככה"נ ע"י בוטנטי Mirai (שיודעים להשתלט על מכונות IoT ולהוציא נפחי תעבורה הזויים). זה גרם לכל מי שהשתמש בשירותים של Dyn, כולל כמות עצומה של אתרים פופולריים, להיות לא נגישים.

~

בליל ה־21/10/2016 "נחסמה" גישה לשירותים רבים ברחבי האינטרנט, ביניהם שירותי ענק עם מליוני משתמשים, כמו Twitter, CNN, BBC, PayPal, Amazon, Netflix, Spotify, Reddit, GitHub ועוד.

אתרי חדשות רנדומליים יפטרו את עצמם מהעניין כשיגידו לכם שהייתה מתקפה על משהו שגרם לאיכסה באינטרנט. אבל בואו ננסה להיות קצת יותר רציניים ואשכרה להבין מה לעזאזל גרם לכל העניין המצ'וגע הזה.

## מתקפת מניעת שירות מבוזרת, או DDoS

בואו נדמיין שאתם מוכרים בירה ישראלית מדהימה בשרונה מרקט, ומציעים טעימות בחינם לאנשים – גם כי אתם נחמדים אש, אבל מן הסתם גם מתוך כוונה שאנשים יטעמו בירה או שתיים ואז ישלמו בכספם.[1] אבל הו אז מגיעים לשם חבורה של יצורים מעיקים, שפשוט מתחילים לאכול לכם את הראש ולבקש לטעום מכל פאקינג בירה שיש לכם במלאי. כל פעם שאתם באים לתת שירות ללקוח אחר, לגיטימי, אחד הקופים מנפנף בבקבוק בירה וצועק לכם "ומה זה?!?!? טעים הדבר הזה?!?" ומטיח את הכוס על הבר. בלית ברירה אתם מסמנים ללקוח האחר שימתין, וחוזרים בצער לנסות לרצות את חבורת הדגנרטים.

סיוט הבלהות שאתם עוברים נקרא DDoS, או Distributed Denial of Service, או בעברית: "מתקפת מניעת שירות מבוזרת". אתם תקועים עם מספר גדול של לקוחות לא לגיטימיים שבכלל לא רוצים את השירות שאתם מציעים (קניית בירה), בעודם לוקחים לכם משאבים בלי שהם באמת צריכים אותם. בכך הם גורמים לכם עומס, ולא מאפשרים לכם לשרת את הלקוחות האמתיים שלכם.

בהקבלה לאתרי אינטרנט: מישהו רע (התוקף) השתלט על המון מכונות (לדוגמה, מחשבים) שמחוברות לאינטרנט. בעגה המקצועית כל אותן מכונות נקראות "זומבים" (נשבע לכם, לא מסתלבט) או "בוטנטים". דמיינו שיש לו כמה מאות אלפים כאלו.

לתוקף הרשע נמאס מהאתר הכי טוב ברשת, [http://nyan.cat][1], מכיוון שבעל האתר החליף את המוזיקה המעולה שהייתה בו קודם. הוא מבקש מכל צבא הזומבים שלו לבצע הורדה של הסרטון הכי כבד באותו אתר. כל שנייה. האתר מצליח להתמודד עם הבקשה הראשונה, העשירית והמאה. אבל בבקשה האלף הוא כבר לא יצליח להחזיר תשובה, כי הוא יהיה עסוק בלתת שירות עבור כל אותם 999 האנשים הקודמים.

זה DDoS.

אתם בטח שואלים את עצמכם "כמה עומס כבר ניתן לגרום לאתר ככה?" – התשובה היא שהמתקפה האחרונה הגדולה המתועדת היא כשהאתר מקבל 600 ג'יגה־בייט&#8230; כל שנייה(!)

## ספר הטלפונים של האינטרנט (DNS)

החלק השני הוא קצת יותר מסובך. נתחיל בכך שנבין מה קורה, ממש ממש בגדול, כשאתם נכנסים לאתר אינטרנט. נניח ל־ynet.co.il\[ר' 2\] (למה אתם עושים לעצמכם את זה?!)

דמיינו מחשב דומה לשלכם שיושב במקום כלשהו בעולם, נניח באוסטרליה. המחשב הזה ממש דומה לשליח אוכל – מטרתו היא להאזין לבקשות שהוא מקבל, ולהחזיר לכל בקשה תשובה מתאימה. כמו שאם נבקש מהשליח של ג'ירף&nbsp; את המנה המלזית ונקבל אחת הביתה? כאן אנחנו יכולים לבקש "תביא לנו את דף הבית!", והוא יביא את דף הבית. אנחנו יכולים לבקש את הסרטון השני בכתבה "האם האנושות תכחד מחר? צפו:", והוא יביא לנו את הסרטון. למחשב הזה נקרא "השרת של אתר ynet.co.il".

לכל מחשב שמחובר לרשת האינטרנט יש כתובת מסוימת שספקית האינטרנט שלו נתנה לו. השם של זה כנראה מוכר לכם, "כתובת IP", והיא מורכבת מ־4 חלקים שמופרדים בנקודה, בעלי 1–3 ספרות כל אחד.[3] כשאנחנו רוצים לשלוח בקשה לאותו מחשב, במקרה הזה לשרת של אתר ynet.co.il, אנחנו נהיה חייבים לספק בדרך כלשהי את כתובת ה־IP שלו. או שמא?

כשאני רוצה להתחיל להזמין את המנה המלזית שלי מהשליח של ג'ירף, אני קודם כל צריך לברר את מספר הטלפון שלהם. אני מסתכל בספר הטלפונים המהודר שלי (נו, תזרמו), ואני מוצא את הרשומה "ג'ירף &#8230;&#8230; 03.0000000". עכשיו אני יכול להתקשר ולהזמין.

אותו דבר בדיוק קורה עם ynet.co.il: אני צריך להבין מה הכתובת שלהם. מה אני עושה? במקום שיהיה לנו ספר טלפונים ידני ומעפאן, האינטרנט המופלא מצא פתרון מדהים: DNS, או Domain Name Server. ה"שירות" הזה מאפשר לכם, בתהליך אוטומטי לחלוטין, לשאול: "מה כתובת ה־IP של ynet.co.il?" ולקבל בחזרה את התשובה.

בואו נסכם: כשאני רוצה להזמין מג'ירף, אני יודע שאני צריך להתקשר ל"ג'ירף". אני פותח ספר טלפונים, מוצא את "ג'ירף", משיג את המספר שלהם, מתקשר, מזמין מנה מלזית, מקבל לבית. כשאני רוצה לקבל את העמוד הראשי של ynet.co.il (נו, היפותטית), המחשב שלי (ברקע) מוצא בעזרת DNS איפה נמצא "ynet.co.il", משיג את הכתובת "88.221.144.18" (ה־IP), מבקש ממנו לקבל את דף הבית, ואנחנו מקבלים אלינו את דף הבית. יאי. ככה לא צריך ספר טלפונים לאתרי אינטרנט או לזכור את כל המספרים המציקים האלו בעל־פה.

ברכותיי! אתם יודעים את העיקרון שעומד מאחורי DNS! קחו לעצמכם מנה מלזית (גם עוגייה זה בסדר) ונמשיך הלאה.

## אז מה קרה?

קול! עכשיו כשאנחנו יודעים בכלליות מה זה DDoS ומה זה DNS אפשר לדבר דוגרי. בואו נחבר סיפור מהחלקים שהשגנו עד כה:

ישנה חברה בשם Dyn שאחראית על נתינת שירותי DNS לאתרים. החברה עברה DDoS, כנראה מהכבדים שנראו עד כה בהיסטוריה. האתרים Twitter, Paypal, Netflix ושאר החברים השתמשו בשירותיהם של Dyn, ולכן לא יכולתם למצוא אותם.

זו אחת ההצהרות שיצאה מהחברה: "המורכבות של ההתקפות מקשה עלינו מאוד. ההתקפות כל־כך מבוזרות, הן באות מעשרות מליוני כתובות IP מרחבי העולם[&#8230;]"

אז מה קורה כשחברה כמו Dyn שמחזיקה את היכולת לענות לכם על "מה הכתובת של &#8230;" לא מסוגלת לספק שירות? בדיוק מה שקורה כשאין לכם את הספר טלפונים – אתם לא יכולים להתקשר לג'ירף!\[4\] (או להיכנס לאתר אינטרנט, כי אין לכם את הכתובת האמתית שלו). עכשיו אתם בטח מגרדים את הראש ממש ושואלים את עצמכם מי יוזם את כל העניין הזה, ואיך יכול להיות שהוא השתלט על כל־כך הרבה מחשבים? אז לגבי "מי יוזם את כל זה" לא תהיה לי תשובה חד משמעית כנראה, אחד הדברים שעלולים לקרות ב־DDoS מתוכנן היטב. אבל לגבי כמות המחשבים?

לפי דיווחים אחרונים, נעשה שימוש בתכנה זדונית בשם Mirai*, שהייתה אחראית על אחת ההתקפות הגדולות שהכרנו עד עצם היום הזה. הרעיון הכללי הוא פשוט ומוכר: התכנה הזדונית מנסה להתחבר לכל מני מכונות IoT (ראוטרים, מצלמות וכד') בעזרת שם משתמש וססמה שבאים עם אותן מכונות מהמפעל (יש לתכנה רשימה מוכנה מראש של שמות משתמש וסיסמה), ולהדביק אותם.[5] בגלל חוסר המודעות שיש לעניין האבטחה של המוצרים האלו, הססמאות האלו עובדות לעתים מאוד קרובות, ובחלק מהפעמים הם אפילו צרובות ממש על פיסת החומרה. כך ניתן להשיג גישה למאות ולמיליוני מכשירים המחוברים לרשת.

## פינת הקונספירציות

נסגור עם פינת "קונספירציות מתקבלות על הדעת" (לא מגיב 28 בנענע10, חייזרים לא מנסים לתקשר איתנו באמצעות DDoS):

  * ברוס שנייר, מומחה אבטחה עם שם בינ"ל, [טוען][2] שלאחרונה אנחנו רואים המון מתקפות DDoS כנגד "תשתיות בסיסיות שגורמות לאינטרנט לעבוד". טענתו הכללית היא שמישהו שם בחוץ "בודק איך להפיל את האינטרנט", ועם כמה הזוי ולא מקצועי שזה נשמע, לבחור יש נקודה לא רעה. המתקפות לאחרונה מתבצעות בצורה אגרסיבית מאוד, כזו שלטענתו "מכריחות את השירותים לחשוף את יכולותיהם ההגנתיות". "מי יעשה דבר שכזה?" הוא טוען, וממשיך לפרט: "לא נראה שזה מעשה ידיו של אקטיביסט, קרימינל או חוקר. לאפיין תשתיות ליבה נשמע כמו דבר שנהוג לעשות בריגול ובאיסוף מודיעין. זה לא מתאים לחברה מסחרית. יותר מזה, הגודל וההיקף של הבדיקות האלו &#8212; ובמיוחד ההתמדה שבהן &#8212; מצביעות על שחקנים לאומיים. זה נראה כאילו מפקדה צבאית מנסה לכייל את הנשקים שלה למקרה של מלחמת סייבר. זה מזכיר לי את התכנית של ארה"ב במלחמה הקרה לטוס בגובה רב מעל ברית המועצות ולכפות עליהם להפעיל את מערכות ההגנה האווירית שלהם, לצורך מיפוי היכולות של ברית המועצות."
  * לפני יומיים פורסמה באתר האבטחה KerbsOnSecurity מאמר על חברה בשם BackConnect שאמורה להגן על חברות מפני מתקפות DDoS. המאמר הלא מחמיא תיאר איך BackConnect פורצת לזומבים שמנסים לעשות DDoS על אתרים שהיא משרתת לצורך איסוף מידע עליהם, ועושה דברים לא לגיטימיים נוספים.[7] מספר שעות לאחר עליית הכתבה לאתר, הוא הותקף ב־640Gb לשנייה. באופן לא קשור(?) המתקפה על Dyn התחילה מספר שעות אחרי ש־Doug Madory, חוקר אבטחת מידע שעובד בחברה, הרצה בפגישה בטקסס על המאמר מ־KerbsOnSecurity, שהוא היה שותף לכתיבתו.

~

לקריאה נוספת:

  * מי גורם לאינטרנט של הדברים להיות תחת מתקפה? קריאה על Mirai של כרבע שעה: [http://bit.ly/2dX6Ou0][3]
  * שווה לקרוא על שיטות להגנה מ־DDoS (חפשו DDoS Mitigation).
  * לינק מעולה (ת' ל־[Ori Gill][4] ול־[Ran Rubinstein][5] על ההפניות) לחשיבה נוספת על מי מאחורי ההתקפה, והאם התיאוריות שהוצגו כאן מחזיקות מים: [http://bit.ly/2dy8cSO][6]

~

* שוחררה כ[קוד פתוח][7], על מנת להקשות על זיהוי התוקף, ככל הנראה. מאז שחרור הקוד הוכפלו כמות המחשבים הנגועים.

[1] הסיפור היפותטי לחלוטין – הטעימות הן רק מברזים. אני לא מרוויח מזה שקל. אבללללל&#8230; אתם לגמרי רוצים לבקר שם, ב[ביר מרקט תל אביב &#8211; Beer Market TLV][8]. הם המקום הכי טוב לשבת בו על בירה ישראלית. אל תהיו קופים.

[2] שימו לב: מה שקורה מרגע שאתם מקלידים "ynet.co.il" בשורת הכתובות ועד שאתם רואים את האתר עצמו הוא הליך מדהים במורכבותו, ומן הסתם שלא אצליח לכסותו במסגרת הפוסט. אמיצים טכנולוגיים יכנסו לפה: [http://bit.ly/2eRz3zl][9] כדי לראות מה קורה ברגע שאנחנו מכניסים [google.com][10] לשורת הכתובות ולוחצים אנטר.

[3] כך נראות כתובות IP מ"סוג" IPv4. הבעיה איתן זה שיש יחסית מעט מהן והן תוכננו בשלב שבו לא חשבנו שיהיו כ"כ הרבה מכשירים שמחוברים לאינטרנט. יש מקסימום 4,300,000,000 מהן, ואנחנו סובלים פה מפיצוץ אוכלוסין. מחניק פה. בעתיד הלא כ"כ רחוק נעבור כולנו להשתמש ב־IPv6 (תהליך שכבר התחיל חלקית) ויהיה לכולנו הרבה יותר כיף. לקריאה נוספת: [http://www.google.com/intl/en/ipv6/][11]

[4] אתם פשוט מחפשים באינטרנט! אני יודע! זרמו איתי עוד טיפה, אנחנו כבר שם נו!

[5] כרגיל המציאות טיפה יותר מורכבת (אבל את האמת שלא בהמון). ראו כאן: [http://bit.ly/2eqnBJA][12]

[7] קישור למאמר: [http://bit.ly/2eg1bua][13]. למאמר נוסף בבלוג של Dyn ראו [http://bit.ly/2eRIbUJ][14]

 [1]: https://l.facebook.com/l.php?u=http%3A%2F%2Fnyan.cat%2F%3Ffbclid%3DIwAR0ENmgaIUhN9YNUzElHBk5dQr9KSJlGyLWOTsr52LGmkAeKJ6XkqUDlxOE&h=AT0qH1LWJWE3m_pLUf1ZOdeR_4nBWuolxalTASmDmg23PyPowAUXhMlZgWKH26_dyKzVhlXg0FzxZknbOzGS0og34Kx7EMHfknXhaiMX6vVuAQUgvTobLm4KBAyeU7fk6QVOj8cevQ&__tn__=-UK-R&c[0]=AT3zO65Pg4dhhfbY8g71z--KNguOle6sTT4Z7ortdMKFTDfcWL13kJy5OgU9m9bx6IVKwTydMxe0nRdDYYIdc_R3iqgflf7dYggCfqIr4UuC9oWUhKsQSzv8QNEercAmvEwao3QiEgVUjZ1hUmZv7xbkKpHSYJrLLCRo5GP9AQ
 [2]: http://bit.ly/2erEg0T
 [3]: https://bit.ly/2dX6Ou0?fbclid=IwAR0OjYjt-TZb7VrEumH7vU-PR094KhUIAkQsEu0IAiCkDDGkdD1TFfGgOTA
 [4]: https://www.facebook.com/ori.gill?__cft__[0]=AZWP-taiJ6VZNOP7B_07ahcs750qlucy2gOalE2zqr5KFxjUbUUq4xosTYCCRG0kKiIIz7DvVwEJrvVQFGKUb1eAAbHysJmF0G-Fbv21KqduXcidTQQGoYlE0rTFWABHAKcUv0EJwCFkXFCQR7CKOFOu&__tn__=-]K-R
 [5]: https://www.facebook.com/ranru?__cft__[0]=AZWP-taiJ6VZNOP7B_07ahcs750qlucy2gOalE2zqr5KFxjUbUUq4xosTYCCRG0kKiIIz7DvVwEJrvVQFGKUb1eAAbHysJmF0G-Fbv21KqduXcidTQQGoYlE0rTFWABHAKcUv0EJwCFkXFCQR7CKOFOu&__tn__=-]K-R
 [6]: https://l.facebook.com/l.php?u=https%3A%2F%2Fbit.ly%2F2dy8cSO%3Ffbclid%3DIwAR01gnPx6P4W6vCxGMGfUU9Q87TJv0PziSgYQfOi3ogKrTo5VIkGF4Fl9IQ&h=AT2BcpDNNPBLKVq-MhzQc6mFFssI7N6LwlaPEle597ZO5qwvFOmSjuqDt8RH9Yfo81hD7DCNS4TsJ2ogf1QszdpltrF9-cZ8Kc-8xVfoYeSUvI22ki7QS6RfpH9T8b-8TnWMWIJV6g&__tn__=-UK-R&c[0]=AT3zO65Pg4dhhfbY8g71z--KNguOle6sTT4Z7ortdMKFTDfcWL13kJy5OgU9m9bx6IVKwTydMxe0nRdDYYIdc_R3iqgflf7dYggCfqIr4UuC9oWUhKsQSzv8QNEercAmvEwao3QiEgVUjZ1hUmZv7xbkKpHSYJrLLCRo5GP9AQ
 [7]: https://github.com/jgamblin/Mirai-Source-Code?fbclid=IwAR3dtrA4q_vjYfm5huGkZSLg-iwO0nZWC0VF4lb_ZC8X9h_4P8IUKdMahGI
 [8]: https://www.facebook.com/BeerMarketTLV/?__cft__[0]=AZWP-taiJ6VZNOP7B_07ahcs750qlucy2gOalE2zqr5KFxjUbUUq4xosTYCCRG0kKiIIz7DvVwEJrvVQFGKUb1eAAbHysJmF0G-Fbv21KqduXcidTQQGoYlE0rTFWABHAKcUv0EJwCFkXFCQR7CKOFOu&__tn__=kK-R
 [9]: https://l.facebook.com/l.php?u=https%3A%2F%2Fbit.ly%2F2eRz3zl%3Ffbclid%3DIwAR0f5S4eOHCnaUZIbnK-FcaPXwKbOpNUMMxXQ00ZruXfrNxmrOCVdbLOMQQ&h=AT3pElzjogYcoCnBygK8-MD9p7yi9-3G1LqMTErrllZpq84IRxFMW3xzldk5xdEuq9LG2QGsAUWKqxUH-YW9qhj5puoephyrsBES6NP93ZzMAtmefg8X6sHTxR8zbGsnCV6aZSZK-A&__tn__=-UK-R&c[0]=AT3zO65Pg4dhhfbY8g71z--KNguOle6sTT4Z7ortdMKFTDfcWL13kJy5OgU9m9bx6IVKwTydMxe0nRdDYYIdc_R3iqgflf7dYggCfqIr4UuC9oWUhKsQSzv8QNEercAmvEwao3QiEgVUjZ1hUmZv7xbkKpHSYJrLLCRo5GP9AQ
 [10]: https://l.facebook.com/l.php?u=https%3A%2F%2Fgoogle.com%2F%3Ffbclid%3DIwAR2rLU_stNtQC8os0cEPIKDufwOh2-u4m_krzr0keIZWLOwnWEe6QYRB6-I&h=AT1eaimVCy21mzpWlKMJUdSjhYV81Sq-isGf_yDbzjRL6xuKvlZgBVDaStjNj2NAl8eICycYYEGTJbD8f9up9oWzzUIdfkKn9Fqq6feumb-C6U9bTgH-e2861MR_dljSa10yhch4dA&__tn__=-UK-R&c[0]=AT3zO65Pg4dhhfbY8g71z--KNguOle6sTT4Z7ortdMKFTDfcWL13kJy5OgU9m9bx6IVKwTydMxe0nRdDYYIdc_R3iqgflf7dYggCfqIr4UuC9oWUhKsQSzv8QNEercAmvEwao3QiEgVUjZ1hUmZv7xbkKpHSYJrLLCRo5GP9AQ
 [11]: https://www.google.com/intl/en/ipv6/?fbclid=IwAR2mTUkJf5m4NFpxcJLIXlCvTXCRR14YFNZmvUBFgcZHz_qN-_sg-zEe9TY
 [12]: https://bit.ly/2eqnBJA?fbclid=IwAR3riMLgpw9NKZX51FjicWgIDO-aU6Y3QpaJUIBg5nYFYoEqYEKU0XRkbzI
 [13]: https://l.facebook.com/l.php?u=https%3A%2F%2Fbit.ly%2F2eg1bua%3Ffbclid%3DIwAR38DuW0ItyM3SC35Sj4MMtGsW53bmqxOxQU4EldI_s6vEjYbKPwS5luhHI&h=AT2JRch1HKa9Abz5XW4TNs7Vb1FWUsZGytybJ9xLwx4K5DpScHczoIhVkeWqh8Z-9t2QrO5_eqBspdl5uQ5HDezdxrpYIJUi78kMMMtEaobH_UGSCykN8LS7W1kJpMP0kk-yi5jBHg&__tn__=-UK-R&c[0]=AT3zO65Pg4dhhfbY8g71z--KNguOle6sTT4Z7ortdMKFTDfcWL13kJy5OgU9m9bx6IVKwTydMxe0nRdDYYIdc_R3iqgflf7dYggCfqIr4UuC9oWUhKsQSzv8QNEercAmvEwao3QiEgVUjZ1hUmZv7xbkKpHSYJrLLCRo5GP9AQ
 [14]: https://l.facebook.com/l.php?u=https%3A%2F%2Fbit.ly%2F2eRIbUJ%3Ffbclid%3DIwAR2KywDZ1jVkev8QV3OQJLfImAdqyef-w6XgUMO8bMkj9OKDQPddeFiLFs0&h=AT08_UpV7wgwTaKfAcOSawv8OcJ0YRMn_Ecr9FSSHD5kckNKh95wZm-51zVLawRkh02233AHrV4_bk7kszXIz_26IxEYyXFvkCkvJvHWr8QCIbhHoUk7DE4sQELv_hW6uAsEmGDvYw&__tn__=-UK-R&c[0]=AT3zO65Pg4dhhfbY8g71z--KNguOle6sTT4Z7ortdMKFTDfcWL13kJy5OgU9m9bx6IVKwTydMxe0nRdDYYIdc_R3iqgflf7dYggCfqIr4UuC9oWUhKsQSzv8QNEercAmvEwao3QiEgVUjZ1hUmZv7xbkKpHSYJrLLCRo5GP9AQ
