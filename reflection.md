# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

- What did the game look like the first time you ran it?
- List at least two concrete bugs you noticed at the start  
  (for example: "the secret number kept changing" or "the hints were backwards").

---
When I first ran the game, I didn't see the debug info so I was guessing blindly. I could tell it was a simple guess the number game. The hints were automatically on so I immediately had the direction of going higher or lower. Then I luckily became in between 47 and 48, however, nothing was correct even though it should have been according to the lower or higher information the app was giving. The answer was 8. So that was the first bug. The second bug came along with that because the game ended early for me. It said I had one attempt left, but I was forbidden from putting in any other inputs. Then the third bug I noticed had nothing to do with the actual guessing, it was the fact that the "new game" button does not work. When I press it, it still says "start a new game to play again" and I am unable to put in any input.

## 2. How did you use AI as a teammate?

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct (including what the AI suggested and how you verified the result).
- Give one example of an AI suggestion that was incorrect or misleading (including what the AI suggested and how you verified the result).

---

I used Claude AI to analyze the codebase and help explain the logic behind several bugs in the Streamlit game. I used prompts that referenced the codebase and asked the AI to trace the logic step-by-step so I could understand why the game behavior did not match what the UI was showing.

One example of a correct AI suggestion was when the AI identified that the secret number was sometimes being passed as a string instead of an integer. The code alternated the type of the secret variable depending on the attempt number, which caused the comparison logic in check_guess() to fall into a TypeError branch and compare numbers as strings instead of integers. This meant guesses like "47" were compared to "8" lexicographically, producing incorrect hints such as telling the user to go lower when the guess was actually much higher than the secret number. I verified this by running the Streamlit app and repeatedly testing guesses that were clearly above or below the secret. After removing the logic that converted the secret number to a string and keeping it as an integer, the hints behaved correctly and the correct number could finally be guessed.

One example of an AI suggestion that was incorrect or incomplete was when the AI fixed the type issue with the secret number but did not notice that the UI hint messages themselves were reversed. Even after fixing the data type issue, the hints displayed to the user were still misleading because the messages did not match the comparison between the guess and the secret. I verified this by running the app and intentionally guessing values higher and lower than the secret number. I then corrected the conditional logic so that the messages properly matched the comparison between guess and secret.


## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran (manual or using pytest)  
  and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

I decided a bug was fixed only after the behavior of the game matched the intended logic during multiple runs of the application. After each change, I restarted the Streamlit app and manually played several rounds to make sure the hints, attempt counter, and new game button behaved correctly.

One manual test I ran was repeatedly guessing numbers until the attempt limit was reached. I watched how the attempt counter changed after each guess and confirmed that the game only ended after the correct number of attempts had been used. Originally the counter did not decrease correctly and the game ended early. After fixing the attempt initialization and the UI update order, the counter correctly updated after each guess and allowed the full number of attempts.

I also tested the New Game button by finishing a round of the game and then clicking the button to start a new game. Originally the game still displayed the message telling the player to start a new game but did not allow any input. After adding logic to reset the status variable back to "playing", the button properly reset the game and allowed the player to start guessing again.

Claude helped me understand how the bugs occurred by walking through the code logic step-by-step and explaining how Streamlit reruns the script on every interaction. This explanation helped me understand why some values appeared incorrect in the UI and what behavior I should expect when testing fixes.

## 4. What did you learn about Streamlit and state?

- In your own words, explain why the secret number kept changing in the original app.
- How would you explain Streamlit "reruns" and session state to a friend who has never used Streamlit?
- What change did you make that finally gave the game a stable secret number?

---

The secret number appeared inconsistent in the original app because the code sometimes converted it into a string instead of keeping it as an integer. This caused incorrect comparisons between the guess and the secret number and made the hints unreliable, which made the game seem unpredictable.

Streamlit reruns the entire script from top to bottom every time the user interacts with the interface, such as clicking a button or submitting a guess. Because of this behavior, any variable that is not stored in st.session_state will reset every time the app reruns. Session state works like memory for the app, allowing values such as the secret number, attempt counter, and game status to persist across reruns.

The change that gave the game a stable secret number was removing the logic that converted the secret number into a string and always passing it to the guessing function as an integer. This ensured that comparisons between the guess and the secret number were always numeric and consistent.

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects?
  - This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI generated code.

One habit I want to reuse in future projects is testing changes immediately after making small edits to the code. Running the app after each modification helped me quickly confirm whether a bug was actually fixed or whether another issue appeared.

One thing I would do differently next time when working with AI is reviewing the suggested fixes more carefully before assuming they are complete. Some suggestions solved part of the problem but still required additional debugging to fully correct the behavior of the program.

This project changed the way I think about AI-generated code because I realized that AI is most helpful for explaining logic and guiding debugging rather than providing perfect fixes on the first attempt. The developer still needs to verify the behavior of the program through testing and careful review of the code.
