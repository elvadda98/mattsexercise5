<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Italian Vocabulary Game</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a93cb 0%, #a4bfef 100%);
            min-height: 100vh;
            padding: 20px;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .container {
            max-width: 1000px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0,0,0,0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
        }

        .header h1 {
            font-size: 2.2em;
            margin-bottom: 10px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.2em;
            opacity: 0.9;
            position: relative;
            z-index: 1;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 10px;
            padding: 20px;
            background: #f8f9fa;
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 12px 24px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #666;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-1px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #e8f4f2);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 25px;
            border: 2px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .word-bank h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            text-align: center;
            font-size: 1.4em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 12px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 10px 18px;
            border-radius: 20px;
            font-weight: bold;
            font-size: 16px;
            box-shadow: 0 3px 10px rgba(74, 111, 165, 0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(74, 111, 165, 0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 20px;
            border-left: 5px solid #4a6fa5;
            box-shadow: 0 4px 15px rgba(0,0,0,0.05);
        }

        .question h3 {
            color: #4a6fa5;
            margin-bottom: 15px;
            font-size: 1.3em;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 8px;
            padding: 8px 12px;
            font-size: 16px;
            margin: 0 5px;
            min-width: 120px;
            transition: border-color 0.3s ease;
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }

        .option {
            padding: 15px 20px;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .option:hover {
            border-color: #4a6fa5;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .option.selected {
            background: #4a6fa5;
            color: white;
            border-color: #4a6fa5;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 20px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 15px;
            color: #4a6fa5;
            font-size: 1.2em;
        }

        .match-item {
            padding: 15px;
            margin: 8px 0;
            border: 2px solid #ddd;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
        }

        .match-item:hover {
            border-color: #4a6fa5;
            transform: translateY(-1px);
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.2);
        }

        .match-item.selected {
            background: #e8f4f2;
            border-color: #4a6fa5;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
        }

        .check-btn {
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 20px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
        }

        .check-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(74, 111, 165, 0.4);
        }

        .reset-btn {
            background: linear-gradient(135deg, #f44336, #ff7961);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            margin: 10px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(244, 67, 54, 0.3);
        }

        .reset-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(244, 67, 54, 0.4);
        }

        .feedback {
            margin-top: 15px;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from { transform: translateX(-20px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: linear-gradient(135deg, #4a6fa5, #6a93cb);
            color: white;
            padding: 15px 20px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 15px rgba(74, 111, 165, 0.3);
            z-index: 1000;
        }

        .icon {
            font-size: 24px;
            margin-right: 10px;
            vertical-align: middle;
        }

        /* Vocabulary List Styles */
        .vocabulary-list {
            padding: 20px;
        }

        .vocabulary-item {
            margin-bottom: 25px;
            padding: 20px;
            border-radius: 10px;
            background: #f8f9fa;
            box-shadow: 0 4px 10px rgba(0,0,0,0.05);
        }

        .vocabulary-item h3 {
            color: #4a6fa5;
            margin-bottom: 10px;
            font-size: 1.4em;
            border-bottom: 2px solid #4a6fa5;
            padding-bottom: 8px;
        }

        .vocabulary-item p {
            margin-bottom: 8px;
            line-height: 1.6;
        }

        .example {
            font-style: italic;
            color: #555;
            padding-left: 15px;
            border-left: 3px solid #4a6fa5;
            margin-top: 10px;
        }

        .italian-flag {
            display: inline-block;
            width: 20px;
            height: 15px;
            background: linear-gradient(to right, #009246 33%, #FFFFFF 33%, #FFFFFF 66%, #CE2B37 66%);
            margin-right: 8px;
            border-radius: 2px;
            vertical-align: middle;
        }

        .conjugation {
            background: #e8f4f8;
            padding: 12px;
            border-radius: 8px;
            margin-top: 8px;
            font-size: 0.9em;
        }

        .conjugation p {
            margin: 3px 0;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 20px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 200px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: block;
                width: fit-content;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span id="score">0</span>/12</div>
    
    <div class="container">
        <div class="header">
            <h1><i class="fas fa-book icon"></i> Italian Vocabulary Game</h1>
            <p>Learn and practice these Italian words and phrases!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('vocabulary')">Vocabulary List</button>
            <button class="nav-btn" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Translations</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Vocabulary List Section -->
        <div id="vocabulary" class="game-section active">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Italian Vocabulary</h2>
            
            <div class="vocabulary-list">
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>parole</h3>
                    <p><strong>Translation:</strong> words</p>
                    <p><strong>Part of Speech:</strong> noun (feminine plural)</p>
                    <p><strong>Singular:</strong> parola</p>
                    <p class="example"><strong>Example:</strong> "Non conosco queste parole italiane." (I don't know these Italian words.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>sviluppare</h3>
                    <p><strong>Translation:</strong> to develop, to expand</p>
                    <p><strong>Part of Speech:</strong> verb</p>
                    <div class="conjugation">
                        <p><strong>Conjugation (present):</strong></p>
                        <p>io sviluppo, tu sviluppi, lui/lei sviluppa</p>
                        <p>noi sviluppiamo, voi sviluppate, loro sviluppano</p>
                    </div>
                    <p class="example"><strong>Example:</strong> "Voglio sviluppare le mie capacità linguistiche." (I want to develop my language skills.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>creare</h3>
                    <p><strong>Translation:</strong> to create, to make</p>
                    <p><strong>Part of Speech:</strong> verb</p>
                    <div class="conjugation">
                        <p><strong>Conjugation (present):</strong></p>
                        <p>io creo, tu crei, lui/lei crea</p>
                        <p>noi creiamo, voi create, loro creano</p>
                    </div>
                    <p class="example"><strong>Example:</strong> "Mi piace creare arte con diversi materiali." (I like to create art with different materials.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>genere</h3>
                    <p><strong>Translation:</strong> gender, genre, type</p>
                    <p><strong>Part of Speech:</strong> noun (masculine)</p>
                    <p class="example"><strong>Example:</strong> "Quale genere di musica preferisci?" (What genre of music do you prefer?)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>uguale</h3>
                    <p><strong>Translation:</strong> equal, same</p>
                    <p><strong>Part of Speech:</strong> adjective</p>
                    <p class="example"><strong>Example:</strong> "I due fratelli sono uguali." (The two brothers are the same.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>apprezzo</h3>
                    <p><strong>Translation:</strong> I appreciate, I value</p>
                    <p><strong>Part of Speech:</strong> verb (1st person singular)</p>
                    <p><strong>Infinitive:</strong> apprezzare</p>
                    <p class="example"><strong>Example:</strong> "Apprezzo molto il tuo aiuto." (I really appreciate your help.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>provare</h3>
                    <p><strong>Translation:</strong> to try, to attempt, to test</p>
                    <p><strong>Part of Speech:</strong> verb</p>
                    <div class="conjugation">
                        <p><strong>Conjugation (present):</strong></p>
                        <p>io provo, tu provi, lui/lei prova</p>
                        <p>noi proviamo, voi provate, loro provano</p>
                    </div>
                    <p class="example"><strong>Example:</strong> "Devo provare questa nuova ricetta." (I need to try this new recipe.)</p>
                </div>
                
                <div class="vocabulary-item">
                    <h3><span class="italian-flag"></span>insegnante</h3>
                    <p><strong>Translation:</strong> teacher</p>
                    <p><strong>Part of Speech:</strong> noun (masculine/feminine)</p>
                    <p><strong>Note:</strong> Can be used for both male and female teachers</p>
                    <p class="example"><strong>Example:</strong> "La mia insegnante di italiano è molto brava." (My Italian teacher is very good.)</p>
                </div>
            </div>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section">
            <div class="word-bank">
                <h3><i class="fas fa-list-ul icon"></i> Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">parole</span>
                    <span class="word-option">sviluppare</span>
                    <span class="word-option">creare</span>
                    <span class="word-option">genere</span>
                    <span class="word-option">uguale</span>
                    <span class="word-option">apprezzo</span>
                    <span class="word-option">provare</span>
                    <span class="word-option">insegnante</span>
                </div>
            </div>

            <div id="fill-gaps-container">
                <!-- Sentences will be dynamically inserted here -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
            <button class="reset-btn" onclick="resetFillBlanks()">Reset Answers</button>
        </div>

        <!-- Matching Section -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 20px; color: #4a6fa5;">Match the Italian words with their English translations</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Italian Words</h4>
                    <div class="match-item" data-word="parole" onclick="selectMatch(this)">parole</div>
                    <div class="match-item" data-word="sviluppare" onclick="selectMatch(this)">sviluppare</div>
                    <div class="match-item" data-word="creare" onclick="selectMatch(this)">creare</div>
                    <div class="match-item" data-word="genere" onclick="selectMatch(this)">genere</div>
                    <div class="match-item" data-word="uguale" onclick="selectMatch(this)">uguale</div>
                    <div class="match-item" data-word="apprezzo" onclick="selectMatch(this)">apprezzo</div>
                    <div class="match-item" data-word="provare" onclick="selectMatch(this)">provare</div>
                    <div class="match-item" data-word="insegnante" onclick="selectMatch(this)">insegnante</div>
                </div>
                <div class="match-column">
                    <h4>English Translations</h4>
                    <div id="meanings-container">
                        <!-- Meanings will be dynamically inserted here -->
                    </div>
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
            <button class="check-btn" onclick="checkMatching()">Check Matching</button>
            <button class="reset-btn" onclick="resetMatching()">Reset Matching</button>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What does "parole" mean?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Sentences</div>
                    <div class="option" onclick="selectOption(this, true)">Words</div>
                    <div class="option" onclick="selectOption(this, false)">Letters</div>
                    <div class="option" onclick="selectOption(this, false)">Books</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: "Sviluppare" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">To destroy</div>
                    <div class="option" onclick="selectOption(this, true)">To develop or expand</div>
                    <div class="option" onclick="selectOption(this, false)">To sleep</div>
                    <div class="option" onclick="selectOption(this, false)">To eat</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: "Creare" is similar to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">To break</div>
                    <div class="option" onclick="selectOption(this, true)">To make or create</div>
                    <div class="option" onclick="selectOption(this, false)">To buy</div>
                    <div class="option" onclick="selectOption(this, false)">To sell</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: "Genere" can refer to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Only music types</div>
                    <div class="option" onclick="selectOption(this, true)">Gender, genre, or type</div>
                    <div class="option" onclick="selectOption(this, false)">Only people</div>
                    <div class="option" onclick="selectOption(this, false)">Only colors</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5: If something is "uguale", it is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Different</div>
                    <div class="option" onclick="selectOption(this, true)">Equal or the same</div>
                    <div class="option" onclick="selectOption(this, false)">Better</div>
                    <div class="option" onclick="selectOption(this, false)">Worse</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6: "Apprezzo" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">I hate</div>
                    <div class="option" onclick="selectOption(this, true)">I appreciate or value</div>
                    <div class="option" onclick="selectOption(this, false)">I forget</div>
                    <div class="option" onclick="selectOption(this, false)">I remember</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Question 7: "Provare" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">To succeed</div>
                    <div class="option" onclick="selectOption(this, true)">To try or attempt</div>
                    <div class="option" onclick="selectOption(this, false)">To finish</div>
                    <div class="option" onclick="selectOption(this, false)">To start</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>
            
            <div class="question">
                <h3>Question 8: An "insegnante" is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">A student</div>
                    <div class="option" onclick="selectOption(this, true)">A teacher</div>
                    <div class="option" onclick="selectOption(this, false)">A book</div>
                    <div class="option" onclick="selectOption(this, false)">A school</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>
            
            <button class="reset-btn" onclick="resetMultipleChoice()">Reset Answers</button>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Track correct answers for each section
        let fillBlanksCorrect = 0;
        let matchingCorrect = 0;
        let multipleChoiceCorrect = 0;
        
        // Definitions for the matching game
        const definitions = [
            { meaning: "parole", text: "Words" },
            { meaning: "sviluppare", text: "To develop, to expand" },
            { meaning: "creare", text: "To create, to make" },
            { meaning: "genere", text: "Gender, genre, type" },
            { meaning: "uguale", text: "Equal, same" },
            { meaning: "apprezzo", text: "I appreciate, I value" },
            { meaning: "provare", text: "To try, to attempt, to test" },
            { meaning: "insegnante", text: "Teacher" }
        ];

        // Sentences for the fill-in-the-blanks game
        const sentences = [
            { text: "Non conosco queste <input type='text' class='fill-blank' data-answer='parole' placeholder='answer'> italiane.", answer: "parole" },
            { text: "Voglio <input type='text' class='fill-blank' data-answer='sviluppare' placeholder='answer'> le mie capacità linguistiche.", answer: "sviluppare" },
            { text: "Mi piace <input type='text' class='fill-blank' data-answer='creare' placeholder='answer'> arte con diversi materiali.", answer: "creare" },
            { text: "Quale <input type='text' class='fill-blank' data-answer='genere' placeholder='answer'> di musica preferisci?", answer: "genere" },
            { text: "I due fratelli sono <input type='text' class='fill-blank' data-answer='uguali' placeholder='answer'>.", answer: "uguali" },
            { text: "<input type='text' class='fill-blank' data-answer='Apprezzo' placeholder='answer'> molto il tuo aiuto.", answer: "Apprezzo" },
            { text: "Devo <input type='text' class='fill-blank' data-answer='provare' placeholder='answer'> questa nuova ricetta.", answer: "provare" },
            { text: "La mia <input type='text' class='fill-blank' data-answer='insegnante' placeholder='answer'> di italiano è molto brava.", answer: "insegnante" },
            { text: "Devo imparare nuove <input type='text' class='fill-blank' data-answer='parole' placeholder='answer'> ogni giorno.", answer: "parole" },
            { text: "La società cerca di <input type='text' class='fill-blank' data-answer='sviluppare' placeholder='answer'> nuove tecnologie.", answer: "sviluppare" },
            { text: "Vuoi <input type='text' class='fill-blank' data-answer='provare' placeholder='answer'> a parlare italiano con me?", answer: "provare" },
            { text: "L'<input type='text' class='fill-blank' data-answer='insegnante' placeholder='answer'> spiega la lezione molto chiaramente.", answer: "insegnante" }
        ];

        // Function to shuffle array (Fisher-Yates algorithm)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // Initialize the fill-in-the-blanks game with shuffled sentences
        function initFillBlanks() {
            const container = document.getElementById('fill-gaps-container');
            container.innerHTML = '';
            
            // Shuffle the sentences
            const shuffledSentences = shuffleArray([...sentences]);
            
            // Create and append the sentence elements
            shuffledSentences.forEach((sentence, index) => {
                const div = document.createElement('div');
                div.className = 'question';
                div.innerHTML = `
                    <h3>Question ${index + 1}:</h3>
                    <p>${sentence.text}</p>
                    <div class="feedback" id="feedback-${index + 1}" style="display: none;"></div>
                `;
                container.appendChild(div);
            });
        }

        // Initialize the matching game with shuffled definitions
        function initMatchingGame() {
            const meaningsContainer = document.getElementById('meanings-container');
            meaningsContainer.innerHTML = '';
            
            // Shuffle the definitions
            const shuffledDefinitions = shuffleArray([...definitions]);
            
            // Create and append the definition elements
            shuffledDefinitions.forEach(def => {
                const div = document.createElement('div');
                div.className = 'match-item';
                div.setAttribute('data-meaning', def.meaning);
                div.onclick = function() { selectMatch(this); };
                div.textContent = def.text;
                meaningsContainer.appendChild(div);
            });
        }

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If showing fill-in-the-blanks section, reinitialize with shuffled sentences
            if (sectionId === 'fill-gaps') {
                initFillBlanks();
            }
            
            // If showing matching section, reinitialize with shuffled definitions
            if (sectionId === 'matching') {
                initMatchingGame();
                // Reset matching game state
                document.querySelectorAll('.match-item').forEach(item => {
                    item.classList.remove('selected', 'matched');
                });
                selectedWord = null;
                selectedMeaning = null;
                matchedPairs = [];
                document.getElementById('matching-feedback').style.display = 'none';
            }
            
            // Update score display
            updateScore();
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank, index) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
                
                // Show feedback for each question
                const feedback = document.getElementById(`feedback-${index+1}`);
                if (userAnswer === correctAnswer) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${blank.dataset.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            fillBlanksCorrect = correctCount;
            updateScore();
        }

        function resetFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            blanks.forEach(blank => {
                blank.value = '';
                blank.classList.remove('correct', 'incorrect');
            });
            
            const feedbacks = document.querySelectorAll('#fill-gaps .feedback');
            feedbacks.forEach(feedback => {
                feedback.style.display = 'none';
            });
            
            fillBlanksCorrect = 0;
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === definitions.length) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            matchingCorrect = matchedPairs.length;
            updateScore();
        }

        function checkMatching() {
            const allMatches = document.querySelectorAll('.match-item[data-word]');
            let allCorrect = true;
            
            allMatches.forEach(item => {
                if (!item.classList.contains('matched')) {
                    allCorrect = false;
                }
            });
            
            const feedback = document.getElementById('matching-feedback');
            if (allCorrect) {
                feedback.textContent = '🎉 Excellent! All matches are correct!';
                feedback.className = 'feedback correct';
            } else {
                const unmatchedCount = definitions.length - matchedPairs.length;
                feedback.textContent = `You have ${unmatchedCount} unmatched pair${unmatchedCount !== 1 ? 's' : ''}. Keep trying!`;
                feedback.className = 'feedback incorrect';
            }
            feedback.style.display = 'block';
        }

        function resetMatching() {
            document.querySelectorAll('.match-item').forEach(item => {
                item.classList.remove('selected', 'matched');
            });
            
            initMatchingGame();
            
            selectedWord = null;
            selectedMeaning = null;
            matchedPairs = [];
            
            document.getElementById('matching-feedback').style.display = 'none';
            
            matchingCorrect = 0;
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
                opt.classList.remove('correct');
                opt.classList.remove('incorrect');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            
            // Count correct answers in multiple choice
            multipleChoiceCorrect = document.querySelectorAll('#multiple-choice .option.correct.selected').length;
            updateScore();
        }

        function resetMultipleChoice() {
            document.querySelectorAll('#multiple-choice .option').forEach(option => {
                option.classList.remove('selected', 'correct', 'incorrect');
            });
            
            document.querySelectorAll('#multiple-choice .feedback').forEach(feedback => {
                feedback.style.display = 'none';
            });
            
            multipleChoiceCorrect = 0;
            updateScore();
        }

        function updateScore() {
            // Total score
            score = fillBlanksCorrect + matchingCorrect + multipleChoiceCorrect;
            document.getElementById('score').textContent = score;
        }
        
        // Initialize the page
        window.onload = function() {
            initFillBlanks();
            initMatchingGame();
        }
    </script>
</body>
</html>
