# Русско-английский машинный переводчик

Это изучение, что вообще собой представляет архитектура. Основано на статье "Attention is all you need" 2017 года под авторством восьми исследователей из Google. Ресурсы были ограничены (у меня не было 8 GPU), поэтому в реализации есть накопление градиентов, когда они обновляются раз в 8 эпох в моем случае, чтобы модель смогла увидеть как можно больше данных и при этом оперативная память не переполнялась. Также я обрезала длины последовательностей до 200 токенов, в этом датасете были последовательности до 8000 токенов. Модель обучена на датасете OPUS-100, на ru-en паре.

Веса модели: https://drive.google.com/drive/folders/1myrFmVeuuM07lNLOnLUPWoY--3BmXane?usp=sharing

## Полученные метрики

Для оценки качества перевода обученной модели были использованы три стандартные метрики: BLEU (Bilingual Evaluation Understudy), METEOR (Metric for Evaluation of Translation with Explicit Ordering) и TER (Translation Edit Rate). Оценка проводилась на тестовой выборке из 2000 предложений. Результаты представлены в таблице ниже.

<table> <tr> <th style="background-color: #2c3e50; color: white; padding: 8px;">Метрика</th> <th style="background-color: #2c3e50; color: white; padding: 8px;">Значение</th> </tr> <tr> <td style="background-color: #2874a6; color: white; padding: 8px;"><b>BLEU</b></td> <td style="background-color: #d4e6f1; padding: 8px;"><b>20.01</b></td> </tr> <tr> <td style="background-color: #1e8449; color: white; padding: 8px;"><b>METEOR</b></td> <td style="background-color: #d5f5e3; padding: 8px;"><b>0.3927</b></td> </tr> <tr> <td style="background-color: #b9770e; color: white; padding: 8px;"><b>TER</b></td> <td style="background-color: #fdebd0; padding: 8px;"><b>1.0747</b></td> </tr> </table>


У BLEU диапазон от 0 до 100, выше 20 минимально достаточно для сохранения общего смысла. 
METEOR от 0 до 1, где 0.3–0.5 среднее качество перевода с передачей ключевой семантики. У TER в идеале стремится к 0, а норма --- 0.4–0.6. Превышение из-за избыточного циклического повтора токенов.

## Инференс

Здесь есть пробелы, потому что нужно обучить обратный токенизатор-склеиватель, чтобы от них избавиться, но в моей реализации его нет(

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 1</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">Было так холодно на улице , вы просто не представля ете .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">It ' s so cold out there , you ' ve no idea .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">It was so cold , you just don ' t have to do it .</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 2</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">Я всё знаю , мистер Уол кер .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">Nothing gets by me , Mr . Wal ker .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">I know everything , Mr . Wal ker .</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 3</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">Есть ли шанс , что все это просто обру шится ?</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">Isn ' t there a chance this lot ' s just gonna collapse ?</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">Is there anything that ' s just a joke ?</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 4</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">34 И зна чали й 25 гер . ( 00 : 06 : 31 )</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">3 Im mer se us 10 ( 00 : 06 : 32 )</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">34 25 H ( 00 : 06 : 31 )</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 5</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">Но два учени ка - это не достаточно , чтобы держать открытой школу .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">Two students are not enough to sustain a school .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">But two - is not enough enough to keep the school school .</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 6</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">Они знали , о деньгах , и они знали точную сумму .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">They knew about the money and they knew the exact amount .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">They knew about money , and they knew the amount of the amount .</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 7</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">14 15</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">84 15</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">14 15</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 8</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">It is currently Fri May 20 , 2016 12 : 29 am</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">It is currently Tue May 17 , 2016 12 : 07 am</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">It is currently Fri May 27 , 2016 1 : 16 am</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 9</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">Каза лось , будто с ними легче разговаривать , чем с местными жи телями , американ цами , которые обслужи вали нас и пода вали нам еду .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">It seemed they were probably easier to talk to than the local Americans who were waiting on us as and serving food .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">It was like it was easier to talk to them than the Americans that had the Americans that had taken us and had brought us .</td>
  </tr>
</table>

<br>

<table>
  <tr>
    <td colspan="2" style="background-color: #2980b9; color: white; padding: 6px; font-size: 18px;"><b>Пример 10</b></td>
  </tr>
  <tr>
    <td style="background-color: #3498db; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #ecf0f1; padding: 6px;">- Мы будем делать домаш ние зада ния .</td>
  </tr>
  <tr>
    <td style="background-color: #2ecc71; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #eafaf1; padding: 6px;">- We ' ll do home work .</td>
  </tr>
  <tr>
    <td style="background-color: #e67e22; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fef9e7; padding: 6px;">- We ' re gonna do the home of the home .</td>
  </tr>
</table>
