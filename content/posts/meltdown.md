---
title: מה זה Meltdown?
author: ים מסיקה
date: 2018-02-04T21:29:00+00:00
categories:
  - קריאה ארוכה
tags:
  - meltdown
  - Side
  - channel
  - attack
  - חולשות
  - מעבד
direction: rtl
---

לפני כחודש האינטרנט רעד והשתגע. המילים Meltdown ו־Spectre נזרקו לחלל האוויר כאילו אין מחר, וכולם דיברו על "פרצות העשור" ועל ההשלכות המשוגעות שלהן על הביצועים והחיים של כולנו. היום אנחנו הולכים להבין אחת ולתמיד מי זאת Meltdown שכולם מדברים עליה. הקריאה, כרגיל, מתאימה גם לאנשים לא טכנולוגיים, אבל היא מעט מאתגרת מבדרך כלל, שכן אנחנו הולכים לצלול לעומק פסיכי ולהסתכל על נבכי מערכת המחשב שלנו עם מיקרוסקופ ביוני (לארתור יש כזה אז גם לנו יש כזה). כדי להבין את הפרצה נהיה חייבים להבין כמה מונחים במדעי־המחשב שלחלוטין נשמעים כמו רדיו מעוך, אז תהיו סבלניים איתי. מוכנים למסע? בואו נצא לדרך.

## ידע בסיסי: על מעבד וזיכרון

נתחיל מסקירה שטחית בצורה היסטרית על איך המחשב בנוי פיזית. נתמקד ב־2 רכיבים עיקריים: זיכרון ומעבד.

את הזיכרון אתם יכולים לדמיין כמסדרון עצום בגודלו שמכיל כמה מיליארדי דלתות, ומאחורי כל דלת יש אות בעברית. אם תעברו דלת אחר דלת ותקראו את האותיות, מדי פעם תגלו שמישהו הכניס לשם מסר קוהרנטי. נניח, בדלת 1,500 עד דלת 1,511 תוכלו למצוא את האותיות "חציליםזהאיכס". בהקבלה למדעי־המחשב, זיכרון של מחשב מכיל בדרך־כלל כמה מיליארדי תאים, ואם קוראים הרבה תאים סמוכים (זה קורה די הרבה) אפשר לראות דברים מעניינים, כמו אולי הפוסט שאתם קוראים ממש עכשיו, או אולי אפילו סיסמאות שהזנתם לאחרונה במחשב. כמו שבמסדרון הדלתות שתיארנו לכל דלת היה מספר, גם לכל תא במחשב יש כתובת של ממש, שמאפשרת לנו להתייחס אליו ולגשת למה שיש בתוכו. לצורך העניין, אפשר להגיד שבכתובת מספר 1716150592 בזיכרון יתכן שנמצא את הערך 152. למרות שזה קצת יקשה עליכם לדמיין דברים, תצטרכו להאמין לי שבכל כתובת בזיכרון שמור מספר בין 0 ל־255.[^1]

המעבד הוא גם רכיב חשוב להחריד במחשב שלכם, והוא "זה שעושה הכול בבית הזה". אם לרדת קצת יותר לפרטים, ברגעים אלו ממש יושבים מתכנתים בכל רחבי העולם, וכותבים להם את הקוד של תוכנות כאלו ואחרות שיופצו בעתיד למיליוני מחשבים. כשהתוכנה (נניח Chrome) תגיע למחשב שלכם, המעבד הוא זה שייקח את אותו קוד שהמתכנתים כתבו, ואשכרה יבצע אותו. הוראה אחר הוראה.[^2] המעבדים היום חייבים להיות מגה משוכללים, מכיוון שמחשבים היום צריכים לבצע המוןןןןןןן הוראות כל שנייה, ואכן, מעבדים מודרניים מצליחים לעמוד בסדר גודל של מיליארדי פעולות בשנייה. בפסקאות הבאות נבין, בין היתר, איך הם עושים את זה.

## על זיכרון וירטואלי, מטמון ו־Speculative Execution

