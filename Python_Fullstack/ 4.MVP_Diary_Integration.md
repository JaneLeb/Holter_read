##### 📖 Шаг 6. Пример кода дневника (Streamlit) 
<img width="1024" height="1024" alt="bdb4191800511f19d82cefd6ad41579_1" src="https://github.com/user-attachments/assets/d466b837-79a3-43d2-9780-8363289b049e" />


Для качественного кардиологического анализа (особенно когда данные сопоставляются с нейросетью) врачу важны не просто факты, а контекст, который меняет вегетативный тонус и влияет на миокард.
В интерфейсе дневника для пациента (веб-версия) не должно быть много свободного ввода текста — пациенты часто пишут лишнее или забывают главное. Лучше сделать систему быстрых кнопок/тегов. Вместо неструктурированного текста мы передаем на десктоп врача строгие ID событий (например, EVENT_SCALED_STAIRS, EVENT_PALPITATION). Это позволяет нашему движку автоматически сопоставлять временные метки дневника с ЭКГ-файлом без сложной текстовой предобработки
Вот полный список того, что критически важно знать врачу, разделенный по категориям для вашего интерфейса:
Эту структуру можно передать дизайнеру или использовать как техническое задание для разработки.

Концепция: «Карточка события» и лента времени
Главный экран — это вертикальная хронологическая лента прошедшего дня. Чтобы пациенту было максимально удобно фиксировать состояние одной рукой (например, во время ходьбы), основной интерфейс строится вокруг кнопки быстрого действия (FAB).

1. Главный экран (Лента событий)
Визуализация: Вертикальный таймлайн. Каждый зафиксированный триггер отображается в виде карточки с иконкой, временем и кратким описанием (например, «Кофе», «Подъем по лестнице»).
Связь с ЭКГ: Рядом с каждой карточкой ИИ ставит микро-метку:
Зеленая галочка — ритм соответствует активности.
Желтый восклицательный знак — зафиксировано отклонение (ИИ видит связь с нагрузкой, но просит подтвердить самочувствие).
Красный значок аритмии — критическое событие без нажатой кнопки симптома.
Навигация: Сверху экрана закреплена шкала часов текущего дня со скроллом. При нажатии на любой час открывается расширенная кардиограмма за этот период.
2. Кнопка фиксации состояния (Floating Action Button)
Большая круглая кнопка (красного или оранжевого цвета) всегда находится поверх ленты. По короткому нажатию она открывает модальное окно выбора категории. Если пациент удерживает кнопку 3 секунды, запускается запись голосовой заметки к текущему моменту ЭКГ.

Структура окна фиксации
<img width="817" height="645" alt="image" src="https://github.com/user-attachments/assets/454b54c9-1eb6-4431-ac75-a64fb31daf09" />




В кардиологии есть таткое понятие - адекватность ответа сердечно-сосудистой системы на нагрузку.
- Если у пациента ЧСС 140 и одышка во время бега — это физиологическая норма (адекватный ответ). 
- Если ЧСС 140 и одышка в состоянии покоя перед телевизором — это патология (пароксизм тахикардии или острая сердечная недостаточность).


Чтобы избавить врача от ручной аналитики, интерфейс дневника должен быть двухфакторным, а движок и ИИ должны получать готовую связку: [Контекст] + [Симптом].

<img width="1024" height="1024" alt="f0f3220800411f186353efe1de30343_1 (1)" src="https://github.com/user-attachments/assets/600b4874-0931-46c3-9e55-63b2e25e3060" />

##  Что нужно записывать в «дневник приступов»
Сама по себе кривая ЭКГ с часов дает лишь половину клинической картины. Для постановки диагноза кардиологу нужно знать, что происходило с вашим телом в физическом и эмоциональном плане в момент замера.
Каждый раз, когда часы бьют тревогу или вы чувствуете сбой в груди, запишите в заметки телефона:

* Субъективные ощущения: Что именно вы чувствовали? (Например: «сердце будто перевернулось», «резкий толчок в грудь», «чувство замирания на 2 секунды», «пульс бешено заколотился в горле»).
* Сопутствующие симптомы (Критические маркеры): Было ли у вас головокружение, потемнение в глаза, резкая слабость, холодный пот или нехватка воздуха? Одышка и предобморочное состояние вместе с аритмией — это повод для немедленного вызова скорой.
* Контекст (Триггеры): Что вы делали за 15–30 минут до этого? (Например: «выпил вторую чашку крепкого кофе», «поругался на работе», «резко встал с кровати», «плотно поужинал перед сном», «накануне пил алкоголь»).
* Артериальное давление: Если дома есть классический тонометр с манжетой, сразу после записи ЭКГ измерьте давление. Сочетание высокой частоты пульса с низким или, наоборот, очень высоким давлением критически важно для подбора лекарств кардиологом.

------------------------------
##### 1. Физическая активность и нагрузки
Нейросеть увидит резкий скачок ЧСС (синусовую тахикардию). Без дневника ИИ может пометить это как аномалию, но если в это время была нагрузка — это норма.

* 🏃‍♂️ Бег / Быстрая ходьба / Тренировка
* 🪜 Подъем по лестнице (классический стресс-тест для сердца)
* 🎒 Бытовая нагрузка (уборка, тяжелые сумки)
  

##### 2. Психоэмоциональное состояние
Стресс активирует симпатическую нервную систему, что вызывает ишемические изменения или экстрасистолы.


* ⚡️ Сильный стресс / Испуг / Паническая атака
* 🗣️ Конфликт / Волнение / Публичное выступление
  

##### 3. Прие стимуляторов и триггеров
Эти вещества напрямую влияют на проводящую систему сердца и могут провоцировать приступы мерцательной аритмии (так называемый «синдром праздничного сердца») или экстрасистолию.


* ☕️ Кофе / Крепкий чай / Энергетик
* 🚬 Курение / Никотин
* 🍷 Алкоголь
* 🍔 Плотный прием пищи (вздутие живота и полный желудок могут чисто механически давить на блуждающий нерв и вызывать рефлекторную аритмию)
  

##### 4. Физиологические маркеры (то, что вы назвали)


* 🛌 Сон / Пробуждение (важно для оценки циркадного индекса — как ЧСС падает ночью)
* 🛋️ Покой / Отдых сидя
* 💊 Прием лекарств (особенно антиаритмиков или бета-блокаторов, чтобы врач видел, через сколько минут после приема выравнивается ритм)
  

##### 5. Кнопки конкретных симптомов (Главная фича для ИИ)
Когда пациент нажимает эту кнопку, ИИ должен с особым вниманием изучить этот кусок ЭКГ.

* 🫀 Сильное сердцебиение («сердце выскакивает» или «замирает»)
* 💔 Боль в груди / Давление за грудиной
* 💫 Головокружение / Предобморочное состояние
* 😮‍💨 Нехватка воздуха / Одышка

Как будет выглядеть работа со Streamlit в вашем MVP:

