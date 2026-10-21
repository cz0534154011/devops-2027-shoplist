1)docker build -t greeter:v1 lessons/02-docker-basics/homework/greeter
2)docker run -d -p 9000 8000 --name greeter: greeter:v1
3)docker run -d -p 9000:8000 -e GREETING=Shalom --name greeter greeter:v1
4){"message": "Shalom, Rivka!"}
5)DOCKER  בודק האם יש שינויים שצורכים ממנו התקנה נוספת וא"כ הוא מתקין אותם היכן שצריך עדכון - הוא יעדכן