עכשיו, כשאנחנו יודעים מה המעבד והזיכרון עושים בגדול, הגיע הזמן לספר שהמעבד צריך לאחזר לעיתים קרובות מאוד דברים מהזיכרון. הבעיה היא שעבור המעבד, לנסות לאחזר נתון מהזיכרון זה כמו ללכת לקולנוע בשביל לראות את Fifty Shades Darker – לכל הפחות בזבוז זמן בלתי נסלח.[^3] לכן עוד בשנות ה־60 המוקדמות הומצא משהו שנקרא "מטמון מעבד" (CPU Cache). מדובר במעין רכיב יקר שנמצא על המעבד, מהיר בטירוף, ומכיל נתונים שונים שאנחנו יודעים שהמעבד ניגש אליהם לעיתים קרובות. ככה, במקום לשאול את הזיכרון שאלה זהה פעמים רבות, המעבד ישלוף את התשובה ישירות מהמטמון הקרוב לביתו, בזריזות וביעילות.

אנחנו ממשיכים עם טריקים מגניבים שהמעבד עושה כדי לייעל דברים, והפעם נדבר על "הרצה שאינה לפי הסדר" (Out of Order Execution). הקונספט פה הוא דווקא די פשוט ומגניב, ואומר שאם יש 2 פעולות שלא קשורות אחת לשנייה וכתובות אחת אחרי השנייה, המעבד יריץ את שתיהן באותו זמן כדי שתהליכים יזוזו מהר יותר. תוכלו לדמיין שאתם מכינים עוגה שמורכבת מבצק וציפוי, אבל מכיוון שאתם אופים מיומנים לאללה אתם מכינים עם יד אחת את הבצק ויד אחרת את הציפוי בתיאום מושלם בין הידיים ובלי להתעכב.

טריק מעבדים נוסף שנלמד יהיה משהו שנקרא "ביצוע לפי הנחות" (Speculative execution). זה כמעט כמו חיזוי עתידות, רק משהו שלרוב אשכרה עובד. נדמיין לרגע שאני מת על קוקטיילים ("נדמיין"), ושכל יום שלישי בשעה 21:00 אני מגיע לבר הבית שלי ומזמין את הקוקטייל האהוב עלי, Industry Sour. אחרי חודשיים של אותה רוטינה אני נכנס לבר בשלישי ב־21:00, פושט את המעיל ומוצא מול הכיסא הקבוע שלי קוקטייל Industry Sour, שמחכה שם במיוחד בשבילי. למה זה קרה? כי הברמן כבר ידע בדיוק מה הולך לקרות, ובזכות זה היה מסוגל לנהל את הזמן שלו ושלי בצורה טובה הרבה יותר.

המעבד שלכם עושה דבר דומה. נניח וכתבנו קוד שמוצא את הסכום של רצף מספרים חיוביים. בקוד כתוב: "אם המספר שאתה קורא מהזיכרון הוא חיובי, תוסיף אותו לסכום". כשהמעבד יראה רצף ארוך מאוד של מספרים חיוביים, הוא יניח שגם המספר הבא שהוא יקרא יהיה חיובי, ויוסיף אותו לסכום עוד לפני שהוא בכלל בדק אם הוא חיובי! אם המעבד יגלה שהוא טעה והמספר בכלל לא חיובי, יופעל מנגנון מתוחכם ומגניב שיחזיר את המצב לקדמותו ללא שום נזק לביצועים.[^5] אם המעבד צדק (ובסבירות גבוהה שזה המצב), הוא ימשיך כרגיל, שמח וטוב לב על כך שנחסך זמן ריצה רב.

