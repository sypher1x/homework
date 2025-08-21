Задание 1.

1. Открываем файл cronetab на редактирование

    sudo crontab -e

<img width="501" height="25" alt="image" src="https://github.com/user-attachments/assets/8a5d0b7b-8ad2-417c-869d-84956d43cca4" />

3. Прописываем команду:
   
   0 16 1 * * apt-get clean && apt-get autoclean
   
   Где 0 - это минута, 16 часы, 1 число месяца, * любой месяц, * любой день недели
   
   apt-get clean — очищает весь кэш загруженных .deb файлов
   
   apt-get autoclean — удаляет устаревшие пакеты (те, которых уже нет в репозиториях и которые всё равно нельзя установить)

<img width="580" height="418" alt="image" src="https://github.com/user-attachments/assets/73dc7aac-a6f7-45b3-89bc-ddf968130b3e" />
