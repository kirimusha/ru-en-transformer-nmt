<p align="center">
  <img src="https://img.shields.io/badge/Русско--английский_машинный_переводчик-e83e8c?style=for-the-badge" alt="Русско-английский машинный переводчик" width="600">
</p>

Это изучение, что вообще собой представляет архитектура Transformer. Основано на статье "Attention is all you need" 2017 года под авторством восьми исследователей из Google. Ресурсы были ограничены (у меня не было 8 GPU), поэтому в реализации есть накопление градиентов, когда они обновляются раз в 8 эпох в моем случае, чтобы модель смогла увидеть как можно больше данных и при этом оперативная память не переполнялась. Также я обрезала длины последовательностей до 200 токенов, в этом датасете были последовательности до 8000 токенов. Модель обучена на датасете OPUS-100, на ru-en паре.

Веса модели: https://drive.google.com/drive/folders/1myrFmVeuuM07lNLOnLUPWoY--3BmXane?usp=sharing

<p align="center">
  <img src="https://img.shields.io/badge/Полученные_метрики-3498db?style=for-the-badge" alt="Полученные метрики" width="200">
</p>

Для оценки качества перевода обученной модели были использованы три стандартные метрики: BLEU (Bilingual Evaluation Understudy), METEOR (Metric for Evaluation of Translation with Explicit Ordering) и TER (Translation Edit Rate). Оценка проводилась на тестовой выборке из 2000 предложений. Результаты представлены в таблице ниже.

<table>
  <tr>
    <th style="background-color: #2c3e50; color: white; padding: 8px;">Метрика</th>
    <th style="background-color: #2c3e50; color: white; padding: 8px;">Значение</th>
  </tr>
  <tr>
    <td style="background-color: #2874a6; color: white; padding: 8px;"><b>BLEU</b></td>
    <td style="background-color: #d4e6f1; padding: 8px;"><b>20.01</b></td>
  </tr>
  <tr>
    <td style="background-color: #1e8449; color: white; padding: 8px;"><b>METEOR</b></td>
    <td style="background-color: #d5f5e3; padding: 8px;"><b>0.3927</b></td>
  </tr>
  <tr>
    <td style="background-color: #b9770e; color: white; padding: 8px;"><b>TER</b></td>
    <td style="background-color: #fdebd0; padding: 8px;"><b>1.0747</b></td>
  </tr>
</table>

У BLEU диапазон от 0 до 100, выше 20 минимально достаточно для сохранения общего смысла. 
METEOR от 0 до 1, где 0.3–0.5 среднее качество перевода с передачей ключевой семантики. У TER в идеале стремится к 0, а норма --- 0.4–0.6. Превышение из-за избыточного циклического повтора токенов.

<p align="center">
  <img src="https://img.shields.io/badge/Инференс-f39c12?style=for-the-badge" alt="Инференс" width="200">
</p>

Здесь есть пробелы, потому что нужно обучить обратный токенизатор-склеиватель, чтобы от них избавиться, но в моей реализации его нет(

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_1-c0392b?style=for-the-badge" alt="Пример 1">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Было так холодно на улице , вы просто не представля ете .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">It ' s so cold out there , you ' ve no idea .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">It was so cold , you just don ' t have to do it .</td>
  </tr>
</table>

<br>

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_2-c0392b?style=for-the-badge" alt="Пример 2">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Я всё знаю , мистер Уол кер .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Nothing gets by me , Mr . Wal ker .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">I know everything , Mr . Wal ker .</td>
  </tr>
</table>

<br>

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_3-c0392b?style=for-the-badge" alt="Пример 3">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Есть ли шанс , что все это просто обру шится ?</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Isn ' t there a chance this lot ' s just gonna collapse ?</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Is there anything that ' s just a joke ?</td>
  </tr>
</table>

<br>

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_5-c0392b?style=for-the-badge" alt="Пример 4">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Но два учени ка - это не достаточно , чтобы держать открытой школу .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Two students are not enough to sustain a school .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">But two - is not enough enough to keep the school school .</td>
  </tr>
</table>

<br>

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_6-c0392b?style=for-the-badge" alt="Пример 5">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Они знали , о деньгах , и они знали точную сумму .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">They knew about the money and they knew the exact amount .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">They knew about money , and they knew the amount of the amount .</td>
  </tr>
</table>

<br>

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_9-c0392b?style=for-the-badge" alt="Пример 6">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">Каза лось , будто с ними легче разговаривать , чем с местными жи телями , американ цами , которые обслужи вали нас и пода вали нам еду .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">It seemed they were probably easier to talk to than the local Americans who were waiting on us as and serving food .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">It was like it was easier to talk to them than the Americans that had the Americans that had taken us and had brought us .</td>
  </tr>
</table>

<br>

<table>
  <p align="left">
  <img src="https://img.shields.io/badge/Пример_10-c0392b?style=for-the-badge" alt="Пример 7">
  </p>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px; width: 15%;"><b>Исходник</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">- Мы будем делать домаш ние зада ния .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Референс</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">- We ' ll do home work .</td>
  </tr>
  <tr>
    <td style="background-color: #c0392b; color: white; padding: 6px;"><b>Перевод</b></td>
    <td style="background-color: #fadbd8; padding: 6px;">- We ' re gonna do the home of the home .</td>
  </tr>
</table>
</body>
</html>
