<img width="522" height="224" alt="image" src="https://github.com/user-attachments/assets/ffb8b1c3-2a7f-4d48-b531-2f6740d5cd2e" />Сначала попытался выполнить задание через VirtualBox (Ubunty 24.04)

По итогу не получилось, потому что mongodb версии 6+ требует AVX для процессора, в VirtualBox вроде настройку включил,
но не заработало.

  Illegal instruction (core dumped)

ИИ подсказал, что можно поставить mongodb 5 версии, но заморачиваться не стал в этот раз - создал виртуалку
на Яндексе.

1. Импортировал ключ:
  curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
  sudo gpg --dearmor -o /usr/share/keyrings/mongodb-server-7.0.gpg

2. Добавил репозиторий:
  echo "deb [signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg] \
  https://repo.mongodb.org/apt/ubuntu noble/mongodb-org/7.0 multiverse" \
  | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list

3. Обновил список пакетов и установил mongodb:
  sudo apt update
  sudo apt install -y mongodb-org

4. Проверил:
 mongod --version
 <img width="599" height="234" alt="image" src="https://github.com/user-attachments/assets/026e78bd-f3b0-4ae4-89e5-5c79efd9a024" />

 systemctl status mongod
 <img width="749" height="166" alt="image" src="https://github.com/user-attachments/assets/2cbfd3c6-170f-40c0-8865-ca1a99b4e40f" />

5. Подключился к mongodb:
  mongosh
  <img width="965" height="129" alt="image" src="https://github.com/user-attachments/assets/d7796847-5100-49c9-9b20-eb41b7b098dc" />

6. Создал БД "homework"
  use homework

7. Тестовый файл
  db.homework_5.insertOne({name: "5"})

8. Пользователь с доступом на чтение:
  db.createUser({
  user: "manager_5",
  pwd: "123",
  roles: [
    { role: "read", db: "homework" }
  ]
  })
  <img width="352" height="132" alt="image" src="https://github.com/user-attachments/assets/45ba4b46-1bfa-4ca5-a62c-2d8f33777673" />

7. Выходим и проверяем:
  exit
  mongosh -u manager_5 -p 123 --authenticationDatabase "homework"

  (Долго не мог понять, почему даю роль только на read - и всё равно остается возможность делать запись. Оказалось, нужно 
  добавить авторизацию в mongod.conf
  
        security:
      authorization: enabled
      
  И всё заработало)

  use homework
  db.homework_5.find()
  <img width="598" height="59" alt="image" src="https://github.com/user-attachments/assets/3cd3e31d-67c5-491b-83c4-9613d9e6cf7f" />

  db.homework_5.insertOne({name: "fail"})
  <img width="960" height="95" alt="image" src="https://github.com/user-attachments/assets/3b43a48e-fee2-4925-a669-1d35b33c841a" />
