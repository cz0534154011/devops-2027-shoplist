# בחירת Image בסיס עם גרסת Python מוגדרת
FROM python:3.12-slim
# קביעת תיקיית העבודה של היישום
WORKDIR /app
# העתקת קובץ התלויות לתיקיית העבודה בתוך ה־Image
COPY requirements.txt /app/
# התקנת התלויות מתוך קובץ הדרישות
RUN pip install -r requirements.txt
# העתקת קובץ האפליקציה לתיקיית העבודה
COPY app.py /app/
# תיעוד הפורט שבו האפליקציה מאזינה
EXPOSE 8000
# הפעלת שרת Flask בעת הפעלת ה־Container
CMD ["python", "app.py"]
