# שיעור 6 – תשובות

## 1. שמות ה-images ו-imagePullPolicy
השמות שנמצאים ב Compose הם נקבעים ע"פ שם התקיה שבה נמצא הקובץ  Compose  + שם השירות
וע"כ אם ניקח את השם הזה  וישתנה מיקום הקבצים בתקיה לא יהיה ניתן למצוא את ה image -וע"כ בונים לכל image שם ברור שתואם למה שכתוב ב Deployment
 וכן בלי `imagePullPolicy: Never`
אם ה image לא קיים Kubernetes ינסה להוריד מהאינטרנט והרי  shoplist-backend:local לא מופיע שם אז הסטטוס של pod יהיה ErrImagePull

## 2. פענוח ה-Secret
הפקודה שהורצה : kubectl get secret postgres-secret -n shoplist -o jsonpath="{.data.POSTGRES_PASSWORD}"
ופוענחה ע"י:  [Text.Encoding]::UTF8.GetString([Convert]::FromBase64String("c2hvcHBhc3M="))
הסיסמא היא רק מקודדת ולא מוצפנת - זה אומר שבקלות ניתן להגיע לסיסמא המקורית
(ניתן להגביל הרשאות גישה לסיסמא)

## 3. איזה קובץ שומר על המוצרים
הקובץ שאחראי לכך הוא: postgres-pvc.yml - הוא שומר את הנתונים בדיסק - שנשמר גם אם ה pod קורס...
הפקודה שתמחוק אותם בכל זאת : kubectl delete pvc postgres-pvc -n shoplist

## 4. מה מחליף את depends_on
בתוך backend מוגדר : readinessProbe שבו בודקים שוב ושוב את /health
במידה וה postgres עוד לא עלה ה pod מסומן כ 0/1 - לא מוכן והוא לא נכנס ל endpoints
וכאשר postgres עולה הבדיקה /health מצליחה ה pod  מסומן כ 1/1 ו Kubernetes מוסיף אתו ל endpoints
Compose קובע סדר הפעלה ו Kubernetes בודק כל הזמן ומחכה שהכול יהיה מוכן

## 5. ד8: selector שגוי
הנורת backend אדומה ואין מוצרים כי אין מי שיקשר בין הנתונים לדפדפן אין אף pod מקושר ברשימה והבקשה לא מגיעה לשום מקום 

## 6. שינוי ב-ConfigMap שלא נקלט
הערכים מ ConfigMap מוגדרים ב backend כמשתני סביבה - אותם התכנית קוראת רק בפעם הראשונה
הפתרון הוא להעלות את ה pod שוב פעם - ה pod יקרא שוב את ConfigMap ואז השינוי יתעדכן

## 7. כמה פעמים כתוב shoplist
צריך לשנות 13 פעמים ב 13 קבצים שונים - דבר לא הגיוני ומסורבל מאד - מוכרחים ל"ייצא" את השם של הפרויקט למקום אחד ע"מ שתהיה אפשרות שזה יהיה דינאמי...

# ד11 בונוס
1- קודם עלה ונוצר ה pod החדש ורק כאשר הוא היה מוכן ה pod הישן התחיל להסגר (rolling update)
2-לא האפליקציה כל הזמן עבדה - כי ה pod  הישן לא נסגר עד שלא הי pod  חדש מוכן