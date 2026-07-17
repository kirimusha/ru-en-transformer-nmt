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





<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Примеры перевода</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Segoe UI', 'Roboto', system-ui, sans-serif;
            background: #0d1117;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }

        .carousel {
            position: relative;
            width: 100%;
            max-width: 820px;
            background: #161b22;
            border-radius: 12px;
            padding: 28px 24px 20px 24px;
            border: 1px solid #30363d;
        }

        .track {
            display: flex;
            transition: transform 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            overflow: hidden;
        }

        .slide {
            min-width: 100%;
            padding: 4px 2px;
        }

        .slide-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding-bottom: 14px;
            border-bottom: 1px solid #21262d;
            margin-bottom: 16px;
        }

        .slide-header h3 {
            color: #f0f6fc;
            font-weight: 600;
            font-size: 15px;
            letter-spacing: 0.3px;
        }

        .slide-counter {
            color: #8b949e;
            font-size: 13px;
            background: #21262d;
            padding: 2px 12px;
            border-radius: 12px;
        }

        .block {
            border-radius: 8px;
            padding: 14px 16px;
            margin-bottom: 10px;
            border-left: 4px solid;
        }

        .block-source {
            background: #0d1117;
            border-left-color: #da3633;
        }

        .block-reference {
            background: #0d1117;
            border-left-color: #3fb950;
        }

        .block-translation {
            background: #0d1117;
            border-left-color: #d29922;
        }

        .block-label {
            font-size: 11px;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 0.8px;
            margin-bottom: 3px;
        }

        .label-source {
            color: #f85149;
        }

        .label-reference {
            color: #3fb950;
        }

        .label-translation {
            color: #d29922;
        }

        .block-text {
            color: #e6edf3;
            font-size: 15px;
            line-height: 1.6;
            word-break: break-word;
        }

        /* Навигация */
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            gap: 24px;
            margin-top: 18px;
            padding-top: 14px;
            border-top: 1px solid #21262d;
        }

        .btn {
            background: #21262d;
            color: #c9d1d9;
            border: 1px solid #30363d;
            width: 38px;
            height: 38px;
            border-radius: 50%;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: 400;
        }

        .btn:hover {
            background: #30363d;
            border-color: #58a6ff;
            color: #58a6ff;
        }

        .dots {
            display: flex;
            gap: 6px;
        }

        .dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #30363d;
            cursor: pointer;
            transition: all 0.25s ease;
            border: none;
        }

        .dot.active {
            background: #58a6ff;
            width: 20px;
            border-radius: 4px;
        }

        .dot:hover {
            background: #8b949e;
        }

        .dot.active:hover {
            background: #58a6ff;
        }

        @media (max-width: 600px) {
            .carousel {
                padding: 16px 12px;
            }
            .block-text {
                font-size: 14px;
            }
            .block {
                padding: 10px 12px;
            }
            .slide-header h3 {
                font-size: 13px;
            }
        }
    </style>
</head>
<body>

<div class="carousel">
    <div class="track" id="track">

        <!-- Слайд 1 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 1</h3>
                <span class="slide-counter">1 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">Было так холодно на улице , вы просто не представля ете .</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">It ' s so cold out there , you ' ve no idea .</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">It was so cold , you just don ' t have to do it .</div>
            </div>
        </div>

        <!-- Слайд 2 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 2</h3>
                <span class="slide-counter">2 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">Я всё знаю , мистер Уол кер .</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">Nothing gets by me , Mr . Wal ker .</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">I know everything , Mr . Wal ker .</div>
            </div>
        </div>

        <!-- Слайд 3 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 3</h3>
                <span class="slide-counter">3 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">Есть ли шанс , что все это просто обру шится ?</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">Isn ' t there a chance this lot ' s just gonna collapse ?</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">Is there anything that ' s just a joke ?</div>
            </div>
        </div>

        <!-- Слайд 4 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 4</h3>
                <span class="slide-counter">4 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">Но два учени ка - это не достаточно , чтобы держать открытой школу .</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">Two students are not enough to sustain a school .</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">But two - is not enough enough to keep the school school .</div>
            </div>
        </div>

        <!-- Слайд 5 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 5</h3>
                <span class="slide-counter">5 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">Они знали , о деньгах , и они знали точную сумму .</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">They knew about the money and they knew the exact amount .</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">They knew about money , and they knew the amount of the amount .</div>
            </div>
        </div>

        <!-- Слайд 6 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 6</h3>
                <span class="slide-counter">6 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">Каза лось , будто с ними легче разговаривать , чем с местными жи телями , американ цами , которые обслужи вали нас и пода вали нам еду .</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">It seemed they were probably easier to talk to than the local Americans who were waiting on us as and serving food .</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">It was like it was easier to talk to them than the Americans that had the Americans that had taken us and had brought us .</div>
            </div>
        </div>

        <!-- Слайд 7 -->
        <div class="slide">
            <div class="slide-header">
                <h3>Пример 7</h3>
                <span class="slide-counter">7 / 7</span>
            </div>
            <div class="block block-source">
                <div class="block-label label-source">Исходник</div>
                <div class="block-text">- Мы будем делать домаш ние зада ния .</div>
            </div>
            <div class="block block-reference">
                <div class="block-label label-reference">Референс</div>
                <div class="block-text">- We ' ll do home work .</div>
            </div>
            <div class="block block-translation">
                <div class="block-label label-translation">Перевод</div>
                <div class="block-text">- We ' re gonna do the home of the home .</div>
            </div>
        </div>

    </div>

    <!-- Управление -->
    <div class="controls">
        <button class="btn" id="prevBtn">‹</button>
        <div class="dots" id="dots"></div>
        <button class="btn" id="nextBtn">›</button>
    </div>
</div>

<script>
    const track = document.getElementById('track');
    const slides = document.querySelectorAll('.slide');
    const dotsContainer = document.getElementById('dots');
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');

    let currentIndex = 0;
    const totalSlides = slides.length;

    // Создаём точки
    for (let i = 0; i < totalSlides; i++) {
        const dot = document.createElement('button');
        dot.className = 'dot' + (i === 0 ? ' active' : '');
        dot.setAttribute('data-index', i);
        dot.addEventListener('click', () => goTo(i));
        dotsContainer.appendChild(dot);
    }

    const dots = dotsContainer.querySelectorAll('.dot');

    function updateCarousel() {
        const offset = -currentIndex * 100;
        track.style.transform = `translateX(${offset}%)`;
        dots.forEach((dot, index) => {
            dot.classList.toggle('active', index === currentIndex);
        });
    }

    function goTo(index) {
        if (index < 0) index = totalSlides - 1;
        if (index >= totalSlides) index = 0;
        currentIndex = index;
        updateCarousel();
    }

    prevBtn.addEventListener('click', () => goTo(currentIndex - 1));
    nextBtn.addEventListener('click', () => goTo(currentIndex + 1));

    // Клавиши
    document.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowLeft') goTo(currentIndex - 1);
        if (e.key === 'ArrowRight') goTo(currentIndex + 1);
    });

    updateCarousel();
</script>

</body>
</html>