אני מקווה שאתם עדיין חיים בשלב הזה, כי אנחנו ממש לקראת סיום. הדבר הכמעט־אחרון שנלמד עליו הוא זיכרון וירטואלי, שהוא קונספט די חשוב שקיים בכל מערכות ההפעלה מאז שנות ה־70. מכיוון שזה קצת קשוח, אני אצטרך שתתרכזו ממש בכמה פסקאות הבאות, ואז תסתכלו באיור שצירפתי. בגדול, הסיפור הוא שמסיבות שקשורות באבטחה ובביצועים, מערכת ההפעלה לא מאפשרת לתהליכים שרצים על המחשב גישה ישירה לזיכרון ה"אמיתי" שנמצא במחשב. במקום זה, היא פותחת עבור כל תהליך חדש שעולה (דמיינו Skype, Chrome או Word), "קופסה מבודדת" וחדשה בזיכרון. לקופסה הזו קוראים "זיכרון וירטואלי". כל פעם שהתהליך ינסה לגשת לכתובת זיכרון מסוימת בתוך הזיכרון הווירטואלי, מערכת ההפעלה "תפנה" אותו לכתובת האמיתית בזיכרון הפיזי של המחשב. אותו זיכרון וירטואלי מאפשר לתהליך לדמיין שהוא רץ לבד, וזה דבר שמשמח מתכנתים, שיכולים להתעלם מהפרעות שעלולות להיגרם מגישה של תהליכים אחרים לזיכרון. לדוגמה, נניח שגם Skype, גם Chrome וגם Word ינסו לקרוא מה יש בכתובת 1234 בזיכרון הווירטואלי שהוקצה להן. מכיוון שהם נמצאים בקופסאות מבודדות שמוקצות להן כתובות מזויפות, כל אחד מהתהליכים ינסה לקרוא מקום אחר בזיכרון הפיזי של המחשב.

![באיור מוצגים שני תהליכים, שמיוצגים בעזרת 2 תאים כל אחד. התא הראשון בכל תהליך נקרא 0-99, והתא השני נקרא 100-199. התאים הללו מייצגים את הזיכרון הוירטואלי של כל אחד מהתהליכים. מעליהם, ישנו בלוק גדול שמעליו כתוב "Physical Memory" ובו 4 תאים: 0-99, 100-199, 200-299, 300-399. מכל אחד מהתאים שלמטה (כלומר, של התהליכים שעבורם מוקצה זיכרון וירטואלי), יוצא חץ שממפה את הזיכרון הוירטואלי לזיכרון הפיזי של המחשב. לדוגמה, מהתא של התהליך הראשון שעליו כתוב 0-99, יוצא חץ לעבר התא 200-299 של הזיכרון הפיזי. מהתא
של התהליך השני שעליו כתוב 0-99, יוצא חץ לעבר התא 100-199 של הזיכרון הפיזי.](/koko.png "איור להמחשת רעיון הזיכרון הוירטואלי.ה")

הדבר היחיד שכל תהליך חולק איתו את הזיכרון הווירטואלי שלו הוא רכיב שנקרא “Kernel”, או ליבת מערכת ההפעלה. מדובר ברכיב חשוב מאוד שמכיל דברים מגה רגישים, ומהתיאור שלי ממש קל לנחש שאנחנו לא רוצים לתת לכל תהליך גישה לקרוא ממנו או לכתוב אליו שום דבר. הסיבה שהוא שם, חולק מרחב וירטואלי עם התהליך שלנו, הוא בעיקר מסיבות שקשורות (שוב) בייעול ביצועים. אבל אל דאגה! אף תהליך לא באמת יכול לקרוא או לכתוב ישירות למרחב הכתובות של ה־Kernel בלי הרשאות מתאימות, שכן יש מערכת הגנה שנמצאת במעבד, ודואגת לבדוק שלכל כתובת יוכל לגשת רק מי שבאמת מורשה לכך. לצורך העניין, לתוכנית עצמה אין גישה לחלק של ה־Kernel בזיכרון הווירטואלי – המעבד לא יאפשר לה לקרוא או לכתוב שום דבר שנמצא שם.

מכיוון שלהבין מה הכתובת של תא מסוים בזיכרון הפיזי לפי כתובתו בזיכרון הווירטואלי יכול לקחת למעבד זמן רב, גם לזה מצאו טריק. אי שם במטמון המעבד יש טבלה שנקראת TLB, או Translation Lookahead Buffer ("חוצץ תרגום בעזרת הצצה"), בה נשמרת הצמדה בין הכתובת הווירטואלית לכתובת הפיזית עבור תאים בזיכרון אליהם ניגש המעבד לאחרונה. זה מאפשר למעבד לגשת במהירות לכתובת הפיזית, ולחסוך פנייה למערכת ההפעלה בבקשה לפענוח הכתובת הווירטואלית, שעלולה להיות יקרה מאוד בביצועים.

## למעמקי מאורת הארנב: איך עובדת Meltdown?

