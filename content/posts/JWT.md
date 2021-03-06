---
title: "למה JWT הוא ערק ערק ככה חזק?"
date: 2020-08-13T17:01:42+03:00
draft: false
categories: ["."]
tags: [""]
featured_image: /JWT.png
---

אוטנטיקציה דרך קוקיז עובדת לא רע בכלל לאתרים, וקשה להצביע על יתרונות אבסולוטים למעבר אל אוטנטיקציה דרך JWT. אבל זה, כמובן, כשהאתר הוא אתר, ושהוא נתמך ע"י דפדפנים שתומכים בקוקיז. מה עם אפליקציות נייטיב, שאין להן תמיכה של קוקיז? כאן JWT זורח. 

למען הסר ספק, ההבדלים בין Authentication, Authorization הן ש-authentication הוא הזדהות (מי אתה?), ו- authorization הוא "אם אתה רוצה לקנות ערק תראה לי שאתה 21+" (הרשאה).

אז הביטוי JWT נקרא Json Web Token, והמימוש שלו חמוד לאללה. JWT זה טוקן הנראה כגוש האש גדול, המופרד ל-3 חלקים בעזרת נקודה חמודה. 

הסטרינג הראשון, הוא בד"כ json בעצמו שמכיל מידע על האלגוריתם האש שהשתמשו בטוקן כדי לחתום אותו. הסטרינג השני, שהוא גם בד"כ Json, הוא המידע עצמו שרוצים לשמור כדי שהמשתמש יוכל להזדהות (כמו ID של המשתמש, תאריך תפוגה ועוד'). 

 שני ה- jsonים מקודדים לסטנדרט של base64url, ו*אינם* מוצפנים, כמובן. כל אחד יכול לפענח את שני הסטרינגים הללו ולקרוא את המידע. מכאן, מגיע הבטיחות בחלק השלישי של הטוקן: החתימה.
 
הרעיון של הטוקן הוא לא להעביר מידע מוצפן (לפחות לא JWT סימטרי), אלא לוודא שאף אחד לא *משנה* את המידע. בשביל לוודא זאת, לוקחים את החלק הראשון (שנקרא גם header), החלק השני (שנקרא גם payload), מחברים אותם לסטרינג אחד, מוסיפים לו מפתח שקיים אצל השרת *בלבד*, ומהאהשים בהתאם להגדרות האלגוריתם של החלק הראשון (ה-header). התוצאה: סטרינג, או 'חתימה' שהיא החלק האחרון בטוקן. וזה, מוודא לנו שאף אחד לא נגע בטוקן. מי שיצר אותו יקבל את אותו הטוקן ללא שינויים זדוניים.

מכיוון שרק לשרת יש את המפתח שאנחנו מוסיפים לחתימה, אף אחד אחר לא יכול לייצר את החתימה, ולכן אף אחד לא יוכל לעבוד על השרת. כשהשרת מקבל את הטוקן, הוא פועל שוב ע"פ ההוראות למעלה, ומוודא שהחתימה שיוצאת לו, היא אותה חתימה המתקבלת בטוקן. לא- מחזיר 401, כן- יאללה ערק.

המקרה בו יש רק מפתח אחד, נקרא JWT סימטרי. כלומר, קיים רק מפתח אחד ('פרטי') שיכול לאשר את החתימה, ובשביל שעוד שרתים יוכלו לאשר את הטוקן, צריך לפזר את המפתח ביניהם. זה בד"כ מתכון לפירצות אבטחה, כשאם שרת אחד נפרץ, כל השרתים שמחזיקים את המפתח נמצאים בסכנה.
