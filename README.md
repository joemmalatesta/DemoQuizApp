# Technical Report

## Problem Statement

This application is a short quiz that tests the users knowledge of common abbreviations for different web development technologies. Each question has one correct answer and three incorrect answers, with a time limit of 15 seconds. Upon clicking one of the answers or the time running out, the correct solution will be revealed and the user will be prompted to move on to the next question. After the quiz is complete, the result is shown and the user is prompted to either exit the game or play again. 

## Functional Features

- Start quiz button
    - This button has an event listener that starts the process from `start -> info -> questions -> reuslt`
- Info screen (exit quiz / continue)
    - An element with static text that allows users to either start the quiz or opt out of the quiz.
- Timer
    - The timer is a functional element that controls both how longer to user has left to play the current question as well as the progress bar element. The timer is dropped to 0 when the user answers and is automatically reset when each new question is shown.
- Questions, options, answers
    - Questions, options, and answers are dynamically shown based on the current `questions` JavaScript object index. When the user selects and item or the timer runs out, the correct answer is shown and the score is updated.
- Next Question
    - This button resets the timer value and initializes the next set of question, options, and answer.
- Tracking score
    - Score is tracked after each question is answered. If it is incorrect or the timer runs out, no score is added. If the user selects the correct answer, one point is added.
- Result screen and buttons
    - The result screen is shown after all of the questions have been exhausted. In doing so, a certain message will display to the user depending on the amount of questions they got right. There is then an option to either exit the game or play again.

## Structure / Setup

- Although there are several files that make up this project, the entire website is displayed in a single page at a single web address. This is because there is only one HTML file, `index.html`. This file is responsible for providing the DOM structure and defining some static information. However, the real power of the application comes in the counterparts that are linked to this static HTML page.
    - Firstly, `css/style.css` is linked to the `index.html` with this line `<link rel="stylesheet" href="css/style.css">`. From there, styling is specifically applied to different elements through the use of `class` attribute on HTML elements.
    - These same `class` attributes are used to connect some of the JavaScript code that dynamically changes content and styling based on logic written alongside the file. To connect the two JavaScript files in our directory, we use `<script src="js/quizApp.js" defer></script>` and `<script src="js/questions.js" defer></script>`. The defer keyword here allows the full HTML code to be parsed before loading the JavaScript.
    - The directory structure of the project is as seen below. A more in depth breakdown of what each of these files does can be read about in the next section
    
    ```jsx
    .
    ├── js/
    │   ├── questions.js
    │   └── quizApp.js
    ├── css/
    │   └── style.css
    ├── index.html
    └── README.md
    ```
    

## Codebase explanation

- ******************Style.css****************** | styling for the HTML elements
    - The `style.css` file is the place where the web design is created. Connected to `index.html` by `<link rel="stylesheet" href="css/style.css">`, everything from the background color to intricate quiz box styling is handled here. Outside of common styling, there are nuanced additions that make the app feel smoother for a better UX. For example, adding a transition to the `quiz_box` element to make it pop as seen below  is a good touch. In addition, many of these styles are reused to make the code more compact. `quiz_box` is the container that is used for each question in the quiz and is hydrated by JavaScript. An addition that can be further made would be to make the quiz responsive to the viewers screen size using media queries.
    
    ```css
    transform: translate(-50%, -50%) scale(0.9);
    transition: all 0.3s ease;
    ```
    
    - It’s important to note that there is some CSS in this file that is directly used in the `index.html` file. However, these classes are still used and are rather dynamically toggled in the `quizApp.js` file.
    
- **Questions.js** | List of questions, options, and answers
    - Questions.js is a simple file that stores a single `questions` object in JSON format that stores the questions, options, and answers all in one place for readability and extensibility. With this being a separate file and imported into QuizApp.js, it is made simple to update and add questions without interfering with the functionality of the application.
- **********QuizApp.js**********
    - This is where the brain of the application is stored. Everything from dynamically displaying questions, keeping track of score, and even doing some reactive CSS is done in this single file connected to the `index.html` by the line `<script src="js/quizApp.js" defer></script>`. There is lots of code and logic cleanly separated by functions and explained by comments.
    - The `showQuestions()` function is the logic that handles dynamically displaying the questions in the format statically created in the CSS file. It accepts an `index` input that aligns with the index of the current question from the imported `questions` object. This function is called each time the `next` button is clicked with the index being incremented each time.
    - `time_line` is the CSS Class that is changed dynamically to make a progress bar that moves as time continues. With the `time_line.style.width` being incremented each time the function is called, which happens ever 29 milliseconds from when the timer is started.
    
    ```jsx
    function startTimerLine(time) {
      counterLine = setInterval(timer, 29);
      function timer() {
        time += 1; //upgrading time value with 1
        time_line.style.width = time + "px"; //increasing width of time_line with px by time value
        if (time > 549) {
          //if time value is greater than 549
          clearInterval(counterLine); //clear counterLine
        }
      }
    }
    ```
    
    - Finally, the score after completing the quiz is calculated and output to the user with the `showResult()` function. This function clears some HTML elements to make way for the final box and then gives you a personalized text output given the score that you receive on the quiz.
- **********************Index.html********************** | The landing page for the website
    - Index.html is where the entire application takes place. With index.html being the file served to the web, the other files are simply there for support and are added in using `<Link>` and `<Script>` tags.
    - There are some static and some dynamic components stored in this file. For example, the `info-list` is a static element in which the text will never change and is therefore written in the HTML file. Other components, such as `quiz_box` have dynamic elements that are hydrated by the JavaScript counterpart.
    - Some improvements in this file could be from consistently naming classes with either all underscores or dashes as well as removing the extra `</html>` tag at the end of the file.
