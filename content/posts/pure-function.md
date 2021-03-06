---
title: "מה זה pure function, ומה ה'חסרון' הכי גדול של זה?"
date: 2020-09-22T08:49:57+03:00
draft: false
categories: ["כללי"]
tags: ["כללי"]
featured_image: /Pure-Function.png
---

מי שחובב תכנות פונקציונאלי (Functional Programming), מכיר את המושג pure function, שמשמעותו פונקציה המספקת את התנאים הבאים:  א) תמיד להחזיר את אותו הפלט ביחס לקלטת שהיא מקבלת, ו- ב) לא להכיל שום side effect בתוך הפונקציה עצמה, כמו: שינוי משתנה מחוץ לסקופ שלה, Input/Output, או כל דבר אחר שלא קשור לפלט של הפונקציה.

כלומר, הפונקציה צריכה להיות תלויה בקלט שהיא מקבלת בלבד, ולא להתערב בשום דבר שאינו בתוך הסקופ שלה, או שאינו חלק מה"עולם" שלה. זה אומר שאפילו לשנות אובייקט בתוך הסקופ שלה גורע ממנה את התואר.

פונקציה PF היא אבן יסוד בפרדיגמה של תכנות פונקציונאלי, אשר שם דגש על כמה שפחות לשנות ולהשפיע על הערכים של אובייקטים קיימים (טכנית, הוא קשוח ומעודד שלא יהיה אפשר לשנות ערכים של אובייקטים לאחר שהם נוצרים, כלומר immutable). הדוגמאות הכי קלאסיות לתיאור פונקציות PF הן פונקציות מתמטיות, המסמנות וי בקלילות על שתי הדרישות. הן תלויות בקלט שהן מקבלות, וביחס אליו מחזירות את הפלט. לא קורה שום דבר אחר בין לבין. 

לפונקציה PF יש מספר יתרונות מאוד נעימים, וחסרון אחד מתסכל במיוחד. היתרון הראשון הוא שקל לבדוק פונקציה שכזאת. מכיוון שהפונקציה תמיד מחזירה את אותו הפלט ביחס לקלט, וגם היא אינה תלויה במידע שנמצא בערוצים אחרים כמו דטאבייס, יהיה קל יותר לבדוק אותה ולוודא שהיא מחזירה את התוצאה הנכונה.

יתרון נוסף, הוא האפשרות לשרשר pure functions אחת עם השנייה. מכיוון שהלוגיקה והערכים שהן מחזירות הם 'צפויים' (predictable), אפשר להרכיב פונקציות ללא חשש מהשלכות לא צפויות של פונקציה אחת על השנייה (כאן הסדר שלהן יכול להיות משנה).

פונקציות PF גם נוחות יותר לתכנות מקבילי (parallel), מכיוון שאסור לפונקציות הללו לקרוא מכל מקור אחר או לכתוב לכל מקור אחר מלבד הקלט שלהן. הן לא ישפיעו על מידע שלעוד טרדים (Threads) אחרים יש גישה אליהם, מה שבעצם מונע race condition.

אבל לפונקציה PF יש חסרון מאוד קריטי: היא לא יכולה לבצע IO! אחרת התואר נשלל ממנה. אם היא מקבלת אינפוט ממקור אחר (read), היא תלויה גם בערך שהיא קראה אותו, ועלולה להחזיר ערך שאינו תלוי בקלט. אם היא משנה משהוא בדטאבייס (write), היא משפיעה על מידע במקור אחר.

מבלי כל ביצוע של IO, אנחנו לא נוכל לקרוא מידע מהדיסק, מהרשת, או לקבל קלט מהאינטראקציה של המשתמש.
איך פותרים את זה? ע"י...לא, כרגע סוג של אי אפשר לפתור את זה.

שפות תכנות פונקציונאליות כמו Haskell & Scala משתמשות ב- IO Monad, מעטפת אבסטרקטית ש'מאפשרת' לפונקציות שלהן גם להיות IO ע"י בוילר פלייט קוד שנכתב מאחורי הקלעים בשבילנו המתכנתים, אבל זה לא באמת פתרון.

כך או כך, פונקציות PF ו- IO לא בדיוק יכולות להתקיים אחת לצד השניה. הפתרון האולי אלגנטי ביותר יהיה לכתוב את ה IO הרלוונטי עם הכלים שיש לנו, ואת שאר הקוד להמשיך לפי קווי היסוד של תכנות פונקציונאלי (למי שמעוניין כמובן).
