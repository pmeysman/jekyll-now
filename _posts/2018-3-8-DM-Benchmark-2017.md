---
layout: post
title: Benchmarking the model on the 2017 season of 'De Mol'
---

A popular request after last week's post on trying to predict the 'mole' (the titular traitor in the Belgian tv show 'De Mol'), was to check how the model would perform on previous seasons where we already know the outcome.
As I only have limited time to spend on this side project every week, I decided to only try it on the season of 2017 (spoilers for last year, seriously).
This time there were 11 possible suspects, of which one actually joined later in the season than the others.
The final episode, number 8, saw the number of contestants whittled down to three, Annelies, Davy and Eline.
The traitor turned out to be Eline. Using data from episodes 1 to 7, my model gave her the highest chance at 53%.
In fact, starting after episode 4, she was its prime suspect.

![Mol chance for the 2017 season]({{ site.baseurl }}/images/mol-2017.001.jpeg "Predictions for the 2017 by episode, until the penultimate episode.")

So great, it works!
Does this mean we can expect to know the true traitor after next week's episode? 

Well, probably not. The model got very confused around episode 6, and actually suggested the wrong traitor, Annelies.

## Where it went wrong

For those who read [last weeks post](https://pmeysman.github.io/DM-First-Iteration/), know that the model is based on building suspicion graphs. In other words, it will try to guess who suspects who, and will use that info to determine the traitor.
After episode six in the previous season, the suspicion matrix looked something like this between the four remaining candidates:

![Suspicion matrix at episode 6, season 2017]({{ site.baseurl }}/images/mol-2017.002.jpeg "Suspicion matrix at episode 6, season 2017. From left up to down right; Robin, Annelies, Davy and Eline (The Mole).")

It's clear why the model might believe that Annelies (top right) might be the traitor. It is also very interesting that the final three candidates all pointed to each other as suspects.
This is a very common strategy. Often candidates will deliberately (or sometimes accidentally) make themselves suspect to misdirect others. So the finalists tend to all be very suspicious.
The mole of course will often start pointing at those that suspect him or her. Interestingly, this triangle structure between the three finalist was in place starting from episode 4.
In fact, if you look at the way the probabilities move above, after episode 4, the top three suspects were the three finalists (while there were seven total contestants).

It was only because Robin dropped out in episode 7, that the model decided to down weight Annelies again.
There is no guarantee that we will be so lucky this year, but we will see.

## Modelling bias

One important thing to note is that this data for the 2017 season was collected after it was done. I of course, already knew the outcome. So it might be that when coding the data, I inadvertently tuned it so that the outcome is Eline.
That is why this year's test is much more critical. The model is unbiased (in so much as I have little idea myself) to the outcome.
But of course, there is no guarantee that it will work especially given the last section.
Honestly I'll be happy if the actual traitor doesn't have a 0% chance at the end of the season.

## An interesting observation

Based on the 2017 season, something interesting pops up.
Out of 7 episodes (and 8 contestants sent home), only three contestants were interviewed about their suspect in the episode that they were sent home in.
While in all the episodes, the majority of candidates were interviewed in such a fashion.
This means that if you want to guess who has to go home in the episode you're watching, you should pay attention to those who don't get a private interview moment.





