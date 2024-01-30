# Containerization_final_task
Задание 1:
1) создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД
2) далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab)
3) по итогу на каждой ноде должно быть по 2 работающих контейнера
4) выводы зафиксировать


Решение: для того чтобы пользоваться swarm, надо запомнить несколько типов сущностей:
- Node - это наши виртуальные машины, на которых установлен docker. Есть manager и workers ноды. Manager нода управляет workers нодами. Она отвечает за создание/обновление/удаление сервисов на workers, а также за их масштабирование и поддержку в требуемом состоянии. Workers ноды используются только для выполнения поставленных задач и не могут управлять кластером.
- Stack - это набор сервисов, которые логически связаны между собой. По сути это набор сервисов, которые мы описываем в обычном compose файле. Части stack (services) могут располагаться как на одной ноде, так и на разных.
- Service - это как раз то, из чего состоит stack. Service является описанием того, какие контейнеры будут создаваться. Если вы пользовались docker-compose.yaml, то уже знакомы с этой сущностью. Кроме стандартных полей docker в режиме swarm поддерживает ряд дополнительных, большинство из которых находятся внутри секции deploy.
- Task - это непосредственно созданный контейнер, который docker создал на основе той информации, которую мы указали при описании service. Swarm будет следить за состоянием контейнера и при необходимости его перезапускать или перемещать на другую ноду.
Таким образом, создаем два ДК-файла, затем компилируем node и совершаем проверку. После чего требуется задеплоить в Stack. Получаем ошибку: "версия данного файла не поддерживается", продолжаем делать. Затем запускаем первый ДК-файл и осуществляем проверку. После чего осуществляем запуск второго  ДК-файла. Воспользуемся функционалом Docker Swarm и создадим дубликат одного и того же образа.Затем тоже самое сделаем  с другими образами, далее повязать их сетью и получить полные дубликаты: БД + WEB.
Второй вариант решение(с использованием второй node): подключаемся по SSH ко второй VM и устанавливаем swarm соединение, предварительно инициализировав manager node.
Затем запускаем первый ДК-файл на manager node и осуществляем проверку. После чего запускаем второй ДК-файл на worker node и также осуществляем проверку.
Пытаемсязапустить первый ДК-файл на node worker. Проверяем, все работает, ошибки дублирования не произошло т. к имена image разные(ДК-файлы в разных директориях находятся на VM). Запускаем на manager node второй ДК-файл и получаем четыре работающих контейнера на одной node и на другой (не считая hello-word). Если бы расположение папок было идентично на manager node и worker node, пришлось бы создавать реплики образов и обвязывать их сетью. Тем временем наблюдаем за статусом worker node, он "упал"(Отказался идти на повышение). Переподключаем swarm соединение. Создаем еще реплики образов и видим как система их перераспределяет. Для того чтобы сервисы распределять на определенные ноды необходимо применять labels.

 ![1](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/1.png)
![2](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/2.png)
![3](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/3.png)
![4](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/4.png)
![5](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/5.png)
![6](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/6.png)
![7](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/7.png)
![8](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/8.png)
![9](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/9.png)
![10](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/10.png)
![11](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/11.png)
![12](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/12.png)
![13](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/13.png)
![14](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/14.png)
![15](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/15.png)
![16](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/16.png)
![17](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/17.png)
![18](https://github.com/natalykozhevnikova/Containerization_final_task/blob/main/18.png)
