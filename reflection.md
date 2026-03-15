# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

The first time I ran the game, it looked simple and functional, but the behavior immediately showed that something was wrong. When I selected Easy mode, the range displayed as 1 to 20, but the “Guess a number” prompt incorrectly showed 1 to 100, which didn’t match the difficulty level. I also noticed that the Attempts Left counter showed 4, while the Developer Debug Info showed 2 attempts, which was contradictory. This same inconsistent behavior appeared in Normal and Hard modes as well. On top of that, the game sometimes ended prematurely, even though I hadn’t guessed the correct number.

---

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).
For Phase 2, I used Claude in VS Code as my main AI teammate to investigate, 
fix, and verify the bugs.

**Correct AI suggestion:**
When I asked Claude to fix the type-mangling bug in app.py, it correctly identified that the if/else block was converting the secret to a string on even attempts and removed it entirely. It also cleaned up the TypeError fallback in logic_utils.py at the same time. I verified this by running pytest and confirming all tests passed.

**Incorrect/misleading AI suggestion:**
When fixing the New Game button, Claude initially only changed random.randint(1, 100) to random.randin (low, high) but missed resetting st.session_state.status, st.session_state.score, and st.session_state.history. I caught this by live testing the game in the browser and noticing the secret was still out of range. I had to prompt Claude again with a more specific instruction to reset all session state.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

To verify each bug was fixed I used two methods: running pytest after each fix and manually testing the live game in the browser with streamlit run app.py.

For Bug #2 (attempts counter), pytest confirmed the fix immediately after Claude changed attempts from 1 to 0. For Bug #3 (type-mangling), I verified by guessing numbers higher and lower than the secret and confirming the hints said "Go LOWER!" and "Go HIGHER!" correctly.

The most valuable test was live browser testing, which caught a bonus bug — the New Game button was generating secrets outside the difficulty range. The pytest alone would not have caught this since it only tests logic_utils.py functions. Claude helped generate 3 new pytest tests targeting the specific 
bugs we fixed, and all 6 tests (3 original + 3 new) passed after the fixes.

---

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- What change did you make that finally gave the game a stable secret number?

I learned that the secret number kept changing because Streamlit reruns the entire script every time the user interacts with the app. Since the original code generated the secret number at the top of the script, each rerun created a brand‑new number, which made the game feel unpredictable and impossible to win consistently. The easiest way I can explain Streamlit reruns to a friend is: every click or input basically “refreshes” the app unless you store values in st.session_state, which acts like a little memory box that survives those refreshes. The fix that finally stabilized the game was moving the secret number into st.session_state and only generating it once when the game starts. After that change, the number stayed consistent throughout the round, and the game finally behaved the way it was supposed to.

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

One habit I want to reuse in future projects is running small tests early and often, instead of waiting until the end to see if everything works. It helped me catch issues faster and made debugging feel way more manageable. Something I would do differently next time is be more specific with my AI prompts, I realized that vague questions lead to vague answers, but targeted prompts with file context (#file:app.py, #file:logic_utils.py) gave me much better results. This project also changed the way I think about AI‑generated code, because I saw firsthand that AI can speed things up, but it still needs a human to verify, correct, and guide it. In the end, it felt more like a collaboration than a shortcut.


