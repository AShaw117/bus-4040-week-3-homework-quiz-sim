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

**The one file rule was easy to hold.** Canvas drawing, the Web Audio API for the beeps,
and plain CSS covered everything. Checking the browser network tab at the end showed a
single request for the HTML file and nothing else, which is the proof that it really does
run offline.

**Pausing the clock during the explanation.** This was the change that made it a study tool
instead of a reflex test. Early on the timer ran through the explanation, so the sensible
move was to skip reading it. Freezing the world while the explanation is on screen means
the game rewards knowing the answer, not skimming past the teaching.

**Tagging every question with a topic and a week.** That one field made the whole end screen
work. Grouping misses by topic turns a score into a study list.

---

## What did not work, and the adjustment I had to make

**The first version was unwinnable, and not in an obvious way.**

My starting numbers were the ones in the original design: a zombie takes 15 seconds to reach
the tower and a new one spawns every 5 seconds. Playing it, a zombie touched the tower at
17 seconds, before question one had even been answered.

Rather than guess at new numbers, I wrote a small simulation of the run and tested five
player profiles across hundreds of trials each. That exposed the real problem, which was not
the one I assumed.

I assumed the issue was backlog, too many zombies to clear. It was not. The queue at the
moment of death was only about three. The actual constraint is **arrival cadence**: once the
field fills, zombies reach the tower on a fixed schedule set by the spawn interval. If
zombies arrive every 5 seconds and you only get to shoot once per question, then taking 11
seconds to read a question means one gets through no matter how many the fireball destroys.
Killing more per shot clears a backlog; it does nothing about the cadence.

That reframing pointed at the fix. The spawn interval has to be in the same range as the time
a person actually spends reading a question.

**Then the rules changed and it had to be rebuilt.** A wrong answer originally still destroyed
one zombie. That felt wrong: being wrong should not be rewarded. So a miscast now kills nothing
at all, and a correct answer kills exactly one unless a combo is running.

That single change broke the balance again, and the simulation found a second failure mode I had
not expected. With spawns made rare enough to keep up, the field sat **empty** most of the time,
so shots were wasted on nothing. Then a lone zombie would appear, and because it only lived long
enough to face about two answers, one wrong guess at the wrong moment was fatal. The game was
being decided by a coin flip rather than by knowledge.

The real lever turned out to be the length of the walk, not the spawn rate. Each zombie has to
survive long enough to face three or four of your answers, so that a single miss is a setback
rather than a death. Zombies now shamble slowly across a long field and spawn close together,
which also keeps four or five on screen and stops the shot-wasting. The result:

**Then I tightened the screws.** Once it played properly I made the zombies quicker, cut the
respawn gap, and steepened the wave ramp, and I changed the win condition so a missed question
comes back instead of the run simply ending after twenty.

The simulation made an unexpected split clear. The requeue rule is almost difficulty neutral on
its own, because a repeat is answered fast and confidently right after you have read the
explanation. It actually raises the number of questions a middling player clears. Nearly all
of the added difficulty comes from the speed changes:

| Player | Win rate, before | Win rate, after |
|---|---|---|
| 7 seconds per question, 95% correct | 100% | 100% |
| 9 seconds, 85% correct | 98% | 85% |
| 11 seconds, 75% correct | 60% | 21% |
| 13 seconds, 60% correct | 4% | 1% |

That is the curve I wanted. Knowing the material and answering promptly wins. Guessing loses,
and losing hands you a list of what to review.

One more bug the tests caught: at a 3 zombie combo the blast only ever killed 2. After the
fireball destroyed its target it kept flying flat and sailed over anything standing in a lower
lane. It now re-aims at the next closest zombie after each kill.

**A smaller thing that did not work:** the built in PDF reader could not open a scanned file
earlier in the week, so the source had to be pulled out with a small Python library instead.
The course files for this assignment were all markdown, so that problem never came up here.

All the tuning numbers sit in one `CFG` block near the top of the JavaScript, with a comment
on each, so the difficulty can be changed without hunting through the code.

---

## How I could apply this elsewhere

The reusable idea is not the zombies. It is the shape of the thing: **source material in,
structured practice out, with the score pointing back at the source.**

- **Any other course.** Swap the twenty question objects and the game is a study tool for a
  different subject. Each question carries its own week, topic, and explanation, so the
  review screen rebuilds itself automatically.
- **Training at work.** The same structure fits onboarding, compliance refreshers, or
  product knowledge for a sales team. The end screen already tells a manager which topics a
  group is weakest on.
- **Checking my own understanding.** Turning material into questions forces you to decide
  what actually matters in it. Writing the quiz taught me the content better than rereading
  the files would have.
- **The general lesson.** When something feels wrong, measure it rather than nudging numbers
  and hoping. The simulation took a few minutes and found a cause I had guessed wrong about.

---

## Files

- `index.html` - the entire game. Double click to play.
- `README.md` - this file.
