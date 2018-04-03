---
layout: post
title: Predicting the traitor for 'De Mol' with a bootstrap model
---

One of my guilty pleasures is the Belgian reality TV program 'De Mol' (translated as The Mole).
It is very successful, both in Belgium and outside, as it has been exported to a large number of countries.
Your own country may even have its own variant!
It is so well known in Belgium that I have to avoid the Belgian news the next day to avoid spoilers if I haven't seen it yet.
Here I am going to try to solve its mystery by using some data munching techniques!

## De Mol program scope

The idea behind the program is quite simple. 10 strangers are shipped off to an exotic location and have to do challenging trials to earn money.
The twist is that one of these stranger is a traitor (the titular Mol), working against the group and deliberately failing at their tasks.
It is up to the other contestants to guess who the traitor is. 
Every episode there is a moment where they have to formalize their guess in a survey by answering questions about the traitor. 
The one that gets the most questions wrong has to leave.
In the final episode, the contestant who has the most questions right (or the quickest time) gets all of the money that the group earned.

The exciting element behind this show is to try to guess the traitor before the end. 
The day after an episode, everyone will be sharing their theories and guesses with their family or coworkers.
As each episode someone leaves, the options narrow and the hype will rise.

If you are familiar with board games that have a traitor mechanic (Werewolves, Saboteur, etc), you should be able to understand the principle.

## What I aim to do

For the past few years, I have been experimenting with tackling predicting the traitor based on some tricks I know from my day job (as a biodata scientist).
I never got around to formalising the model, but would just work it out on paper. From what I could see, it seemed to work well. But that is mostly retrospective, after the program is done and the traitor had been revealed.
This year, I'm going to try to make my predictions in advance with the formalised model. And to have some proof after the fact, I am going to keep these blogpost online.
I don't expect 100% accuracy from my model. In fact I put it on even odds if it will make the right prediction before the final episode.
But I am very curious how it will adapt and change as the season progress and it gets more and more data.
So I'll try to post each week what it is predicting and I'll discuss some of the things where it may go horribly wrong.

## How the model works

There are a few commonly accepted ways of figuring out the traitor in De Mol.
* You can observe the body language of the different candidates and the reactions. But this requires far more people knowledge than I have.
* You can keep track of who is earning the most money for the group. But I think there are just some people that are very unlucky.
* You can scour the episodes for small clues that the traitor or the production team is leaving. I saw the ones of previous seasons and would never be able to figure those out.

The way my model works is by estimating who suspects who of being the traitor and then observing who has to leave each week.
If someone has to leave, that means that they are most wrong about who the traitor is. This means we simply have a game of elimination and can pinpoint the traitor if enough people have left. (I may later do a blog post about how much is enough people in this case.)
So all we have to do is know whom suspects who. The problem is that this is a closely kept secret between the contestants and we don't know what they fill into their surveys.
However each episode they have small interviews with the contestants and ask them about their suspicions and theories.
This is only a few data samples, but we can try to extrapolate from that.
So each episode I hand annotate these little interviews with whom is pointing fingers at who. (I don't want to right a complex machine learning parser to do this automatically.)
Since this is only a small selection of insight into the current thoughts of each contestant, I bootstrap these statements to build a suspicion matrix.
The matrix will then contain for all remaining contestants how many times another contestant has pointed the finger at them in these interviews.
I will keep track of which contestants have dropped out and which remain. Then the algorithm will try every possible traitor to check who has garnered the most suspicion with those that remain and the least with those that have dropped out.

## The assumptions that make it work

The model is based on some assumptions that I've noted in previous years:
* Contestants will hedge their bets. They will often suspect multiple people and fill in their survey as such. Only in the last few episodes will they narrow down to one person.
* The editors almost never show an interview for a person who is going to leave soon. We need to rely on previous episodes to establish their suspicions.
* People hardly ever change their minds about who the traitor is, and typically become more sure as the series progress. They are still human and suffer from confirmation bias.
* The interviews are truthful as they are talking to the camera without any of the other contestants overhearing. This is not the case for the traitor.
* A good check is to see if suspected traitor (whos suspicions are disregarded when they are considered as such) is making odd statements. This usually doesn't work as they usually throw suspicion on those that suspect them, but can be an interesting validation step.

## The suspicion matrix after episode 2

I did not have enough data after a single episode, but some trends do emerge after the second one.

|*Suspect*||Bahador  |Lloyd  |Jeffrey  |Steve  |Pieter  |Joke  |Channy  |Katrien  |Pascale  |
|:--------||---------|-------|---------|-------|--------|------|--------|---------|---------|
|Pieter  ||6|14|21|11|0|23|6|30|8|
|Pascale  ||44|24|4|10|9|4|5|5|0|
|Lloyd  ||12|0|9|5|21|5|5|11|19|
|Steve  ||6|2|20|0|25|5|12|5|8|
|Joke  ||6|14|9|5|4|0|46|24|4|
|Jeffrey  ||6|6|0|5|4|28|6|11|33|
|Bahador  ||0|28|19|25|19|5|12|5|4|
|Katrien  ||6|6|9|5|9|22|5|0|18|

With the rows as the potential suspect, and the columns with each contestant. Thus it is COLUMN suspects ROW.
The values are normalised at the column level to avoid weighting a single contestant higher than another.
It is a good sign that none of the contestants suspect themselves. This is a good sanity check for the model.
Furthermore each contestant seems to primarily suspect between one and three others, which seems reasonable.

## De Mol probability after episode 2

These are the first results:

|*Traitor*||*Probability*|
|:-------||:----------|
|Pieter||32%|
|Pascale||22%|
|Bahador||20%|
|Jeffrey||10%|
|Lloyd||7%|
|Katrien||4%|
|Steve||2%|
|Joke||1%|

Note that at this point it is simply putting forward those that are most suspected by the group. 
The top three (Pieter, Pascal and Bahador) are among those that have gotten the most suspicion from the other candidates.
But that doesn't mean that they are right. As contestants will start to drop out, this will most definitely change.
What is a good sign, is that Joke is at 1%. The suspicion matrix indicates that she was the prime suspect of Channy, who dropped out in episode 2.
Thus at the moment the model thinks it is highly unlikely that Joke is the traitor. However this is only true if it is right about the Channy suspicion, so it may have to correct itself later.