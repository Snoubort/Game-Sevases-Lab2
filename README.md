Основы работы с Unity
Отчет по лабораторной работе #1 выполнил(а):
- Кулаков Иван Александрович
- РИ300003

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * |   60 |
| Задание 2 | * |   20 |
| Задание 3 | * |   20 |

## Цель работы
Cоздание интерактивного приложения и изучение принципов интеграции в него игровых сервисов
## Задание 1
###По теме видео практических работ 1-5 повторить реализацию игры на Unity. Привести описание выполненных действий.
Ход работы:
- Создать новый проект из шаблона 3D – Core;
- Проверить, что настроена интеграция редактора Unity и Visual Studio Code (пункты 8-10 введения);
Скрин "Подготовка"
- Создать объект Plane;
Скрин plane
- Создать объект Cube;
Скрин cube
-	Создать объект Sphere;
Скрин Sphere
-	Установить компонент Sphere Collider для объекта Sphere;
-	Настроить Sphere Collider в роли триггера;
Скрин Sphere collider
-	Объект куб перекрасить в красный цвет;
Скрин Red cube
-	Добавить кубу симуляцию физики, при это куб не должен проваливаться под Plane;
Скрин Red cube phisics
- Написать скрипт, который будет выводить в консоль сообщение о том, что объект Sphere столкнулся с объектом Cube;

        '''C#
        public class AddCollider : MonoBehaviour
        {
            // Start is called before the first frame update
            void Start()
            {

            }

            // Update is called once per frame
            void Update()
            {

            }

            private void OnTriggerEnter(Collider other) {
                Debug.Log("Произошло столкновение с " + other.gameObject.name);
                other.gameObject.GetComponent<Renderer>().material.SetColor("_Color", Color.green); 
            }

            private void OnTriggerExit(Collider other) {
                Debug.Log("Завершили столкновение с " + other.gameObject.name);
                other.gameObject.GetComponent<Renderer>().material.SetColor("_Color", Color.red);
            }
        }
        '''

- При столкновении Cube должен менять свой цвет на зелёный, а при завершении столкновения обратно на красный.
Скрин Cube + sphere
Скрин Cube+sphere 2

## Задание 2
### В проект, выполненный в предыдущем задании, добавить систему проверки того, что SDK подключен (доступен в режиме онлайн и отвечает на запросы):
- Что произойдёт с координатами объекта, если он перестанет быть дочерним?
Координаты объекта изменятся с локальных на мировые
Скрин cube dom
Скрин Cube nod dom
- Создайте три различных примера работы компонента RigidBody?
Падение(гравитация), Существование как физической сущности(kinematic), Активация тригеров
Скрин Gravity, trigger detection, kinematic
Скрин Gravity, trigger, kinematic 2
Скрин Not detected

## Задание 3
### Реализуйте на сцене генерацию n кубиков. Число n вводится пользователем после старта сцены.
Создаём public переменную типа int
Считываем значение переменной при обновлении
Если значение - верное количество объектов - спавник кубики
Скрин Start spawn
Скрин End Spawn
        '''C#

        public class ReadLine : MonoBehaviour
        {
            public int n = -1;
            public GameObject PrefCub;
            public GameObject self;
            // Start is called before the first frame update
            void Start()
            {

            }

            // Update is called once per frame
            void Update()
            {
                if(n >=0){
                    for (int i = 0; i<n; i++){
                        Instantiate(PrefCub, self.transform.position, self.transform.rotation);
                    }
                    n = -1;
                }

            }
        }
    
        '''



