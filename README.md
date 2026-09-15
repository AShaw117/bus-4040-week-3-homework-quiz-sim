# bus-4040-week-3-homework-quiz-sim

**Wizard Tower** is a study game for BUS 4040, Week 3, Part 2. It quizzes you on the
course material from weeks 1 through 3 while zombies shuffle toward your tower.

### ▶ [Play it here](https://ashaw117.github.io/bus-4040-week-3-homework-quiz-sim/)

Or download `index.html` and double click it. That is the whole install. There is no
server, no build step, no internet connection, and no outside libraries. The HTML, the
CSS, the JavaScript, the pixel art, the sound, and all twenty questions live in that one
file. The hosted link above is the same single file served by GitHub Pages.

---

## What type of quiz I created

A gamified, timed quiz with an arcade layer on top. The quiz half is ordinary: twenty
questions, a mix of multiple choice and true or false, one at a time, reshuffled on every
run, with an immediate explanation and the week to go back to. The game half is what makes
you answer instead of stalling.

**The scene.** You are a wizard on a tower rendered in 8 bit pixel art on a canvas. Zombies
spawn on the right and walk left. If one touches the tower, the run ends.

**The loop.**

| Event | What happens |
|---|---|
| Correct answer | The fireball destroys the closest zombie and that question is done. +1 point, plus up to +5 for answering fast |
| Wrong answer | The spell misfires and kills nothing. Every zombie hops 7 seconds closer, the combo resets, and the question goes to the back of the line |
| 3 correct in a row | Combo: the blast takes 2 zombies |
| 6 correct in a row | Combo: the blast takes 3 |
| A zombie reaches the tower | Run over |
| All 20 cleared | You win, the banner reads TOWER HELD |

A wrong answer is pure punishment. The bolt still leaves the staff, but it droops,
guts out in a puff of smoke, and the horde jumps forward while you read why you were
wrong. Killing anything requires getting the question right.

**You have to clear all twenty to win.** A question you miss is not gone, it goes to
the back of the queue and comes round again, flagged as a second chance, until you get
it right. The run has no fixed length: it ends when you clear the board or when a zombie
touches the tower. The counter in the corner reads cleared out of twenty, not question
number, because a bad run can take thirty attempts to clear twenty questions.

The clock runs while you read the question and stops while you read the explanation. The
pressure is on recall, not on reading the teaching part.

**Waves.** Every 28 seconds of live play the wave advances. Zombies walk faster and spawn
closer together, down to a floor so it never becomes impossible.

**The end screen.** Score, questions cleared, wrong answers, accuracy, best streak, zombies
destroyed, wave reached, and time survived. Below that is a **Concepts to work on** list that
groups trouble by topic and names the week to review, saying how many times each topic cost
you and whether anything was never cleared. Then comes a review of all twenty questions,
sorted worst first: never cleared, then missed before clearing, then right first time, then
never came up. A **Retry missed only** button rebuilds the deck from anything you fumbled or
never reached.

**Controls.** Click an answer, or press `1` `2` `3` `4`. True or false takes `T` or `F`.
`Enter` moves on after the explanation. Sound can be switched off on the title screen.

---

## Where the questions come from

Every question and every explanation is drawn from the nine files in the Week 1 to 3 folder:
the three weekly narratives, the three homework handouts, the GitHub connection guide, the
Cline and MindRouter setup guide, and the week 2 survey. Nothing was added from outside
that material, and anything ambiguous in the source was skipped rather than guessed at.

| Week | Questions | Topics covered |
|---|---|---|
| 1 | 5 | Course structure, why a business school teaches AI, why version control matters, GitHub accounts |
| 2 | 7 | History of Git, how Git works, creating a repo, git commands, keeping secrets out of AI prompts, push errors |
| 3 | 8 | How models work, model families, tokens and cost, harnesses versus models, skills, open weight versus closed, MindRouter setup |

Fifteen are multiple choice and five are true or false.

---

## What worked

The prompt I gave it was pretty airtight - everything worked right away for the most part, the game ran fine, it asked the right questions, it progressed correctly, etc. I could have turned it in right away but I still wanted to make a few tweaks.

## What did not work, and the adjustment I had to make

The few things I didn't like were a few of the game mechanics and how it flowed, it was very easy to win and felt pretty slow. I just upped the zombie frequency and made them advance on you for wrong answers. Everything else worked fine, it was just that little tweak.

## How I could apply this elsewhere

The reusable idea is not the zombies. It is the shape of the thing: **source material in,
structured practice out, with the score pointing back at the source.**

- **Any other course.** Swap the twenty question objects and the game is a study tool for a
  different subject. Each question carries its own week, topic, and explanation, so the
  review screen rebuilds itself automatically.
- **Gamifying Other Things** I can use this current build for any class I want, but I can also now
  easily try to make other less fun things more fun. I can't think of anything off the top of my
  head but it's good to have in my back pocket.

---

## Files

- `index.html` - the entire game. Double click to play.
- `README.md` - this file.
