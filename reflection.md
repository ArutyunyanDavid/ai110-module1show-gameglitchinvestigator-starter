# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

---

## 1. What was broken when you started?

While playing the game, I found several bugs:

1. **Backwards hints.** When the secret number was 60 and I guessed 100, the app told me to "Go Higher!" even though 100 is already higher than 60. It should have said "Go Lower!" The high/low hint messages were swapped relative to the comparison.
2. **"Start New Game" didn't reset.** Clicking the "Start New Game" button did not fully restart the game. The score, guess history, and especially the game status carried over, so after a finished game the board stayed locked instead of starting fresh.
3. **"Show Hint" did nothing.** Pressing the "Show Hint" button never displayed a hint. It was wired up as a checkbox that only gated the high/low message, so it never produced or persisted an actual hint about the secret number.
4. **No warning for out-of-range guesses.** Entering a number outside the valid range (for example, above 100 or below the minimum) was processed like a normal guess. There was no clear warning, and the invalid guess still counted against my attempts.

**Bug Reproduction Log**

Document at least 3 bugs you found. Add rows as needed.

| Input | Expected Behavior | Actual Behavior | Console Output / Error |
|-------|-------------------|-----------------|------------------------|
| Secret = 60, guessed `100` | Hint says "Go Lower!" | Hint said "Go Higher!" | No console error; wrong hint text shown in UI |
| Clicked "Start New Game" after a finished game | Fresh game: new secret, attempts/score/history reset, status active, input usable | Game stayed locked; score and history carried over and status was not reset | No console error; game state not reset in UI |
| Clicked "Show Hint" button | A hint appears (e.g., even/odd or lower/upper half of range) | No hint appeared at all | No console error; nothing rendered |
| Guessed `150` (above 100) | Clear warning; guess not counted as an attempt | Treated as a normal guess; counted as an attempt with no warning | No console error; no warning shown |


---

## 2. How did you use AI as a teammate?

I used Claude in VS Code and ChatGPT as AI teammates while debugging the guessing game. One correct suggestion from Claude was to move the guess-checking logic into `logic_utils.py` and fix the high/low comparison so that guesses were compared as numbers. I verified this by running `python -m pytest tests/test_game_logic.py -v`, and all 4 tests passed.

One incomplete suggestion was when Claude first only swapped the message strings for "Go Higher" and "Go Lower." That fixed one visible case, but it did not fully solve the deeper problem because the secret number could sometimes be converted into a string, which could cause text-based comparison bugs. I verified the final fix by reviewing the code, running pytest again, and manually testing the Streamlit webpage.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

I decided a bug was really fixed only after checking both the code and the actual Streamlit webpage, not just one or the other. For automated testing I ran `python -m pytest tests/test_game_logic.py -v` and got `4 passed in 0.04s`, which checked winning guesses, too-high guesses, too-low guesses, and that the comparison was numeric instead of string-based. I also manually tested the webpage by guessing too high and too low, pressing New Game, pressing Show Hint, and entering out-of-range guesses to make sure each one behaved correctly. AI helped me understand why the numeric comparison test mattered, because comparing `"100"` and `"60"` as strings can give the wrong result even though the numbers look obvious. Running the tests after each fix gave me confidence that my changes worked and didn't break anything else.

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

The biggest thing I learned is that Streamlit reruns the entire script from top to bottom every time the user interacts with the page, like clicking a button or submitting a guess. That means a normal variable doesn't stick around — it gets created fresh and resets on every rerun, which is why the secret number kept changing. To fix that, you have to store anything you want to remember in `st.session_state`, which Streamlit keeps between reruns. In this game I used `st.session_state` to hold the secret number, attempts, score, status, history, and hint so they survive each rerun. The way I'd explain it to a friend: Streamlit re-reads the whole recipe every time you touch something, so anything you want it to remember has to go in a special box (`st.session_state`) instead of a regular variable.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

One habit I want to reuse is testing one bug at a time and writing down the expected behavior before I change any code, so I always know what "fixed" should look like. I also want to keep using pytest and Git commits as checkpoints, because running the tests after each fix and committing working code made it easy to see progress and go back if something broke. Next time I would review the AI's diffs more carefully before accepting changes, since I sometimes trusted edits too quickly. I would also ask more focused prompts earlier instead of broad ones, which would have saved time. Overall, this project made me trust AI-generated code less blindly, because even code that looks polished can have hidden logic and state bugs.
