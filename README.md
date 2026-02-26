Домашнее задание к занятию 5 «Тестирование roles»

Подготовка к выполнению

1) Установка molecule:

![alt text](image.png)
![alt text](image-1.png)

2) Установка podman, tox и несколькими пайтонами (3.7 и 3.9):
![alt text](image-2.png)
![alt text](image-3.png)
![alt text](image-4.png)

---=== Molecule ===---

molecule init scenario default --driver-name docker
default — имя нового сценария. Сценарий — это «набор инструкций», как тестировать роль.
--driver-name docker — говорит Molecule использовать Docker-контейнеры как виртуальные машины для тестирования.
![alt text](image-5.png)
![alt text](image-6.png)

Смотрим структуру:

![alt text](image-7.png)

Выполняем:

![alt text](image-8.png)

Вопрос 2:
Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи molecule init scenario --driver-name docker.

![alt text](image-9.png)
![alt text](image-10.png)
![alt text](image-11.png)

Тест пройден, преведущая конфигурация удалена через molecule destroy
![alt text](image-12.png)