פיווווו! עברתם את החלק הקשה. אם לא דילגתם עד לפה, קחו לעצמכם צנצנת עוגיות או משהו, זה מגיע לכם.

אחרי שהצבנו את כל נגני המקהלה במקומות המתאימים, הגיע הזמן לקרשנדו. תניחו שאתם תהליך כלשהו, רצים לכם בתוך הזיכרון הווירטואלי. בחלק הראשון, אתם יוצרים סדרה של 256 תאים פנויים בזיכרון[^4], נקרא לה מוישלעך, ודואגים שאף אחד מהכתובות של התאים של מוישלעך, שכתובתם היא מ־100 ועד 355 (החלטה שרירותית שלי לשם הדוגמה), לא יהיו שמורים ב־TLB של המעבד. עד כאן הכול חוקי.

בחלק השני אתם עושים מעשה נבזי ולא חוקי, ומבקשים ערך ישירות מכתובת ששייכת ל־Kernel! (סאונד של צעקות בוז והיסטריה כאן, בבקשה). נקרא לערך שיושב באותה כתובת "רוחל'ה". רוחל'ה למעשה יכולה להיות חלק מסיסמה או כל פרט עסיסי שאתם רוצים לדמיין שקיים שם. כמובן שמי שהתעמק בפסקאות הקודמות יצעק עלי ויגיד שזה השלב שהמעבד אמור לזרוק אותי לכל הרוחות, מכיוון שהוא אמור לבדוק שיש לנו בכלל גישה לכתובת של רוחל'ה. 5 נקודות לגריפינדור! אבל... למעבד לוקח זמן לבדוק האם יש לכם גישה לרוחל'ה או לא, ובינתיים הוא ימשיך להריץ את הפקודות הבאות, עוד לפני שמשהו רע קורה. זה לא באמת נורא, כי בכל מקרה המעבד לא ימסור לכם את רוחל'ה עד שהוא יוודא שיש לכם גישה אליה – הוא רק יבצע את הפעולות הבאות "בצד" למקרה שבאמת יש לכם גישה, וככה הוא יחסוך זמן חישוב יקר. שוב, רק אם באמת יש לכם גישה הוא באמת ידאג לשמור את מה שקרה בעקבות הפעולות, ולהחזיר לתהליך שלכם את רוחל'ה, אבל אין לכם גישה, אז הוא לא. הכל בסדר.

בשביל החלק השלישי תהיו חייבים לזכור שהערך של רוחל'ה (זה שביקשתם מה־Kernel) חייב להיות בין 0 ל־255, כי אלו הם הערכים שיכולים להיות בכתובת כלשהי בזיכרון (ראו פסקה ראשונה במאמר), ושמוישלעך הוא סדרה של תאים שהקצנו וכתובתם היא מ־100 ועד 355. עכשיו אתם תבקשו לשמור בצד, במשתנה שנקרא "ארפכשד" (סליחה, נגמרו לי השמות והשעה 5:20 בבוקר) את (ריכוז, ריכוז): הערך שנמצא במוישלעך, במיקום (רוחל'ה+100). זאת אומרת שאם הערך בתוך ה־Kernel (רוחל'ה) היה 50, אתם הולכים לשמור בצד את מה שיש בתא ה־150 בסדרת התאים שיצרנו לפני כמה פסקאות (מוישלעך).

עכשיו אנחנו בחלק הרביעי והמעניין באמת. נחשו מי התעורר וצועק "מה זה השטויות האלו!?" המעבד! זה השלב שבו הוא כבר קלט שכל מה שניסינו לעשות בשלב 2 פחות סבבה, וכאן הוא כבר מחזיר את המצב לקדמותו, כך שעל פניו הוא לעולם לא \*באמת\* ישנה את ארפכשד ורוחל'ה, וכל זה מתקיים אך ורק באופן זמני בתוך מוחו הקודח של המעבד. איפה הקאץ'? בעצם זה שקראנו את הכתובת של מוישלעך במיקום רוחל'ה+100 לתוך ארפכשד, המיקום של מוישלעך במקום רוחל'ה+100 נשמר ב־TLB.

בחלק החמישי והאחרון לכתבה זו (אומג, שרדת!!! לקרוא... רק... עוד... קצת! אנחנו שם), נלמד טריק מגניב ממש שנקרא Flush+Reload: אנחנו ננסה לגשת לכל אחת מהכתובות שנמצאות במוישעלך. נניח שעבור תא 100 שנמצא במוישעלך קיבלנו תשובה תוך 30 מילישניות, עבור תא 101 תוך 32 מילישניות, עבור תא 102 תוך 31 מילישניות (...) ועבור תא 150 קיבלנו תשובה תוך מילישנייה אחת. נוכל להסיק מהסיפור שמישהו "נגע" בכתובת 150 במוישלע'ך והיא שמורה ב־TLB, לכן היא אוחזרה כל־כך מהר, מה שאומר שהערך של רוחל'ה היה 50! גילינו ערך ממרחב הזיכרון של ה־Kernel בלי שהייתה לנו גישה אליו בכלל! תדהמה והמולה!

מה שתוקף חביב יעשה עכשיו, זה לקרוא בשיטה הזו הרבה הרבה רוחל'ה-יות, אחת אחרי השנייה. כשהוא יסיים, יהיה בידיו מיפוי מלא של הזיכרון. טירוף.

## אחרית דבר

יש הרבה דברים מגניבים שאני ממליץ עליהם כקריאות המשך שאולי עוד אסקור פה בעתיד, כמו על Spectre, על הדרכים למניעה של שתי החולשות ועל לינוס טורבלס, ה־מפתח של לינוקס, מתחרפן מעצבים וקורא לתיקון שאינטל הציעו לו "זבל מוחלט" ([COMPLETE GARBAGE][1]). אני פה כדי לענות לשאלות מיד אחרי תנומה קלה, וכרגיל, אשמח לכל פידבק או תיקון לפוסט.

שבוע קול לכולם :) 

