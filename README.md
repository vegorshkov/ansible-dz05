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

перешел на ubuntu, так как на альт 10  не стартовали докер контейнеры.

на ubuntu провожу траблшутинг

![alt text](image-13.png)

Столкнулся с пролеммой, что молекула установилась в локальное окружение в то время как ансибл с библиотеками установлены в системное окружения.

Видны ошибки путей.
Как одно из решений, для последующего "анти-изменения" думаю разметить все в одном месте - в проекте.

Создадим единое окружение для molecule и ansible:

1) Создим виртуальное окружение в папке проекта
cd ~/STUDENT/ansible-dz05
python3 -m venv venv
source venv/bin/activate

2) Установим всё в одном месте
pip install ansible molecule molecule-plugins[docker]
ansible-galaxy collection install ansible.posix community.docker

3) Теперь всё в одном окружении, пути синхронизированы
molecule --version
which ansible  # покажет ./venv/bin/ansible

4) Устанавливаем molecule

pip install ansible molecule-plugins[docker]
# Optionally, install a verifier like testinfra
pip install pytest-testinfra ansible-lint yamllint


![alt text](image-14.png)
![alt text](image-15.png)
![alt text](image-16.png)
![alt text](image-17.png)

Инициализируем стандартный сценарий:
```
molecule init scenario default
```

![alt text](image-18.png)

Скорректировал структуру molecule.yml
![alt text](image-19.png)

![alt text](image-20.png)

![alt text](image-21.png)

Запустили molecule test
![alt text](image-22.png)

![alt text](image-23.png)

Пересоздаем виртуальное окружение:

![alt text](image-24.png)

![alt text](image-25.png)

![alt text](<Снимок экрана от 2026-03-01 13-33-35.png>)

![alt text](<Снимок экрана от 2026-03-01 13-39-58.png>)

![alt text](<Снимок экрана от 2026-03-01 14-01-49.png>)

![alt text](image-26.png)

Нашли что не ставиться нормально клик - фиксим.
![alt text](image-27.png)

Установка проходит, но требует sudo
![alt text](image-28.png)

Выполняется процесс установки:
![alt text](image-29.png)

Роад МАП:

Нашли самые сложные подводные камни:
Что уже преодолено:

-Конфликты версий Molecule и Ansible

-Проблема с http+docker

-Отсутствие Python в контейнере

-Неправильный порядок задач в роли

-Отсутствие sudo в контейнере

-Добавление репозитория ClickHouse

-Установка ClickHouse

![alt text](image-30.png)

Полный тест:

![alt text](image-31.png)

Полный вывод сценария: [text](roles/clickhouse/clickhouse_scenario.txt)


Осталось vector:

![alt text](image-32.png)

Команды:
```
  Уничтожить старые контейнеры
molecule destroy

  Создать заново
molecule create
  Подготовить контейнер (установить Python и зависимости)
molecule prepare

  Применить роль vector
molecule converge

  Проверить результат
molecule verify

  Если всё хорошо, можно запустить полный тест
molecule test
```
![alt text](image-33.png)

![alt text](image-34.png)

Запускаем полную проверку:
![alt text](image-35.png)

Корректирую и запускаем

![alt text](image-36.png)

![alt text](image-37.png)


Круто, Думал что мозг взорвется :-)


TOX

![alt text](image-39.png)

Фиксим:
![alt text](image-40.png)

![alt text](image-41.png)

Смотрим:

![alt text](image-42.png)

![alt text](image-43.png)

![alt text](image-44.png)

![alt text](image-45.png)


Vector работает, конфиг валидный, версия правильная!

ClickHouse роль:

![alt text](image-46.png)

    Устанавливает зависимости (git, nginx)

    Клонирует репозиторий Lighthouse

    Настраивает nginx для работы с Lighthouse

    Запускает и включает nginx

Сценарий тестирования (molecule/ubuntu):

    create - создаёт Docker контейнер с Ubuntu 22.04
    prepare - устанавливает Python и зависимости
    converge - применяет роль Lighthouse
    verify - проверяет:

        Процесс nginx запущен

        Файлы Lighthouse существуют

        Lighthouse отвечает на порту 8080

Проверка СТЕКА всех ролей:

![alt text](image-47.png)

Для памяти:
molecule test -s stack --destroy=never

![alt text](image-48.png)
![alt text](image-49.png)
![alt text](image-50.png)
![alt text](image-51.png)
![alt text](image-52.png)
![alt text](image-53.png)
![alt text](image-54.png)

Думаю что проще доустановить systems в контейнер для клика.  Иначе трабл с правами пользователя клик (хотя права повышал)