# Архитектура Transformer для русско-английского машинного перевода

Это изучение, что вообще собой представляет архитектура. Основано на статье "Attention is all you need" 2017 года под авторством восьми исследователей из Google. Ресурсы были ограничены (у меня не было 8 GPU), поэтому в реализации есть накопление градиентов, когда они обновляются раз в 8 эпох в моем случае, чтобы модель смогла увидеть как можно больше данных и при этом оперативная память не переполнялась. Также я обрезала длины последовательностей до 200 токенов, в этом датасете были последовательности до 8000 токенов. Модель обучена на датасете OPUS-100, на ru-en паре.

Веса модели: https://drive.google.com/drive/folders/1myrFmVeuuM07lNLOnLUPWoY--3BmXane?usp=sharing

## Полученные метрики

Для оценки качества перевода обученной модели были использованы три стандартные метрики: BLEU (Bilingual Evaluation Understudy), METEOR (Metric for Evaluation of Translation with Explicit Ordering) и TER (Translation Edit Rate). Оценка проводилась на тестовой выборке из 2000 предложений. Результаты представлены в таблице ниже.

<table>
  <thead>
    <tr style="background-color: #2c3e50; color: white;">
      <th style="padding: 10px; text-align: center;">Метрика</th>
      <th style="padding: 10px; text-align: center;">Значение</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background-color: #d5e8d0;">
      <td style="padding: 8px; text-align: center; font-weight: bold;">BLEU</td>
      <td style="padding: 8px; text-align: center; color: #27ae60; font-weight: bold;">20.01</td>
    </tr>
    <tr style="background-color: #fdebd0;">
      <td style="padding: 8px; text-align: center; font-weight: bold;">METEOR</td>
      <td style="padding: 8px; text-align: center; color: #e67e22; font-weight: bold;">0.3927</td>
    </tr>
    <tr style="background-color: #fadbd8;">
      <td style="padding: 8px; text-align: center; font-weight: bold;">TER</td>
      <td style="padding: 8px; text-align: center; color: #c0392b; font-weight: bold;">1.0747</td>
    </tr>
  </tbody>
</table>

У BLEU диапазон от 0 до 100, выше 20 минимально достаточно для сохранения общего смысла. 
METEOR от 0 до 1, где 0.3–0.5 среднее качество перевода с передачей ключевой семантики. У TER в идеале стремится к 0, а норма --- 0.4–0.6. Превышение из-за избыточного циклического повтора токенов.

## Инференс

Здесь есть пробелы, потому что нужно обучить обратный токенизатор-склеиватель, чтобы от них избавиться, но в моей реализации его нет(

<h3 style="color: #2980b9;">Пример 1</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">Было так холодно на улице , вы просто не представля ете .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">It ' s so cold out there , you ' ve no idea .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">It was so cold , you just don ' t have to do it .</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 2</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">Я всё знаю , мистер Уол кер .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">Nothing gets by me , Mr . Wal ker .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">I know everything , Mr . Wal ker .</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 3</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">Есть ли шанс , что все это просто обру шится ?</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">Isn ' t there a chance this lot ' s just gonna collapse ?</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">Is there anything that ' s just a joke ?</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 4</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">34 И зна чали й 25 гер . ( 00 : 06 : 31 )</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">3 Im mer se us 10 ( 00 : 06 : 32 )</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">34 25 H ( 00 : 06 : 31 )</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 5</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">Но два учени ка - это не достаточно , чтобы держать открытой школу .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">Two students are not enough to sustain a school .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">But two - is not enough enough to keep the school school .</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 6</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">Они знали , о деньгах , и они знали точную сумму .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">They knew about the money and they knew the exact amount .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">They knew about money , and they knew the amount of the amount .</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 7</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">14 15</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">84 15</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">14 15</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 8</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">It is currently Fri May 20 , 2016 12 : 29 am</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">It is currently Tue May 17 , 2016 12 : 07 am</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">It is currently Fri May 27 , 2016 1 : 16 am</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 9</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">Каза лось , будто с ними легче разговаривать , чем с местными жи телями , американ цами , которые обслужи вали нас и пода вали нам еду .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">It seemed they were probably easier to talk to than the local Americans who were waiting on us as and serving food .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">It was like it was easier to talk to them than the Americans that had the Americans that had taken us and had brought us .</td>
  </tr>
</table>

<br>

<h3 style="color: #2980b9;">Пример 10</h3>

<table style="border-collapse: collapse; width: 100%;">
  <tr>
    <td style="background-color: #3498db; color: white; padding: 8px; font-weight: bold; width: 15%;">Исходник</td>
    <td style="background-color: #ecf0f1; padding: 8px;">- Мы будем делать домаш ние зада ния .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 8px; font-weight: bold;">Референс</td>
    <td style="background-color: #eafaf1; padding: 8px;">- We ' ll do home work .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 8px; font-weight: bold;">Перевод</td>
    <td style="background-color: #fef9e7; padding: 8px;">- We ' re gonna do the home of the home .</td>
  </tr>
</table>