----

מאמרים שעזרו לי לכתוב לכם דברים:

  * ה־[White Paper][3] של Meltdown
  * הערך [Meltdown][4] בוויקיפדיה (ועוד 20 ערכים שנבעו משם)
  * [ArsTechnica][5]
  * [ברומיום][6]
  * [מת' קליין][7], משם התמונה המצורפת. תודה על זה.
  * על [Flush+Reload][8] מכנס BlackHat 2016

[^1]: בגדול כל כתובת מתייחסת ל־8 תאים קטנים שנקראים "ביט" (bit) כל אחד, או "בייט" (byte) עבור כל 8 הערכים. גישה לכתובת מסוימת בזיכרון תביא בייט, ולא ביט, ולכן 2^8 ערכים אפשריים מביאים 256 אפשרויות, שהן מ־0 עד 255.

[^2]: ואם לנדנק לרגע – הוא לא מבצע לחלוטין את ההוראות שהמתכנתים כתבו, אלא תוצר ש"תורגם" מאותן הוראות שלהם לשפה שהמעבד מבין.

[^3]: רק שלמעבד אשכרה יוצא מהעניין משהו

[^4]: לאנשים טכניים מאוד, במימוש המקורי מדובר על 256*PAGE_SIZE בייט כדי למנוע מה־prefetcher לבאס אותנו ולקרוא יותר מדי מידע ל־cache.

[^5]: התותח [Tal Skverer][2] מחדד שכן יש "עונש" קל בביצועים עבור כל ניסיון של המעבד להחזיר את המצב לקדמותו, אבל הוא כמעט זניח יחסית למה ששיטת הניחוש המדוברת חוסכת.

 [1]: http://lkml.iu.edu/hypermail/linux/kernel/1801.2/04628.html
 [2]: https://www.facebook.com/tal.sk?__cft__[0]=AZV2FmL4atVCjupqKvEkmcAwObKqAE7tjNRKcrIGH1MIS8QBFnjaho8-X8l8aHcHVDdpUIPCJ8dSvhZt0GodXk2WwaAzsGW1Ih67yVycFeCB53parsc4zsJ5QkPbh4SwVk8&__tn__=-]K-R
 [3]: http://bit.ly/2E6myYl
 [4]: http://bit.ly/2CSO7o1
 [5]: http://bit.ly/2lQ7iIg
 [6]: http://bit.ly/2GKt09t
 [7]: http://bit.ly/2FFJPBQ
 [8]: http://bit.ly/2E1qUQL
