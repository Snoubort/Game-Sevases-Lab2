Основы работы с Unity
Отчет по лабораторной работе #1 выполнил(а):
- Кулаков Иван Александрович
- РИ300003

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * |   60 |
| Задание 2 | * |   20 |
| Задание 3 | # |   20 |

## Цель работы
Cоздание интерактивного приложения и изучение принципов интеграции в него игровых сервисов
## Задание 1
### По теме видео практических работ 1-5 повторить реализацию игры на Unity. Привести описание выполненных действий.
Ход работы:
- Изменяем название сцены;
- Создаём сферу, вытягием ём по вертикали, тем самым делая яйцо, создаём материал;
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")
- Импортируем пак драконов;
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")
- Делаем дубликат модели дракона;
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")
- Создаём новый контроллер анимации для дракона, редактируем, вешаем новый котроллер на дубликат дракона;
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")
- Создаём щит и материал для него;
- Импортируем огненные материалы, добавляем текстуры для яца и земли;
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")
- Создаём скрипт движения для дракона, делаем так, чтоб он случайно менял направление движения, так же делаем, чтоб дракон с определённым интервалом спавнил яйца из префаба;


        public class EnemyDragon : MonoBehaviour
        {
            public GameObject dragonEggPrefab;
            public float speed = 1;
            public float timeBetweenEggDrops = 1f;
            public float leftRightDistance = 10f;
            public float chanceDirection = 0.1f;
            // Start is called before the first frame update
            void Start()
            {
                Invoke("DropEgg", 2f);
            }

            void DropEgg(){
                Vector3 myVector = new Vector3(0.0f, 5.0f, 0.0f);
                GameObject egg = Instantiate<GameObject>(dragonEggPrefab);
                egg.transform.position = transform.position + myVector;
                Invoke("DropEgg", timeBetweenEggDrops);
            }

            // Update is called once per frame
            void Update()
            {
                Vector3 pos = transform.position;
                pos.x += speed * Time.deltaTime;
                transform.position = pos;

                if (pos.x < -leftRightDistance){
                    speed = Mathf.Abs(speed);
                }
                else if (pos.x > leftRightDistance){
                    speed = -Mathf.Abs(speed);
                }
            }

            private void FixedUpdate() {
                if (Random.value < chanceDirection){
                    speed *= -1;
                }
            }
        }
        

- Создём скрипт для яйца, при падении ниже определённого Y яйцо уничтожается, при касание же земли яйцо становится невидимым и запсукает систему частиц;
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")


        public class DragonEgg : MonoBehaviour
        {
            public static float bottomY = -30f;
            // Start is called before the first frame update
            void Start()
            {

            }

            private void OnTriggerEnter(Collider other) {
                ParticleSystem ps = GetComponent<ParticleSystem>();
                var em = ps.emission;
                em.enabled = true;

                Renderer rend;
                rend = GetComponent<Renderer>();
                rend.enabled = false;
            }

            // Update is called once per frame
            void Update()
            {
                if(transform.position.y < bottomY){
                    Destroy(this.gameObject);
                }
            }
        }
        

Скрин Cube + sphere
Скрин Cube+sphere 2

- Создаём спавн нескольких щитов разного радиуса из префаба
![Скрин интеграция](https://github.com/Snoubort/Game-servases-lab1/blob/main/%D0%9F%D0%BE%D0%B4%D0%B3%D0%BE%D1%82%D0%BE%D0%B2%D0%BA%D0%B0.PNG?raw=true "Интеграция")


        public class DragonPicker : MonoBehaviour
        {
            public GameObject energyShieldPrefab;
            public int numEnergyShield = 3;
            public float energyShieldBottomY = -6f;
            public float energyShieldRadius = 1.5f;
            // Start is called before the first frame update
            void Start()
            {
                YandexSDK sdk = YandexSDK.instance;
                print(sdk);
                print("SDK загружен");
                for (int i = 1; i<= numEnergyShield; i++){
                    GameObject tShieldGo = Instantiate<GameObject>(energyShieldPrefab);
                    tShieldGo.transform.position = new Vector3(0, energyShieldBottomY, 0);
                    tShieldGo.transform.localScale = new Vector3(1*i, 1*i, 1*i);
                }
            }

            // Update is called once per frame
            void Update()
            {

            }
        }


## Задание 2
### В проект, выполненный в предыдущем задании, добавить систему проверки того, что SDK подключен (доступен в режиме онлайн и отвечает на запросы):
- Импортируем в Unity YandexSDK
- Инициализируем SDK при старте игры
- Добавляем на сцену объект SDK
- Выводим в консолько сам SDK, если запуск был не успешен, то выведется ошибка, смотри код в предыдущем пункте
        
        
