# LB-PR-5-Hurzidze-Anton

## Хурцидзе Антон IПЗ 4.02 Лабораторна-Практична робота № 5

## Тема: Обробка координатних даних: Придушення шумів у потоці (Real-time).
## Мета: Реалізувати програмну архітектуру для потокової (real-time) обробки координатних даних (імітація роботи мікроконтролера, без доступу до "майбутніх" точок). Імплементувати три типи цифрових фільтрів: SMA (Simple Moving Average), EMA (Exponential Moving Average) та Median Filter. Дослідити компроміс між якістю фільтрації (гладкістю) та динамічними спотвореннями (затримкою/lag). Провести спектральний аналіз помилки фільтрації.

### 1. Підготовка коду

#### Створюємо новий ноутбук в застосунку Google Colab та копіюємо код з завдання:

![0](https://github.com/GAMECHl/LB-PR-5/blob/main/1.png)
#### Рис. 1 - Створенний ноутбук

### 2. Реалізація

#### У блоці "2. РЕАЛІЗАЦІЯ ФІЛЬТРІВ" змінюємо TODO та return x на робочий кодта запускаємо скрипт. Лінії на графіках згладженні, тому все вірно:

`` class SMAFilter:
    def __init__(self, w):
        self.w = w
        self.q = deque(maxlen=w)
        self.sum = 0.0  # Для оптимізації O(1) ``
        
    def update(self, x):
        # Якщо черга повна, віднімаємо старий елемент
        if len(self.q) == self.w:
            self.sum -= self.q[0]
        
        # Додаємо новий елемент
        self.q.append(x)
        self.sum += x
        
        # Повертаємо середнє
        return self.sum / len(self.q) 

`` class EMAFilter:
    def __init__(self, alpha):
        self.a = alpha
        self.last = None ``

    def update(self, x):
        # Ініціалізація першим значенням
        if self.last is None:
            self.last = x
        else:
            # Експоненційне згладжування
            self.last = self.a * x + (1 - self.a) * self.last
        
        return self.last

`` class MedianFilter:
    def __init__(self, w):
        if w % 2 == 0:
            w += 1
        self.w = w
        self.q = deque(maxlen=w)``

    def update(self, x):
        # Додаємо елемент у чергу
        self.q.append(x)
        
        # Повертаємо медіану
        return np.median(self.q) 
