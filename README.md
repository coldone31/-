[Uploadin<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>369게임</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
        }
        button {
            margin: 5px;
            padding: 10px 20px;
        }
        .buttons {
            display: flex;
            margin-top: 10px;
        }
        #result {
            font-size: 1.2em;
            margin-top: 10px;
        }
        #score {
            font-size: 1.2em;
            margin-top: 10px;
            color: blue;
        }
    </style>
</head>
<body>
    <h1><b><ins>369 게임</ins></b></h1>
    <h4><b><i><ins><mark>규칙: 2초 안에 정답을 맞추어야 함!</mark></ins></i></b></h4>
    <h2><div id="number-display">숫자: </div></h2>
    
    <div class="buttons">
        <button id="start-button"><b><mark>게임 시작</mark></b></button>
    </div>

    <div class="buttons">
        <button class="clap-button">0 박수</button>
        <button class="clap-button">1 박수</button>
        <button class="clap-button">2 박수</button>
        <button class="clap-button">3 박수</button>
    </div>

    <div id="result"></div>
    <div id="score">점수: 0</div>
    
    <div class="buttons">
        <button id="end-button" style="display: none;"><b><mark>게임 종료</mark></b></button>
    </div>

    <script>
        let score = 0; // 점수를 저장할 변수
        let isAnswered = false; // 사용자가 답을 제출했는지 여부

        // 0부터 999 사이의 랜덤한 숫자를 생성하는 함수
        const generateRandomNumber = () => Math.floor(1000 * Math.random());

        // 숫자에 포함된 3, 6, 9의 개수를 세는 함수
        const countClaps = (number) => {
            let count = 0;
            let numberStr = number.toString();
            for (let i = 0; i < numberStr.length; i++) {
                let currentNumber = numberStr[i];
                if (['3', '6', '9'].includes(numberStr[i])) {
                    count++;
                }
            }
            return count;
        }

        // 새로운 게임을 시작하는 함수
        const newGame = () => {
            let number = generateRandomNumber();
            let clapCount = countClaps(number);
            document.querySelector('#number-display').textContent = `숫자: ${number}`;
            document.querySelector('#number-display').dataset.clapCount = clapCount;
            document.querySelector('#result').textContent = '';
            isAnswered = false;
        }

        // 사용자가 선택한 답을 확인하는 함수
        const checkAnswer = (userAnswer) => {
            isAnswered = true;
            let correctAnswer = parseInt(document.querySelector('#number-display').dataset.clapCount);
            let result = document.querySelector('#result');
            let message;
            let color;

            if (userAnswer === correctAnswer) {
                message = '정답입니다!';
                color = 'green';
                score++;
            } else {
                message = `오답입니다. 정답은 ${correctAnswer} 박수입니다.`;
                color = 'red';
                score--;
            }

            result.textContent = message;
            result.style.color = color;
            updateScore();
            setTimeout(newGame, 2000);
        }

        // 게임을 초기화하는 함수
        const resetGame = () => {
            score = 0;
            document.querySelector('#number-display').textContent = '숫자: ';
            document.querySelector('#result').textContent = '';
            updateScore();
        }

        // 게임을 시작하는 함수
        const startGame = () => {
            newGame();
            document.querySelector('#start-button').style.display = 'none';
            document.querySelector('#end-button').style.display = 'block';
        }

        // 게임을 종료하는 함수
        const endGame = () => {
            resetGame();
            document.querySelector('#start-button').style.display = 'block';
            document.querySelector('#end-button').style.display = 'none';
            isAnswered = true;
        }

        // 점수가 변경될 시 업데이트 되는 함수
        const updateScore = () => {
            document.querySelector('#score').textContent = `점수: ${score}`;
        }

        // 게임 시작 버튼 클릭 시 실행 이벤트 리스너
        document.querySelector('#start-button').addEventListener('click', startGame);

        // 게임 종료 버튼 클릭 시 실행 이벤트 리스너
        document.querySelector('#end-button').addEventListener('click', endGame);

        // 각 박수 버튼 클릭 시 실행 이벤트 리스너
        document.querySelectorAll('.clap-button').forEach((button, index) => {
            button.addEventListener('click', () => {
                checkAnswer(index);
            });
        });
    </script>
</body>
</html>
