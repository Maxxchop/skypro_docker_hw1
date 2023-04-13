# skypro_docker_hw1

## Задание 1

Создайте контейнер с любой версией Python. 

Добейтесь того, чтобы внутри контейнера вы могли выполнять команды Python в консоли (набирать 2+2 и получать 4)

Находясь внутри контейнера:

- создайте файл main.py с простейшим кодом print(”Hello world”)
- запустите файл интерпретатором Python в контейнере (получен вывод Hello world)
- сохраните файл в какую-либо директорию на контейнере
- добейтесь того, чтобы после остановки и удаления контейнера с Python, ваша программа осталась на локальной машине.

```
  docker run -it -v C:\Users\Макс\Desktop\docker:/my_files python bash
  cd my_files/
  cat > main.py
  print("Hello, world!")
  {CTRL+D}
  python main.py
  exit
  docker rm 60e9fd9302e1
```

## Задание 2

- Создайте папку на локальном компьютере.
- Запустите контейнер с Postgres так, чтобы данные базы сохранялись в созданной выше папке.
- Создайте в Postgres-контейнере новую БД, в ней таблицу.
- Остановите и удалите контейнер.
- Создайте новый контейнер в Postgres и получите доступ к данным (БД и таблице), созданным в первом контейнере.

```
docker run --name postgres-first -e POSTGRES_PASSWORD=postgres -p 5432:5432 -e POSTGRES_DB=habrdb -e PGDATA=/var/lib/postgresql/data/pgdata -d -v C:\Users\Макс\Desktop\docker:/var/lib/postgresql/data postgres
docker exec -it postgres-first bash
> root@e5d64853a0cb:/#  psql -U postgres
>> postgres=# CREATE TABLE public.persons (id int PRIMARY KEY, lastName varchar(255), firstName varchar(255));
>> postgres=# INSERT INTO public.persons VALUES (1, 'Max', 'Chepurnov');
>> postgres=# exit
exit
docker stop postgres-first
docker rm postgres-first
docker run --name postgres-second -e POSTGRES_PASSWORD=postgres -p 5432:5432 -e POSTGRES_DB=habrdb -e PGDATA=/var/lib/postgresql/data/pgdata -d -v C:\Users\Макс\Desktop\docker:/var/lib/postgresql/data postgres
> root@8d19f48c1826:/# psql -U postgres
>> postgres=# SELECT * FROM public.persons;
```
