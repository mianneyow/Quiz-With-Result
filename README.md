# Quiz App Documentation

This repository contains a browser-based Filipino-themed quiz application. It is divided into several functions to manage state, load questions, track scores, and provide interactive feedback.

## 🚀 Features Implemented

- ✅ Topic-based quiz selection
- ✅ Dynamic quiz flow (Back, Next, Submit)
- ✅ User answer tracking
- ✅ Score calculation & display
- ✅ Question randomization
- ✅ Image questions support
- ✅ Alert for empty choices
- ✅ Confirmation pop up before submission
- ✅ Results page with incorrect answer highlighting
- ✅ Restart and Return to Menu navigation

---

## 🔧 Developer Notes

- Questions are organized by topic ID (PPC, PMC, KB, AYATP)

- You can add more questions to the questions[] array, but keep the same format. user_choice is used to track user input and validate during results, keep it empty in the questions[] array.

- Images must be stored in the img/questions/ directory

- Scoring is based on correct answers and capped at MAX_QUESTIONS per topic

---

## 🖼️ Notes for Frontend Developers

These are functions and elements that are safe to **change**, **style**, **customize**, or **enhance with UI animations and effects** in the `script.js` file. They will not affect quiz logic (except for when you add **style.display = "none" || "block"** to them since they hide and display the elements). 

### DOM Elements By ID
```js
const elements = {
    menu, quiz, result, // divs
    questionNumber, // h2
    questionText, // p
    questionImg, // img
    choice1, choice2, choice3, choice4, // button
    backButton, nextButton, // button
    finalScore, // h3 
    returnMenu, restart, // button
    correctAnswersDiv // div container for generated custom tags in result
};
```
Use these for:
- Styling containers (menu, quiz, result)
- Customizing buttons (nextButton, backButton, restart, etc.)
- Modifying question display (questionText, quastionImg, choices)

### Generated Component for Incorrect Answers
For adding classes and changing styles of incorrect answers in the result state, go to func `displayCorrectAnswers()` and modify the generated component's innerHTML as needed.

- `${element["choices"][element["answer"].charCodeAt(0) - 65]}`  
  Converts answer and user_choice **(A, B, C, D)** to its integer value **[0, 1, 2, 3]** for indexing the choices arraay **choices: ["Diagonal Way", "Batch Process", "Iterative Process", "Step-by-Step Process"]**.

```js
// For changing background-color
let isCorrect = element["answer"] === element["user_choice"]; 

// ADD CLASSES AND CHANGE STYLES FOR FRONTEND
correctAnswerComponent.innerHTML = `
<div class="" style="background-color: ${isCorrect? "green" : "red"}">
    <h2 class="">${currentQuestionList.indexOf(element) + 1}</h2>
    <p class="">${element["question"]}</p>
    <img class="img-fluid" src="${element["img_src"]? element["img_src"] : "" }" alt="">
    <div class=""><h5>Correct Answer:</h5> ${element["choices"][element["answer"].charCodeAt(0) - 65]}</div>
    <div class=""><h5>Your Answer:</h5> ${element["choices"][element["user_choice"].charCodeAt(0) - 65]}</div>
</div>
`;
```

### App State Display
- `changeDivDisplay(state)`  
  Shows the correct section (`menu`, `quiz`, or `result`) based on the current state.

- `displayMenuScores()`  
  Update scores value in the menu for each topic from the internal `scores` object.

### Dynamic Component Displays
- `handleQuizState()`  
 Toggles visibility of `backButton`. Updates the button text dynamically (e.g., `"Next"` or `"Submit"`).

- `displayCorrectAnswers()`   
  Generates and displays multiple custom tags contiaining quiz content after submission. Each item is highlighted if they are correct or incorrect.
  
### Visual Feedback
- `event.target.matches(".choices")`   
Highlights selected answers in the quiz (`green`).

- `resetHighlight()`  
  Clears the selected choice highlight (e.g., removes green background).

- `alert("ERROR: Please Choose An Answer")` AND `confirm("Are you sure you want to submit?")`   
  Not sure if pwede sila imodify? pop up sila or something.
---

## 🧠 Backend (Logic-Only) Functions

These functions handle **quiz logic**, **state management**, and **data processing**.

### State Flow 
- `changeDivContent()`  
  Changes content based on `appState` (menu, quiz, results).

### Menu Topics & Setup
- `setChosenTopic()`   
  Creates array of filtered questions by `topic_id`, shuffles them, and prepares the quiz variables.
  

### Quiz Flow & Utils
- `handleQuizState()`  
  Controls quiz progression logic: shows next question or moves to result state.

- `shuffle(array)`  
  Randomizes question order using Fisher-Yates algorithm.

- `loadNextQuestion()`  
  Loads and displays the next question and choices. Increases questionsAsked variable count. Calls `checkAnswer()` each display.

- `prevQuestion()`   
  Loads previous question and resets choices. Seduces score if previous choice was correct.

- `checkAnswer()`  
  Ensures an answer was chosen, validates the selected answer, and updates score logic (including `user_choice`). Calls `loadNextQuestion()` and handles confirmation when submitting at the end of the quiz.

### Results State & Score
- `displayCorrectAnswers()`   
  Generates and displays multiple custom tags contiaining quiz content after submission. Each item is highlighted if they are correct or incorrect.

- `saveScore()`  
  Compares and stores the highest score to the appropriate topic category (`scores[topic]`) and displays current score in the results section.

- `event.target === elements.restart`
  Resets values for quiz, button visibility & text, shuffles question list, and returns to quiz state.

- `event.target === elements.returnMenu`   
  Calls `displayMenuScores()` to update highest score, resets score count, and change to menu state.

---
## 📂 Project Structure

This codebase is implemented in pure JavaScript, HTML, and CSS. All logic related to state transitions and event handling is included in a single JS file.

```css
project-root/
│
├── img/
│   └── questions/
│       └── bea.jpg
│       ├── Kath.png
│       └── mechado-ata.jpg
│
├── index.html
├── style.css
└── script.js

```

---
## 🧩 App States

```js
const MENU = 0, QUIZ = 1, RESULTS = 2;
let appState = 0;
```

- `MENU` - Displays quiz categories and highest scores per topic
- `QUIZ` - Shows the current question and choices
- `RESULTS` - Displays final score and correct answers. (each question light up green or red depending on user's answer)

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/aaronbolado/quiz-application.git
cd quiz-application
```
