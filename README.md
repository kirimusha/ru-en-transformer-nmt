# Архитектура Transformer для русско-английского машинного перевода

Это изучение, что вообще собой представляет архитектура. Основано на статье "Attention is all you need" 2017 года под авторством восьми исследователей из Google. Ресурсы были ограничены (у меня не было 8 GPU), поэтому в реализации есть накопление градиентов, когда они обновляются раз в 8 эпох в моем случае, чтобы модель смогла увидеть как можно больше данных и при этом оперативная память не переполнялась. Также я обрезала длины последовательностей до 200 токенов, в этом датасете были последовательности до 8000 токенов. Модель обучена на датасете OPUS-100, на ru-en паре.

Веса модели: https://drive.google.com/drive/folders/1myrFmVeuuM07lNLOnLUPWoY--3BmXane?usp=sharing

## Полученные метрики

Для оценки качества перевода обученной модели были использованы три стандартные метрики: BLEU (Bilingual Evaluation Understudy), METEOR (Metric for Evaluation of Translation with Explicit Ordering) и TER (Translation Edit Rate). Оценка проводилась на тестовой выборке из 2000 предложений. Результаты представлены в таблице ниже.

| Метрика | Значение |
| :--- | :--- |
| BLEU | <font color="#27ae60">**20.01**</font> |
| METEOR | <font color="#e67e22">**0.3927**</font> |
| TER | <font color="#c0392b">**1.0747**</font> |

У BLEU диапазон от 0 до 100, выше 20 минимально достаточно для сохранения общего смысла. 
METEOR от 0 до 1, где 0.3–0.5 среднее качество перевода с передачей ключевой семантики. У TER в идеале стремится к 0, а норма --- 0.4–0.6. Превышение из-за избыточного циклического повтора токенов.

## Инференс

Здесь есть пробелы, потому что нужно обучить обратный токенизатор-склеиватель, чтобы от них избавиться, но в моей реализации его нет(

### Пример 1

<font color="#3498db">**Исходник:**</font> Было так холодно на улице , вы просто не представля ете .  
<font color="#2ecc71">**Референс:**</font> It ' s so cold out there , you ' ve no idea .  
<font color="#e67e22">**Перевод:**</font> It was so cold , you just don ' t have to do it .

---

### Пример 2

<font color="#3498db">**Исходник:**</font> Я всё знаю , мистер Уол кер .  
<font color="#2ecc71">**Референс:**</font> Nothing gets by me , Mr . Wal ker .  
<font color="#e67e22">**Перевод:**</font> I know everything , Mr . Wal ker .

---

### Пример 3

<font color="#3498db">**Исходник:**</font> Есть ли шанс , что все это просто обру шится ?  
<font color="#2ecc71">**Референс:**</font> Isn ' t there a chance this lot ' s just gonna collapse ?  
<font color="#e67e22">**Перевод:**</font> Is there anything that ' s just a joke ?

---

### Пример 4

<font color="#3498db">**Исходник:**</font> 34 И зна чали й 25 гер . ( 00 : 06 : 31 )  
<font color="#2ecc71">**Референс:**</font> 3 Im mer se us 10 ( 00 : 06 : 32 )  
<font color="#e67e22">**Перевод:**</font> 34 25 H ( 00 : 06 : 31 )

---

### Пример 5

<font color="#3498db">**Исходник:**</font> Но два учени ка - это не достаточно , чтобы держать открытой школу .  
<font color="#2ecc71">**Референс:**</font> Two students are not enough to sustain a school .  
<font color="#e67e22">**Перевод:**</font> But two - is not enough enough to keep the school school .

---

### Пример 6

<font color="#3498db">**Исходник:**</font> Они знали , о деньгах , и они знали точную сумму .  
<font color="#2ecc71">**Референс:**</font> They knew about the money and they knew the exact amount .  
<font color="#e67e22">**Перевод:**</font> They knew about money , and they knew the amount of the amount .

---

### Пример 7

<font color="#3498db">**Исходник:**</font> 14 15  
<font color="#2ecc71">**Референс:**</font> 84 15  
<font color="#e67e22">**Перевод:**</font> 14 15

---

### Пример 8

<font color="#3498db">**Исходник:**</font> It is currently Fri May 20 , 2016 12 : 29 am  
<font color="#2ecc71">**Референс:**</font> It is currently Tue May 17 , 2016 12 : 07 am  
<font color="#e67e22">**Перевод:**</font> It is currently Fri May 27 , 2016 1 : 16 am

---

### Пример 9

<font color="#3498db">**Исходник:**</font> Каза лось , будто с ними легче разговаривать , чем с местными жи телями , американ цами , которые обслужи вали нас и пода вали нам еду .  
<font color="#2ecc71">**Референс:**</font> It seemed they were probably easier to talk to than the local Americans who were waiting on us as and serving food .  
<font color="#e67e22">**Перевод:**</font> It was like it was easier to talk to them than the Americans that had the Americans that had taken us and had brought us .

---

### Пример 10

<font color="#3498db">**Исходник:**</font> - Мы будем делать домаш ние зада ния .  
<font color="#2ecc71">**Референс:**</font> - We ' ll do home work .  
<font color="#e67e22">**Перевод:**</font> - We ' re gonna do the home of the home .
