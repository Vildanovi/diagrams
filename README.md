# Проект базы данных фильмов

## Фильмы
#### База фильмов с параметрами и возможностью фиксации лайков пользователей.
#### У фильма может быть сразу несколько жанров, а у поля — несколько значений.
#### Рейтинг Motion Picture Association (сокращённо МРА) - определяет возрастное ограничение для фильма.

## Пользователи
#### Содержат данные пользователей, которые могут добавлять друг друга в друзья.
#### При добавлении в друзья есть статусы
- `неподтверждённая` — когда один пользователь отправил запрос на добавление другого пользователя в друзья,
- `подтверждённая` — когда второй пользователь согласился на добавление.

> База данных сформирована на сайте dbdiagram.io и доступна по ссылке
> 
> [Перейти](https://dbdiagram.io/d/filmorate-6542bee07d8bbd64654e0274)


[Посмотреть в файле *.png](src/main/resources/images/DB_Filmorate.png)
![Превью базы данных](src/main/resources/images/DB_Filmorate.png)

## Базовые запросы
#### Получить список фильмов отсортированных по дате релиза
```roomsql
SELECT film.name,
       release_date
FROM film
ORDER BY release_date DESC; 
```


- #### Получить список фильмов отсортированных по дате релиза за выбранный период
```roomsql
SELECT release_date,
       SUM(total)
FROM film
WHERE release_date between '2022-01-01' AND '2023-01-01'
ORDER BY release_date DESC; 
```

- #### Получить имена, логины и email пользователей c необходимым лимитом
```roomsql
SELECT user.name,
       user.email,
       user.login
FROM user
LIMIT 10;
```

- #### Получить количество друзей пользователей
```roomsql
SELECT name, 
       t.friends_count 
FROM user
JOIN (
SELECT user1_id, 
       count(*) AS friends_count
FROM user_friends
GROUP BY user1_id
) AS f
ON user.id = f.user1_id;
```