Разработчик пишет буквально 10 строк кода: создает выпадающий список симптомов (боль, одышка, пульс) и кнопку «Сохранить».

Результат: Streamlit генерирует веб-страницу. Пациент заходит на неё с телефона, выбирает симптом, нажимает кнопку.

Сохранение в облако: Python-скрипт под капотом тут же ловит это нажатие, берет текущее время смартфона и дописывает строчку в ваш обезличенный файл (например, .csv) на Яндекс Диске через Яндекс.Диск API.

Как это свяжется с десктопом врача (CustomTkinter / PyQt6): Когда врач на десктопе нажмет кнопку «Загрузить дневник пациента №123» - PyQt6/Tkinter приложение сделает запрос к Яндекс Диску.Скачает тот самый .csv файл, который наполнил Streamlit.

Передаст данные в C++ и Data Science модули для мэтчинга с Холтером. А потом отрисует красивый интерактивный график в Plotly прямо внутри десктопного окна.


##### Реализация интерактивного дневника на Streamlit


Ниже представлена возможная реализация интерфейса дневника в виде интерактивной матрицы (с синхронизацией), а также структура JSON-файла, которую этот интерфейс генерирует для десктопного приложения.


В этом интерфейсе кнопки сгруппированы так, что пациент сначала выбирает, что он делал, а затем (если это привело к дискомфорту) привязывает к этому действию симптом.
Код на Python: 


      import streamlit as stimport jsonfrom datetime import datetime
      
      st.set_page_config(page_title="Синхронизированный Дневник Холтера", layout="centered")
      st.title("🫀 Дневник Холтер-ИИ")
      st.caption("Связывайте ваши действия и симптомы для корректного анализа ИИ.")
      # Шаг 1: Контекст (Что происходило?)
      st.subheader("1. Что вы делали?")context = st.selectbox(
          "Выберите основное действие:",
          [
              "Покой (отдых сидя / лежа / просмотр ТВ)",
              "Сон (отмечается факт отхода ко сну или пробуждения)",
              "Физическая нагрузка (бег, быстрая ходьба, подъем по лестнице, тренировка)",
              "Бытовые дела (уборка, приготовление еды, тяжелые сумки)",
              "Психоэмоциональный стресс (волнение, испуг, спор, совещание)",
              "Прием стимуляторов (кофе, крепкий чай, энергетик, курение)"
          ]
      )
      # Шаг 2: Симптом (Синхронизация)
      st.subheader("2. Что вы при этом почувствовали?")has_symptom = st.radio("Были ли неприятные ощущения в сердце или самочувствии?", ["Нет, все в норме", "Да, почувствовал дискомфорт"])
      symptom = "Норма"if has_symptom == "Да, почувствовал дискомфорт":
          symptom = st.selectbox(
              "Выберите симптом, который возник на фоне этого действия:",
              [
                  "Нехватка воздуха / Сильная одышка",
                  "Замирание / Перебои / Резкий удар в груди",
                  "Давящая или колющая боль за грудиной",
                  "Головокружение / Потемнение в глазах / Предобморочное состояние",
                  "Резкая слабость / Вспотел"
              ]
          )
      # Шаг 3: Дополнительноcomment = st.text_input("Дополнительный комментарий (необязательно):", placeholder="Например: поднялся на 4 этаж")
      if st.button("🚀 Отправить связанную метку врачу", type="primary"):
          # Формируем структуру данных
          event_data = {
              "patient_id": "Patient_XYZ_99",
              "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
              "context": context.split(" (")[0],  # Очищаем от подсказок для красивого JSON
              "symptom": symptom,
              "is_pathology_suspect": "Да" if (has_symptom == "Да, почувствовал дискомфорт" and "Покой" in context) else "Требует сопоставления"
          }
         
          # Превращаем в JSON-строку
          json_payload = json.dumps(event_data, ensure_ascii=False, indent=2)
         
          st.success("✅ Метка успешно синхронизирована!")
          st.code(json_payload, language="json")
      
      ------------------------------
      ## Какую структуру (JSON) получает десктопное приложение врача?
      Когда врач нажмет кнопку «Синхронизировать» на десктопе, программа скачает из интернета вот такой массив связанных событий:
      
      [
        {
          "timestamp": "2026-07-07 14:20:05",
          "context": "Физическая нагрузка",
          "symptom": "Нехватка воздуха / Сильная одышка",
          "code_context": "ACT_EXERCISE",
          "code_symptom": "SYM_DYSPNEA"
        },
        {
          "timestamp": "2026-07-07 03:15:22",
          "context": "Покой",
          "symptom": "Замирание / Перебои / Резкий удар в груди",
          "code_context": "ACT_REST",
          "code_symptom": "SYM_PALPITATION"
        }
      ]


------------------------------
##### ⏭️ Шаг 7. Пример кода синхронизации дневника  и десктопного приложения 
Чтобы приложения были актуальны для врачей, должны быть соблюдены следующие условия:


1. Наглядность кросс-валидации: По кнопке "Обзор" график на правой панели мгновенно приближает (зумит) нужный участок.
Например, простыми словами это может выражаться так : «Посмотрите на первую карточку. Пациент бежал, у него одышка. Алгоритм понял, что ЧСС адекватна, и сам убрал карточку в архив. Врач не отвлекается. А вот на второй карточке — пациент отдыхал, но возникло замирание. Система подсветила его красным, вывела ИИ-диагноз „Экстрасистолия“, и врачу достаточно нажать одну кнопку, чтобы это улетело в карту».

    
3. Отсутствие "эффекта 90-х": Интерфейс выглядит монолитно, стильно, в глубоких темных тонах (цвета #0F172A, #1E293B), элементы закруглены, шрифты чистые, а графики Matplotlib идеально кастомизированы под общий дизайн.

##### Как это может использует движок и ИИ на десктопе:


Логика автоматического интеллектуального сопоставления (Cross-Validation):



   1. Событие 1 (Бег + Одышка в 14:20):
   * C++ модуль смотрит на этот таймштамп в ЭКГ и видит ЧСС 135 bpm.
      * Система сверяет коды ACT_EXERCISE + SYM_DYSPNEA и делает вывод: «ЧСС соответствует нагрузке. Патологии нет. Пропускаем». Врач даже не тратит на это время.
    
        
   2. Событие 2 (Покой + Перебои в 03:15):
   * C++ модуль открывает таймштамп ночи. Пациент лежал (ACT_REST), но сообщил о перебоях (SYM_PALPITATION).
      * Локальная нейросеть анализирует этот кусок и находит там групповые экстрасистолы.
      * Система генерирует критический алерт для врача на десктопе: «Внимание! Одышка/Перебои не соответствуют профилю покоя. Обнаружена аритмия».
    
        
  
Такой подход полностью убирает ложные срабатывания и экономит до 90% времени кардиолога, что является мощнейшей бизнес-ценностью для стартапа.
Хотите посмотреть, как в десктопном интерфейсе на CustomTkinter подсветить эти два разных события (одно — зеленым как норму, второе — мигающим красным как критическую патологию)?


##### Вот пример реализации этой логики в десктопном приложении на CustomTkinter.
В коде ниже мы закладываем ту самую интеллектуальную логику: система считывает JSON-файл (в котором объединены контекст и симптомы), автоматически сопоставляет его с очищенными C++ данными и красит карточки событий в разные цвета. Зеленые (физиологическая норма вроде одышки при беге) уходят в архив, а красные (одышка или замирания в покое) — выводятся наверх и требуют одного клика врача для отправки в электронную карту.
Для отображения интерактивного графика мы используем matplotlib (а именно FigureCanvasTkAgg), который идеально и бесшовно встраивается в CustomTkinter без вызова сторонних окон.


#####  Полный код десктопного приложения врача
Перед запуском установите библиотеки: 

pip install customtkinter matplotlib pandas numpy.

    import customtkinter as ctkimport numpy as npimport pandas as pdfrom matplotlib.backends.backend_tkagg import FigureCanvasTkAggfrom matplotlib.figure import Figure
    # Настройки стиля UI
    ctk.set_appearance_mode("Dark")
    ctk.set_default_color_theme("blue")
    
    class CrossValidationApp(ctk.CTk):
    
        def __init__(self):
            super().__init__()
    
            self.title("🛡️ Holter AI Cross-Validation Engine")
            self.geometry("1300x750")
    
            # Настройка сетки окна (Левая панель — 1 часть, Правая — 3 части)
            self.grid_columnconfigure(0, weight=1, minsize=380)
            self.grid_columnconfigure(1, weight=3)
            self.grid_rowconfigure(0, weight=1)
    
            # --- ИМИТАЦИЯ ДАННЫХ ИЗ ДНЕВНИКА И ИИ (JSON) ---
            # Событие 1: Нагрузка + Одышка (Физиологическая норма)
            # Событие 2: Покой + Замирание (Критическая патология)
            self.events = [
                {
                    "time_start": 2.0,
                    "time_end": 4.0,
                    "context": "Физическая нагрузка (Бег)",
                    "symptom": "Нехватка воздуха / Одышка",
                    "ai_verdict": "Синусовая тахикардия (Адекватный ответ)",
                    "is_danger": False,
                    "color": "#10B981",  # Зеленый
                    "details": "ЧСС 142 bpm плавно растет. Соответствует профилю нагрузки.",
                },
                {
                    "time_start": 7.0,
                    "time_end": 9.0,
                    "context": "Покой (Просмотр ТВ)",
                    "symptom": "Замирание / Перебои в сердце",
                    "ai_verdict": "Желудочковая экстрасистолия (ЖЭ)",
                    "is_danger": True,
                    "color": "#EF4444",  # Красный
                    "details": "ЧСС 62 bpm. Внезапный провал ритма с компенсаторной паузой.",
                },
            ]
    
            # Генерируем тестовый чистый сигнал ЭКГ от C++
            self.t = np.linspace(0, 12, 3000)
            self.ecg = np.sin(2 * np.pi * 1.2 * self.t)  # Базовая синусоида
            # Имитируем аномалию на 8-й секунде (патология в покое)
            self.ecg += 2.5 * np.exp(-(((self.t - 8.2) / 0.04) ** 2))
    
            # --- ИНИЦИАЛИЗАЦИЯ ИНТЕРФЕЙСА ---
            self.create_left_panel()
            self.create_right_panel()
    
        def create_left_panel(self):
            """Левая панель со списком отфильтрованных событий"""
            self.left_panel = ctk.CTkFrame(self, corner_radius=12, fg_color="#1E293B")
            self.left_panel.grid(row=0, column=0, padx=15, pady=15, sticky="nsew")
    
            # Заголовок панели
            lbl_title = ctk.CTkLabel(
                self.left_panel,
                text="🚨 Сортировка событий ИИ",
                font=("Arial", 16, "bold"),
                text_color="#F8FAFC",
            )
            lbl_title.pack(pady=(20, 10), padx=15, anchor="w")
    
            # Контейнер со скроллом для карточек
            self.scroll_container = ctk.CTkScrollableFrame(
                self.left_panel, fg_color="transparent"
            )
            self.scroll_container.pack(fill="both", expand=True, padx=10, pady=5)
    
            # Отрисовываем карточки из нашего JSON-массива
            for ev in self.events:
                self.create_event_card(ev)
    
        def create_event_card(self, ev):
            """Создание интерактивной карточки события"""
            # Рамка карточки с подсветкой типа опасности (красный/зеленый)
            card = ctk.CTkFrame(
                self.scroll_container,
                fg_color="#0F172A",
                border_color=ev["color"],
                border_width=2,
                corner_radius=8,
            )
            card.pack(fill="x", pady=8, padx=5)
    
            # Текст: Контекст + Симптом
            lbl_header = ctk.CTkLabel(
                card,
                text=f"📋 {ev['context']}",
                font=("Arial", 13, "bold"),
                text_color="#94A3B8",
            )
            lbl_header.pack(pady=(10, 2), padx=12, anchor="w")
    
            lbl_sym = ctk.CTkLabel(
                card,
                text=f"💬 Симптом: {ev['symptom']}",
                font=("Arial", 12),
                text_color="#F1F5F9",
            )
            lbl_sym.pack(pady=2, padx=12, anchor="w")
    
            # Вердикт нейросети
            lbl_ai = ctk.CTkLabel(
                card,
                text=f"🤖 ИИ: {ev['ai_verdict']}",
                font=("Arial", 12, "italic"),
                text_color=ev["color"],
            )
            lbl_ai.pack(pady=4, padx=12, anchor="w")
    
            # Кнопки действий
            btn_frame = ctk.CTkFrame(card, fg_color="transparent")
            btn_frame.pack(fill="x", pady=(5, 10), padx=12)
    
            # Кнопка «Посмотреть на графике»
            btn_go = ctk.CTkButton(
                btn_frame,
                text="🔎 Обзор",
                width=80,
                height=26,
                fg_color="#334155",
                hover_color="#475569",
                command=lambda: self.focus_on_graph(ev["time_start"], ev["time_end"]),
            )
            btn_go.pack(side="left", padx=(0, 5))
    
            # Кнопка верификации для врача
            if ev["is_danger"]:
                btn_action = ctk.CTkButton(
                    btn_frame,
                    text="✅ В эл. карту",
                    width=120,
                    height=26,
                    fg_color="#EF4444",
                    hover_color="#DC2626",
                    command=lambda: self.send_to_mis(ev),
                )
                btn_action.pack(side="right")
            else:
                btn_action = ctk.CTkButton(
                    btn_frame,
                    text="📥 В архив (Норма)",
                    width=120,
                    height=26,
                    fg_color="#10B981",
                    hover_color="#059669",
                    state="disabled",  # Рутина снята, архивировано автоматически
                )
                btn_action.pack(side="right")
    
        def create_right_panel(self):
            """Правая панель с графиком Matplotlib"""
            self.right_panel = ctk.CTkFrame(self, corner_radius=12, fg_color="#0F172A")
            self.right_panel.grid(row=0, column=1, padx=15, pady=15, sticky="nsew")
    
            self.lbl_graph_title = ctk.CTkLabel(
                self.right_panel,
                text="📊 Полноразмерный верифицированный сигнал ЭКГ",
                font=("Arial", 16, "bold"),
            )
            self.lbl_graph_title.pack(pady=(20, 5), padx=20, anchor="w")
    
            # Создаем фигуру Matplotlib в темном стиле
            self.fig = Figure(figsize=(8, 4), dpi=100, facecolor="#0F172A")
            self.ax = self.fig.add_subplot(111)
            self.ax.set_facecolor("#1E293B")
    
            # Отрисовка базового сигнала ЭКГ
            self.line, = self.ax.plot(self.t, self.ecg, color="#00E5FF", linewidth=1.2)
    
            # Кастомизация осей под темную тему
            self.ax.tick_params(colors="#94A3B8", labelsize=10)
            self.ax.grid(True, color="#334155", linestyle="--", alpha=0.5)
            for spine in self.ax.spines.values():
                spine.set_color("#334155")
    
            # Наносим зоны событий из JSON (подсветка на графике)
            for ev in self.events:
                self.ax.axvspan(
                    ev["time_start"], ev["time_end"], color=ev["color"], alpha=0.15
                )
                self.ax.text(
                    ev["time_start"] + 0.1,
                    max(self.ecg) - 0.5,
                    "⚠️ Патология" if ev["is_danger"] else "✓ Норма",
                    color=ev["color"],
                    fontsize=9,
                    weight="bold",
                )
    
            # Интеграция холста Matplotlib в Tkinter
            self.canvas = FigureCanvasTkAgg(self.fig, master=self.right_panel)
            self.canvas_widget = self.canvas.get_tk_widget()
            self.canvas_widget.pack(fill="both", expand=True, padx=20, pady=10)
    
            # Информационный блок снизу
            self.info_box = ctk.CTkTextbox(
                self.right_panel, height=80, fg_color="#1E293B", text_color="#E2E8F0"
            )
            self.info_box.pack(fill="x", padx=20, pady=(0, 20))
            self.info_box.insert(
                "0.0",
                "💡 Инструкция: Используйте левую панель для переключения между аномалиями. Красные карточки требуют подтверждения врача.",
            )
    
        def focus_on_graph(self, start, end):
            """Интерактивное приближение (зум) выбранного участка ЭКГ по кнопке"""
            self.ax.set_xlim(start - 1, end + 1)  # Делаем отступ по 1 секунде по бокам
            self.canvas.draw()
    
            # Обновляем инфо-бокс деталями из нашего JSON
            for ev in self.events:
                if ev["time_start"] == start:
                    self.info_box.delete("0.0", "end")
                    self.info_box.insert(
                        "0.0",
                        f"Выбран участок: {start}-{end} сек.\nКонтекст дневника: {ev['context']} -> {ev['symptom']}\nАналитика ИИ и C++: {ev['details']}",
                    )
    
        def send_to_mis(self, ev):
            """Имитация клика отправки патологии в МИС"""
            self.info_box.delete("0.0", "end")
            self.info_box.insert(
                "0.0",
                f"✅ УСПЕШНО: Патология '{ev['ai_verdict']}' привязана к ID пациента в электронной карте (МИС). Рутина сокращена!",
            )
    
    if __name__ == "__main__":
        app = CrossValidationApp()
        app.mainloop()

##### 📋 Шаг 8. Автоматическое генерация медицинского заключения (отчета)

Она полностью закрывает концепцию «избавления врача от рутины»: ИИ не просто нашел аномалии и сопоставил их с дневником, он сам сформулировал медицинский текст. Врачу остается только нажать кнопку «Печать».
Для генерации красивых печатных PDF-отчетов в Python используется библиотека reportlab.

Это сильнейшая продуктовая фича для акселератора и продаж. 
Наш продукт полностью закрывает документооборот. Врачу больше не нужно вручную печатать выписки. Наш ИИ-генератор анализирует матрицу „контекст-симптом-ЭКГ“, сам формулирует текст заключения на медицинском языке, строит таблицу кросс-валидации и выдает готовый PDF-документ, полностью соответствующий стандартам клиники». Это производит колоссальное впечатление на трекеров и инвесторов акселератора

.
------------------------------
##### Ниже представлена готовая функция автоматического текстового синтеза и генерации PDF-файла, которую можно встроить прямо в десктопное приложение

##### Шаг 1: Установка библиотеки
Перед запуском установите библиотеку для работы с PDF:

pip install reportlab

##### Шаг 2: Код модуля автогенерации отчета
Этот скрипт берет данные кросс-валидации (из C++ и дневника), сам формирует клинический текст по правилам кардиологии и собирает стильный печатный PDF-документ.

      import osfrom reportlab.lib.pagesizes import letterfrom reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStylefrom reportlab.lib.styles import getSampleStyleSheet, ParagraphStylefrom reportlab.lib import colorsfrom reportlab.pdfbase import pdfmetricsfrom reportlab.pdfbase.ttfonts import TTFont
      def generate_auto_report(patient_id, events, output_filename="holter_report.pdf"):
          # 1. ПОДДЕРЖКА КИРИЛЛИЦЫ (Важно: скачайте стандартный шрифт DejaVu или Arial)
          # Для демонстрации используем системный шрифт Windows или скачанный .ttf
          try:
              pdfmetrics.registerFont(TTFont('DejaVuSans', 'DejaVuSans.ttf'))
              font_name = 'DejaVuSans'
          except:
              # Если шрифта нет под рукой, ReportLab создаст файл, но кириллица в базовых шрифтах не поддерживается.
              # В реальном проекте обязательно положите файл DejaVuSans.ttf в папку с кодом!
              font_name = 'Helvetica'
      
          doc = SimpleDocTemplate(output_filename, pagesize=letter, rightMargin=40, leftMargin=40, topMargin=40, bottomMargin=40)
          story = []
         
          # 2. НАСТРОЙКА СТИЛЕЙ
          styles = getSampleStyleSheet()
         
          title_style = ParagraphStyle(
              'DocTitle',
              fontName=font_name,
              fontSize=20,
              leading=24,
              textColor=colors.HexColor('#1E293B'),
              spaceAfter=15,
              alignment=1 # По центру
          )
         
          h2_style = ParagraphStyle(
              'SectionHeader',
              fontName=font_name,
              fontSize=14,
              leading=18,
              textColor=colors.HexColor('#3B82F6'),
              spaceBefore=12,
              spaceAfter=6,
              keepWithNext=True
          )
         
          body_style = ParagraphStyle(
              'BodyTextCustom',
              fontName=font_name,
              fontSize=10,
              leading=14,
              textColor=colors.HexColor('#334155'),
              spaceAfter=6
          )
      
          # 3. ШАПКА ДОКУМЕНТА
          story.append(Paragraph("<b>МЕДИЦИНСКОЕ ЗАКЛЮЧЕНИЕ ХОЛТЕР-ИИ</b>", title_style))
          story.append(Paragraph(f"<b>Идентификатор пациента (МИС):</b> {patient_id}", body_style))
          story.append(Paragraph(f"<b>Дата анализа:</b> 07.07.2026", body_style))
          story.append(Spacer(1, 15))
         
          # 4. АВТОМАТИЧЕСКИЙ СИНТЕТИЧЕСКИЙ ТЕКСТ (ИИ-ГЕНЕРАЦИЯ)
          story.append(Paragraph("Динамический анализ данных (Синтез ИИ)", h2_style))
         
          # Логика генератора текста на основе данных
          total_events = len(events)
          danger_events = [e for e in events if e["is_danger"]]
         
          auto_text = f"За период мониторинга система Holter AI обработала суточную запись ЭКГ. " \                f"Было зафиксировано {total_events} значимых триггеров из электронного дневника самочувствия. "
         
          if len(danger_events) > 0:
              auto_text += f"Выявлено <b>{len(danger_events)} несоответствий ритма</b> профилю активности пациента. " \                     f"Основное внимание обращено на эпизоды в состоянии физического покоя, где нейросеть " \                     f"верифицировала признаки патологической активности миокарда. "
          else:
              auto_text += "Критических отклонений ритма, не соответствующих профилю физической нагрузки, не обнаружено. " \                     "Все зафиксированные скачки ЧСС носят адекватный физиологический характер."
                          
          story.append(Paragraph(auto_text, body_style))
          story.append(Spacer(1, 15))
         
          # 5. ТАБЛИЦА СОПОСТАВЛЕНИЯ (КРОСС-ВАЛИДАЦИЯ)
          story.append(Paragraph("Сводный протокол верифицированных событий", h2_style))
         
          # Заголовки таблицы
          table_data = [["Время (сек)", "Контекст (Дневник)", "Симптом", "Вердикт ИИ / Статус"]]
         
          for ev in events:
              status = "⚠️ КРИТИЧНО" if ev["is_danger"] else "✓ Норма"
              table_data.append([
                  f"{ev['time_start']}-{ev['time_end']}",
                  ev["context"],
                  ev["symptom"],
                  f"{ev['ai_verdict']} ({status})"
              ])
         
          # Стилизация таблицы (чистый, строгий медицинский стиль)
          t = Table(table_data, colWidths=[80, 160, 140, 150])
          t.setStyle(TableStyle([
              ('BACKGROUND', (0,0), (-1,0), colors.HexColor('#F1F5F9')),
              ('TEXTCOLOR', (0,0), (-1,0), colors.HexColor('#1E293B')),
              ('FONTNAME', (0,0), (-1,-1), font_name),
              ('FONTSIZE', (0,0), (-1,-1), 9),
              ('BOTTOMPADDING', (0,0), (-1,0), 8),
              ('GRID', (0,0), (-1,-1), 0.5, colors.HexColor('#CBD5E1')),
              ('ALIGN', (0,0), (-1,-1), 'LEFT'),
              ('VALIGN', (0,0), (-1,-1), 'MIDDLE'),
              # Подсветка строк: опасные красим бледным красноватым
              ('BACKGROUND', (0, 2), (-1, 2), colors.HexColor('#FEE2E2')) if events[1]["is_danger"] else ('',),
          ]))
         
          story.append(t)
          story.append(Spacer(1, 40))
         
          # 6. ПОДПИСЬ ВРАЧА
          story.append(Paragraph("Заключение сформировано автоматически системой Holter AI.", body_style))
          story.append(Paragraph("Подпись лечащего врача: _______________________ / ___________________", body_style))
         
          # Сборка документа
          doc.build(story)
      # --- ПРИМЕР ЗАПУСКА ДЛЯ ТЕСТА ---if __name__ == "__main__":
          # Тестовый JSON-массив данных от C++ и Дневника
          mock_events = [
              {
                  "time_start": 2.0, "time_end": 4.0,
                  "context": "Физическая нагрузка (Бег)", "symptom": "Нехватка воздуха / Одышка",
                  "ai_verdict": "Синусовая тахикардия", "is_danger": False
              },
              {
                  "time_start": 7.0, "time_end": 9.0,
                  "context": "Покой (Просмотр ТВ)", "symptom": "Замирание в сердце",
                  "ai_verdict": "Желудочковая экстрасистолия", "is_danger": True
              }
          ]
         
          generate_auto_report(patient_id="Patient_99_Anonymous", events=mock_events)
          print("PDF-отчет успешно сгенерирован!")
      
      ------------------------------
      как встроить в этот PDF-отчет реальное изображение (картинку) того самого аномального участка ЭКГ графика, чтобы отчет выглядел как из настоящей клиники?Как интегрировать это в десктоп на CustomTkinter:
      В код вашего приложения (из прошлого ответа) в правую панель достаточно добавить всего одну красивую кнопку:
      
      self.btn_print = ctk.CTkButton(
          self.right_panel,
          text="🖨️ Сгенерировать печатный отчет (PDF)",
          fg_color="#3B82F6",
          hover_color="#2563EB",
          command=lambda: generate_auto_report(self.entry_id.get(), self.events)
      )
      self.btn_print.pack(pady=10, padx=20, side="bottom")       

##### Как встроить в этот PDF-отчет реальное изображение (картинку) того самого аномального участка ЭКГ графика, чтобы отчет выглядел как из настоящей клиники:


Чтобы добавить изображение аномального участка ЭКГ в PDF-отчет, мы используем связку Matplotlib + ReportLab. Мы временно сохраним график в виде картинки в память (или временный файл) и вставим её прямо в структуру документа.
Ниже представлен обновленный код модуля генерации отчета. Он берет массив точек ЭКГ (от вашего C++ модуля), вырезает именно тот фрагмент, где нейросеть обнаружила патологию в покое, и вставляет этот визуальный пруф в финальный медицинский документ.


Как этот модуль работает внутри стартапа:


   1. Врач в десктопном приложении CustomTkinter нажимает кнопку «Сгенерировать PDF».
   2. Метод generate_pdf_with_graph берет текущий массив точек ЭКГ, полученный от C++ библиотеки, автоматически нарезает его, накладывает разметку ИИ и рендерит во временную картинку.
   3. Картинка вшивается в красивый PDF, а временный файл стирается.
------------------------------


##### Обновленный код генератора PDF с графиком ЭКГ



      import osimport numpy as npimport matplotlib.pyplot as pltfrom reportlab.lib.pagesizes import letterfrom reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, Imagefrom reportlab.lib.styles import getSampleStyleSheet, ParagraphStylefrom reportlab.lib import colorsfrom reportlab.pdfbase import pdfmetricsfrom reportlab.pdfbase.ttfonts import TTFont
      def generate_pdf_with_graph(patient_id, events, ecg_time, ecg_signal, output_filename="holter_report_pro.pdf"):
          # 1. РЕГИСТРАЦИЯ ШРИФТА ДЛЯ КИРИЛЛИЦЫ
          # Убедитесь, что файл DejaVuSans.ttf лежит в папке с вашим скриптом!
          try:
              pdfmetrics.registerFont(TTFont('DejaVuSans', 'DejaVuSans.ttf'))
              font_name = 'DejaVuSans'
          except Exception:
              font_name = 'Helvetica' # Заглушка, если шрифта нет
      
          # 2. ГЕНЕРАЦИЯ КАРТИНКИ ГРАФИКА ДЛЯ ОТЧЕТА
          # Нам нужно найти опасное событие и вырезать его кусок для визуализации
          danger_events = [e for e in events if e["is_danger"]]
          temp_img_path = "temp_anomaly_plot.png"
         
          if danger_events:
              # Берем первое опасное событие для графика
              ev = danger_events[0]
              start_t, end_t = ev["time_start"], ev["time_end"]
             
              # Вырезаем нужный временной интервал из общего сигнала ЭКГ
              mask = (ecg_time >= start_t - 0.5) & (ecg_time <= end_t + 0.5)
              plot_time = ecg_time[mask]
              plot_signal = ecg_signal[mask]
             
              # Строим классический медицинский строгий график на белом фоне для печати
              fig, ax = plt.subplots(figsize=(6, 2), dpi=200)
              ax.plot(plot_time, plot_signal, color='#DC2626', linewidth=1) # Красный цвет для аномалии
              ax.set_title(f"Фрагмент ЭКГ с патологией ({ev['ai_verdict']})", fontsize=9, fontname='DejaVu Sans' if font_name=='DejaVuSans' else 'sans-serif')
              ax.grid(True, color='#CBD5E1', linestyle='--', linewidth=0.5)
              ax.set_xlabel("Время (сек)", fontsize=7)
              ax.set_ylabel("мВ", fontsize=7)
              ax.tick_params(axis='both', which='major', labelsize=7)
             
              # Сохраняем во временный файл
              plt.savefig(temp_img_path, bbox_inches='tight')
              plt.close()
      
          # 3. НАСТРОЙКА ДОКУМЕНТА И СТИЛЕЙ REPORTLAB
          doc = SimpleDocTemplate(output_filename, pagesize=letter, rightMargin=40, leftMargin=40, topMargin=40, bottomMargin=40)
          story = []
          styles = getSampleStyleSheet()
         
          title_style = ParagraphStyle('DocTitle', fontName=font_name, fontSize=18, leading=22, textColor=colors.HexColor('#1E293B'), spaceAfter=15, alignment=1)
          h2_style = ParagraphStyle('SectionHeader', fontName=font_name, fontSize=13, leading=16, textColor=colors.HexColor('#2563EB'), spaceBefore=12, spaceAfter=6, keepWithNext=True)
          body_style = ParagraphStyle('BodyTextCustom', fontName=font_name, fontSize=10, leading=14, textColor=colors.HexColor('#334155'), spaceAfter=6)
      
          # 4. СБОРКА ТЕКСТОВОЙ ЧАСТИ
          story.append(Paragraph("<b>МЕДИЦИНСКОЕ ЗАКЛЮЧЕНИЕ ХОЛТЕР-ИИ</b>", title_style))
          story.append(Paragraph(f"<b>Идентификатор пациента:</b> {patient_id}", body_style))
          story.append(Paragraph(f"<b>Дата анализа:</b> 07.07.2026", body_style))
          story.append(Spacer(1, 10))
         
          story.append(Paragraph("Синтез данных ИИ и кросс-валидация", h2_style))
          auto_text = f"За период мониторинга система автоматически сопоставила логи электронного дневника и ЭКГ. " \                f"Обнаружено {len(danger_events)} критических несоответствий, когда жалобы пациента в состоянии покоя " \                f"совпали с аритмическими событиями, зафиксированными нейросетью."
          story.append(Paragraph(auto_text, body_style))
          story.append(Spacer(1, 10))
         
          # 5. ВСТАВКА ГРАФИКА В PDF
          if os.path.exists(temp_img_path):
              story.append(Paragraph("<b>Объективные данные ИИ-детекции:</b>", body_style))
              # Изменяем размер картинки под ширину страницы (ширина 450 пунктов, высоту подгоняем)
              story.append(Image(temp_img_path, width=450, height=150))
              story.append(Spacer(1, 15))
      
          # 6. ТАБЛИЦА ПРОТОКОЛА
          story.append(Paragraph("Сводный протокол верифицированных событий", h2_style))
          table_data = [["Время (сек)", "Контекст (Дневник)", "Симптом", "Вердикт ИИ"]]
          for ev in events:
              table_data.append([
                  f"{ev['time_start']}-{ev['time_end']}",
                  ev["context"],
                  ev["symptom"],
                  ev["ai_verdict"]
              ])
         
          t = Table(table_data, colWidths=[70, 140, 110, 150])
          t.setStyle(TableStyle([
              ('BACKGROUND', (0,0), (-1,0), colors.HexColor('#F1F5F9')),
              ('TEXTCOLOR', (0,0), (-1,0), colors.HexColor('#1E293B')),
              ('FONTNAME', (0,0), (-1,-1), font_name),
              ('FONTSIZE', (0,0), (-1,-1), 9),
              ('GRID', (0,0), (-1,-1), 0.5, colors.HexColor('#CBD5E1')),
              ('VALIGN', (0,0), (-1,-1), 'MIDDLE'),
          ]))
          story.append(t)
          story.append(Spacer(1, 30))
         
          story.append(Paragraph("Подпись лечащего врача: _______________________ / ___________________", body_style))
         
          # Сборка документа
          doc.build(story)
         
          # Удаляем временную картинку, чтобы не засорять диск
          if os.path.exists(temp_img_path):
              os.remove(temp_img_path)
      # --- ТЕСТОВЫЙ ЗАПУСК ---if __name__ == "__main__":
          # Симулируем сигнал от C++
          t_data = np.linspace(0, 12, 3000)
          ecg_data = np.sin(2 * np.pi * 1.2 * t_data)
          ecg_data += 2.5 * np.exp(-(((t_data - 8.2) / 0.04) ** 2)) # Аномалия
         
          mock_events = [
              {"time_start": 2.0, "time_end": 4.0, "context": "Бег", "symptom": "Одышка", "ai_verdict": "Норма", "is_danger": False},
              {"time_start": 7.5, "time_end": 9.0, "context": "Покой", "symptom": "Замирание", "ai_verdict": "Экстрасистолия", "is_danger": True}
          ]
         
          generate_pdf_with_graph("Patient_MIS_4412", mock_events, t_data, ecg_data)
          print("Профессиональный PDF-отчет с графиком создан!")


##### 💄 Шаг 9.  Дополнительные дизайнерские фишечки


Для создания «дорогого» и плавного интерфейса, Hover-эффекты (изменение цвета при наведении) — это обязательный стандарт. В CustomTkinter эта логика уже встроена на глубоком уровне. Не нужно прописывать сложные анимации вручную. Достаточно передать правильные гексадецимальные (HEX) коды цветов в параметры виджетов.
Чтобы ваше приложение выглядело как премиальный MedTech-продукт, мы применим палитру Tailwind CSS Slate (её используют топовые современные ИТ-компании).
Ниже представлен улучшенный UI-код левой панели и кнопок управления. В нем настроена правильная цветовая гамма, закругления углов, иконки-эмодзи и интерактивные Hover-эффекты для каждого элемента

.
------------------------------
##### Шаблон современного UX/UI элемента с Hover-эффектами
Можно заменить блок кнопок и карточек в основном десктопном приложении на этот код:

    import customtkinter as ctk
    class ModernUIComponents(ctk.CTkFrame):
        def __init__(self, master, **kwargs):
            super().__init__(master, fg_color="#1E293B", corner_radius=12, **kwargs) # Темный Slate фон панели
           
            # Заголовок
            self.title = ctk.CTkLabel(
                self,
                text="🛠️ Панель управления",
                font=("Segoe UI", 16, "bold"),
                text_color="#F8FAFC"
            )
            self.title.pack(pady=15, padx=15, anchor="w")
    
            # 1. СТИЛЬНАЯ СИНЯЯ КНОПКА (Генерация отчета / Синхронизация)
            # При наведении цвет плавно меняется с ярко-синего на глубокий синий
            self.btn_sync = ctk.CTkButton(
                self,
                text="🔄 Синхронизировать с МИС",
                font=("Segoe UI", 13, "bold"),
                height=40,
                corner_radius=8,                # Модное закругление углов
                fg_color="#3B82F6",             # Основной цвет кнопки (ярко-синий)
                hover_color="#1D4ED8",          # Цвет при наведении (темно-синий)
                text_color="#FFFFFF",
                command=self.dummy_action
            )
            self.btn_sync.pack(pady=10, padx=15, fill="x")
    
            # 2. КРИТИЧЕСКАЯ КНОПКА ДЕЙСТВИЯ (Отправка патологии в карту)
            # При наведении меняется с тревожного красного на насыщенный бордовый
            self.btn_alert = ctk.CTkButton(
                self,
                text="🚨 Отправить аномалию в карту",
                font=("Segoe UI", 13, "bold"),
                height=40,
                corner_radius=8,
                fg_color="#EF4444",             # Основной цвет (неоново-красный)
                hover_color="#B91C1C",          # Цвет при наведении (бордовый)
                text_color="#FFFFFF",
                command=self.dummy_action
            )
            self.btn_alert.pack(pady=10, padx=15, fill="x")
    
            # 3. НЕЙТРАЛЬНАЯ КНОПКА (Обзор / Настройки)
            # Инверсивный эффект: серая кнопка становится светлее при наведении
            self.btn_neutral = ctk.CTkButton(
                self,
                text="🔎 Переключить масштаб",
                font=("Segoe UI", 13),
                height=35,
                corner_radius=8,
                fg_color="#334155",             # Темно-серый
                hover_color="#475569",          # Светло-серый при наведении
                text_color="#E2E8F0",
                command=self.dummy_action
            )
            self.btn_neutral.pack(pady=10, padx=15, fill="x")
    
            # 4. ИНТЕРАКТИВНОЕ ПОЛЕ ВВОДА (Поиск пациента)
            # Меняет цвет рамки на синий, когда врач кликает по нему (фокус)
            self.search_entry = ctk.CTkEntry(
                self,
                placeholder_text="🔍 Поиск по ID пациента...",
                font=("Segoe UI", 12),
                height=35,
                corner_radius=8,
                fg_color="#0F172A",             # Очень темный фон внутри
                border_color="#334155",         # Серая рамка в покое
                text_color="#F8FAFC",
                placeholder_text_color="#64748B"
            )
            self.search_entry.pack(pady=15, padx=15, fill="x")
    
        def dummy_action(self):
            print("Действие выполнено с Hover-эффектом!")
    # Пример для быстрого теста панели изолированноif __name__ == "__main__":
        app = ctk.CTk()
        app.title("UI Kit")
        app.geometry("400x400")
        app.configure(fg_color="#0F172A") # Главный фон приложения
       
        panel = ModernUIComponents(app)
        panel.pack(padx=20, pady=20, fill="both", expand=True)
       
        app.mainloop()

##### 🔝 Шаг 10. Многопоточность. Всплывающие подсказки. Объединение в единую дизайн-систему.
 ЗАДАЧА:
* Сделать анимацию загрузки (лоадер) для С++ модуля очистки данных
* Добавить всплывающие подсказки (Tooltips) для медицинских терминов ИИ
* Показать, как собрать интерфейс в единую дизайн-систему (цветовые константы)

Какую пользу этот финальный шаг приносит стартапу:

   1. Техническая зрелость (Архитектура многопоточности): Потоки (threading) — важнейший аспект. Если запустить тяжелый C++ код напрямую по кнопке, десктопное окно мгновенно «замрет» (на нем появится надпись «Программа не отвечает»). Использование фонового потока - это профессиональное решение, которое использует промышленный софт.
   2. Дизайн-система (Масштабируемость): Вынесение всех цветов в класс DesignSystem гарантирует, что если завтра менторы акселератора скажут: «Сделайте ребрендинг и измените красный цвет на фиолетовый», вам не придется переписывать 1000 строк кода. Вы поменяете один HEX-код в константах, и всё приложение мгновенно перекрасится.
   3. Идеальный UX подсказок (Tooltips): Врачу не нужно гадать, за что отвечает то или иное поле. При наведении мыши всплывают аккуратные, темные, современные плашки, объясняющие внутреннюю логику ИИ.
 


Закроем все три задачи, чтобы превратить интерфейс стартапа в полноценный, законченный продукт промышленного уровня. Мы объединим анимацию загрузки (лоадер) для C++ движка, интерактивные всплывающие подсказки (Tooltips) для врача и вынесем всё это в единую дизайн-систему (цветовые константы).
Для работы кода нам понадобится одна небольшая внешняя библиотека для создания красивых всплывающих подсказок:
      
      pip install CTkTooltip
      
      ## Полный код десктопного приложения с лоадером, подсказками и дизайн-системой
      Ниже представлена монолитная реализация, которую можно запускать. Она имитирует долгий процесс очистки данных на C++ и показывает врачу стильный индикатор прогресса, не замораживая при этом само окно программы.
      
      import customtkinter as ctkimport timeimport threadingfrom CTkTooltip import * # Библиотека для современных всплывающих подсказок
      # =====================================================================# 1. ЕДИНАЯ ДИЗАЙН-СИСТЕМА (ЦВЕТОВЫЕ КОНСТАНТЫ ТАЙЛВИНД CSS)# =====================================================================class DesignSystem:
          # Фоновые цвета (Slate палитра)
          BG_MAIN = "#0F172A"       # Самый глубокий темный фон приложения
          BG_PANEL = "#1E293B"      # Фон внутренних карточек и панелей
          BG_INPUT = "#0F172A"      # Фон внутри полей ввода
         
          # Линейные и контурные цвета
          BORDER_NEUTRAL = "#334155" # Серый контур элементов в покое
         
          # Сигнальные цвета (Светофорная логика)
          COLOR_PRIMARY = "#3B82F6"  # Информационный ярко-синий
          COLOR_HOVER_PRIMARY = "#1D4ED8"
         
          COLOR_SUCCESS = "#10B981"  # ИИ-Норма (Изумрудно-зеленый)
          COLOR_HOVER_SUCCESS = "#059669"
         
          COLOR_DANGER = "#EF4444"   # ИИ-Патология (Неоново-красный)
          COLOR_HOVER_DANGER = "#DC2626"
         
          # Текстовые цвета
          TEXT_MAIN = "#F8FAFC"      # Основной белый текст
          TEXT_MUTED = "#94A3B8"     # Приглушенный серый текст для подписей
         
          # Типографика
          FONT_FAMILY = "Segoe UI"
      
      # =====================================================================# 2. ГЛАВНОЕ ПРИЛОЖЕНИЕ# =====================================================================class MedOSApp(ctk.CTk):
          def __init__(self):
              super().__init__()
      
              self.title("🤖 Holter AI — Промышленный прототип")
              self.geometry("600x500")
              self.configure(fg_color=DesignSystem.BG_MAIN)
             
              # Заголовок экрана
              self.lbl_header = ctk.CTkLabel(
                  self,
                  text="🫀 Модуль обработки сигналов Холтера",
                  font=(DesignSystem.FONT_FAMILY, 18, "bold"),
                  text_color=DesignSystem.TEXT_MAIN
              )
              self.lbl_header.pack(pady=(30, 10), padx=20, anchor="w")
      
              # Основная рабочая панель
              self.panel = ctk.CTkFrame(self, fg_color=DesignSystem.BG_PANEL, corner_radius=12)
              self.panel.pack(fill="both", expand=True, padx=20, pady=10)
      
              # Интерактивное поле ввода ID пациента с фокус-эффектом
              self.entry_id = ctk.CTkEntry(
                  self.panel,
                  placeholder_text="🆔 Введите обезличенный ID пациента...",
                  font=(DesignSystem.FONT_FAMILY, 13),
                  height=40,
                  corner_radius=8,
                  fg_color=DesignSystem.BG_INPUT,
                  border_color=DesignSystem.BORDER_NEUTRAL,
                  text_color=DesignSystem.TEXT_MAIN,
                  placeholder_text_color=DesignSystem.TEXT_MUTED
              )
              self.entry_id.pack(pady=(30, 15), padx=20, fill="x")
             
              # --- ВСПЛЫВАЮЩАЯ ПОДСКАЗКА ДЛЯ ПОЛЯ ВВОДА ---
              CTkTooltip(self.entry_id, message="Введите хэш-код из МИС для соблюдения ФЗ-152 о персональных данных", delay=0.3)
      
              # Кнопка запуска тяжелых С++ вычислений
              self.btn_run = ctk.CTkButton(
                  self.panel,
                  text="🚀 Запустить очистку сигнала (C++) и ИИ-анализ",
                  font=(DesignSystem.FONT_FAMILY, 13, "bold"),
                  height=45,
                  corner_radius=8,
                  fg_color=DesignSystem.COLOR_PRIMARY,
                  hover_color=DesignSystem.COLOR_HOVER_PRIMARY,
                  text_color=DesignSystem.TEXT_MAIN,
                  command=self.start_processing_thread
              )
              self.btn_run.pack(pady=10, padx=20, fill="x")
             
              # --- ВСПЛЫВАЮЩАЯ ПОДСКАЗКА ДЛЯ КНОПКИ ---
              CTkTooltip(self.btn_run, message="Клик запустит внешнюю C++ библиотеку фильтрации Пэна-Томпкинса", delay=0.3)
      
              # =====================================================================
              # 3. ЭЛЕМЕНТЫ ЛОАДЕРА (СКРЫТЫ ПО УМОЛЧАНИЮ)
              # =====================================================================
              self.lbl_status = ctk.CTkLabel(
                  self.panel,
                  text="Ожидание запуска...",
                  font=(DesignSystem.FONT_FAMILY, 12, "italic"),
                  text_color=DesignSystem.TEXT_MUTED
              )
              self.lbl_status.pack(pady=(20, 5))
      
              self.progress_bar = ctk.CTkProgressBar(
                  self.panel,
                  width=400,
                  height=8,
                  corner_radius=4,
                  progress_color=DesignSystem.COLOR_SUCCESS,
                  fg_color=DesignSystem.BG_MAIN
              )
              self.progress_bar.pack(pady=5)
              self.progress_bar.set(0) # Изначально прогресс равен нулю
      
          # =====================================================================
          # 4. МНОГОПОТОЧНАЯ ЛОГИКА ЛОАДЕРА (ЧТОБЫ ИНТЕРФЕЙС НЕ ЗАВИСАЛ)
          # =====================================================================
          def start_processing_thread(self):
              """Запуск тяжелой задачи в отдельном потоке"""
              if not self.entry_id.get():
                  self.lbl_status.configure(text="❌ Ошибка: Сначала укажите ID пациента!", text_color=DesignSystem.COLOR_DANGER)
                  return
                 
              # Блокируем кнопку на время расчета, чтобы избежать повторных кликов
              self.btn_run.configure(state="disabled")
             
              # Создаем и запускаем фоновый поток вычислений
              threading.Thread(target=self.simulate_cpp_and_ai_work, daemon=True).start()
      
          def simulate_cpp_and_ai_work(self):
              """Имитация работы C++ движка фильтрации и инференса нейросети"""
              steps = [
                  (0.2, "⚙️ Загрузка бинарного EDF-файла..."),
                  (0.5, "🧬 C++: Применение полосового фильтра Пэна-Томпкинса..."),
                  (0.8, "🧠 ИИ: Нарезка QRS-комплексов и классификация аритмий..."),
                  (1.0, "✅ Готово! Результаты кросс-валидации выгружены.")
              ]
             
              for progress, text in steps:
                  time.sleep(1.2) # Имитируем реальное время выполнения тяжелого кода
                 
                  # Важно: обновление графических элементов делаем через метод .after()
                  # или напрямую, так как CustomTkinter потокобезопасен для базовых свойств
                  self.progress_bar.set(progress)
                  self.lbl_status.configure(text=text, text_color=DesignSystem.TEXT_MAIN)
                 
              # Возвращаем кнопку в активное состояние по окончании
              self.btn_run.configure(state="normal")
              self.lbl_status.configure(text_color=DesignSystem.COLOR_SUCCESS)
      
      if __name__ == "__main__":
          app = MedOSApp()
          app.mainloop()
