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
| | | | |
| | | | |
| | | | |

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

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.
