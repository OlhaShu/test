```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Integrated Systems Flashcards</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        #app {
            text-align: center;
        }
        #card-container {
            perspective: 1000px;
            width: 300px;
            height: 200px;
            margin: 20px auto;
            position: relative;
        }
        .card {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-style: preserve-3d;
            transition: transform 0.6s ease-in-out;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            border-radius: 8px;
        }
        .card.flipped {
            transform: rotateY(180deg);
        }
        .front, .back {
            position: absolute;
            width: 100%;
            height: 100%;
            backface-visibility: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
            box-sizing: border-box;
            border-radius: 8px;
        }
        .front {
            background-color: #ffffff;
            font-weight: bold;
        }
        .back {
            background-color: #e0e0e0;
            transform: rotateY(180deg);
        }
        .buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            opacity: 0.9;
        }
        #shuffle-btn {
            background-color: #4CAF50;
            color: white;
        }
        #learned-btn {
            background-color: #2196F3;
            color: white;
        }
        #flip-btn {
            background-color: #FFC107;
            color: black;
        }
        #next-btn {
            background-color: #FF5722;
            color: white;
        }
        #card-count {
            margin-top: 10px;
        }
        .animate-fade {
            animation: fade 0.5s;
        }
        @keyframes fade {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .animate-slide-out {
            animation: slideOut 0.5s forwards;
        }
        @keyframes slideOut {
            from { transform: translateX(0); opacity: 1; }
            to { transform: translateX(100%); opacity: 0; }
        }
    </style>
</head>
<body>
    <div id="app">
        <h1 id="title">Flashcards: Integrated Systems</h1>
        <div id="card-container">
            <div id="card" class="card">
                <div class="front" id="front"></div>
                <div class="back" id="back"></div>
            </div>
        </div>
        <div class="buttons">
            <button id="flip-btn">Flip</button>
            <button id="next-btn">Next</button>
            <button id="shuffle-btn">Shuffle</button>
            <button id="learned-btn">Mark as Learned</button>
        </div>
        <p id="card-count">Cards left: 10</p>
    </div>

    <script>
        const data = {
            ui: {
                en: {
                    title: 'Flashcards: Integrated Systems',
                    flip: 'Flip',
                    next: 'Next',
                    shuffle: 'Shuffle',
                    learned: 'Mark as Learned',
                    count: 'Cards left: '
                },
                uk: {
                    title: 'Флеш-картки: Інтегровані системи',
                    flip: 'Перевернути',
                    next: 'Наступна',
                    shuffle: 'Перемішати',
                    learned: 'Позначити як вивчене',
                    count: 'Карток залишилося: '
                }
            },
            cards: [
                {
                    front: {
                        en: 'What is an integrated system?',
                        uk: 'Що таке інтегрована система?'
                    },
                    back: {
                        en: 'A system that combines various functions and processes of an organization to ensure more efficient operation.',
                        uk: 'Система, яка об\'єднує різні функції та процеси організації з метою забезпечення більш ефективної роботи.'
                    }
                },
                {
                    front: {
                        en: 'What is system audit?',
                        uk: 'Що таке аудит системи?'
                    },
                    back: {
                        en: 'One of the most effective methods of quality control; auditors evaluate effectiveness by checking documentation, procedures, and results.',
                        uk: 'Аудит є одним з найефективніших методів контролю якості інтегрованої системи. Аудитори проводять оцінку ефективності системи, перевіряючи документацію, виконання процедур та результати роботи.'
                    }
                },
                {
                    front: {
                        en: 'What is measurement and analysis of results?',
                        uk: 'Що таке вимірювання та аналіз результатів?'
                    },
                    back: {
                        en: 'Collecting and analyzing data to identify problems and find solutions.',
                        uk: 'Для забезпечення якості інтегрованої системи необхідно збирати дані та проводити їх аналіз. Це допомагає ідентифікувати проблеми та знайти способи їх вирішення.'
                    }
                },
                {
                    front: {
                        en: 'What is internal control?',
                        uk: 'Що таке внутрішній контроль?'
                    },
                    back: {
                        en: 'A system of procedures and policies for effective process execution and risk optimization, including self-checks.',
                        uk: 'Внутрішній контроль - це система процедур та політик, яка розробляється організацією для забезпечення ефективного виконання процесів та оптимізації ризиків. Цей метод включає в себе проведення самоперевірок та оцінок системи.'
                    }
                },
                {
                    front: {
                        en: 'What is external control?',
                        uk: 'Що таке зовнішній контроль?'
                    },
                    back: {
                        en: 'Evaluation by independent organizations to ensure compliance with standards.',
                        uk: 'Зовнішній контроль - це проведення оцінки системи з боку незалежних організацій. Цей метод контролю допомагає забезпечити незалежну оцінку системи та її відповідність стандартам.'
                    }
                },
                {
                    front: {
                        en: 'What is system testing?',
                        uk: 'Що таке тестування системи?'
                    },
                    back: {
                        en: 'A process to determine functionality and compliance with requirements, can be manual or automatic.',
                        uk: 'Тестування - це процес, який допомагає визначити функціональність системи та її відповідність вимогам. Тестування може бути проведене вручну або автоматично.'
                    }
                },
                {
                    front: {
                        en: 'What is Continuous Integration (CI)?',
                        uk: 'Що таке Continuous Integration (CI)?'
                    },
                    back: {
                        en: 'An approach to software development involving constant integration of code changes to quickly detect errors and ensure high quality.',
                        uk: 'Continuous Integration (CI) - це підхід до розробки програмного забезпечення, який полягає у постійному злитті (інтеграції) змін до вихідного коду з метою швидкого виявлення помилок та забезпечення високої якості продукту.'
                    }
                },
                {
                    front: {
                        en: 'Advantages of Continuous Integration?',
                        uk: 'Переваги Continuous Integration?'
                    },
                    back: {
                        en: 'Faster reaction to changes; reduced risk; high quality; lower costs; shorter release time.',
                        uk: 'Швидша реакція на зміни в коді; Зменшення ризику з\'єднання з розробкою; Забезпечення високої якості програмного забезпечення; Зменшення витрат на тестування та налагодження; Зменшення часу, необхідного для випуску нової версії програмного забезпечення.'
                    }
                },
                {
                    front: {
                        en: 'What is Continuous Delivery (CD)?',
                        uk: 'Що таке Continuous Delivery (CD)?'
                    },
                    back: {
                        en: 'An approach for automatic release of new versions, automating testing, build, delivery, and deployment.',
                        uk: 'Continuous Delivery (CD) - це підхід до розробки програмного забезпечення, який полягає у постійному автоматичному випуску (доставці) нових версій продукту.'
                    }
                },
                {
                    front: {
                        en: 'Difference between CI and CD?',
                        uk: 'Основна різниця між CI та CD?'
                    },
                    back: {
                        en: 'CI focuses on code quality and error reduction; CD on automation of deployment and release.',
                        uk: 'Основна різниця між CI та CD полягає в тому, що CI спрямований на забезпечення сталої якості коду та зниження кількості помилок, тоді як CD фокусується на максимальній автоматизації процесу розгортання програмного забезпечення та його випуску.'
                    }
                }
            ]
        };

        const lang = navigator.language.startsWith('uk') ? 'uk' : 'en';
        const ui = data.ui[lang];
        let deck = [...data.cards];
        let currentIndex = 0;
        let isFlipped = false;

        const titleEl = document.getElementById('title');
        const frontEl = document.getElementById('front');
        const backEl = document.getElementById('back');
        const cardEl = document.getElementById('card');
        const countEl = document.getElementById('card-count');
        const flipBtn = document.getElementById('flip-btn');
        const nextBtn = document.getElementById('next-btn');
        const shuffleBtn = document.getElementById('shuffle-btn');
        const learnedBtn = document.getElementById('learned-btn');

        titleEl.textContent = ui.title;
        flipBtn.textContent = ui.flip;
        nextBtn.textContent = ui.next;
        shuffleBtn.textContent = ui.shuffle;
        learnedBtn.textContent = ui.learned;
        countEl.textContent = ui.count + deck.length;

        function showCard() {
            if (deck.length === 0) {
                frontEl.textContent = lang === 'uk' ? 'Немає більше карток' : 'No more cards';
                backEl.textContent = '';
                return;
            }
            const card = deck[currentIndex];
            frontEl.textContent = card.front[lang];
            backEl.textContent = card.back[lang];
            cardEl.classList.remove('flipped');
            isFlipped = false;
            cardEl.classList.add('animate-fade');
            setTimeout(() => cardEl.classList.remove('animate-fade'), 500);
        }

        function shuffleDeck() {
            deck.sort(() => Math.random() - 0.5);
            currentIndex = 0;
            showCard();
            cardEl.classList.add('animate-fade');
            setTimeout(() => cardEl.classList.remove('animate-fade'), 500);
        }

        function markLearned() {
            if (deck.length === 0) return;
            cardEl.classList.add('animate-slide-out');
            setTimeout(() => {
                deck.splice(currentIndex, 1);
                if (currentIndex >= deck.length) currentIndex = 0;
                showCard();
                countEl.textContent = ui.count + deck.length;
                cardEl.classList.remove('animate-slide-out');
            }, 500);
        }

        flipBtn.addEventListener('click', () => {
            cardEl.classList.toggle('flipped');
            isFlipped = !isFlipped;
        });

        nextBtn.addEventListener('click', () => {
            currentIndex = (currentIndex + 1) % deck.length;
            showCard();
        });

        shuffleBtn.addEventListener('click', shuffleDeck);

        learnedBtn.addEventListener('click', markLearned);

        // Initial show
        showCard();
    </script>
</body>
</html>
```
