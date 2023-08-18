# Monty Hall with Bayes

Leonardo C.  

12 April 2023

I finally got an understanding of the solution of the Monty Hall problem after watching [this video from Lazy Programmer](https://www.youtube.com/watch?v=5NZa7enz_-c). Another explanation I found to be really clear is [this Medium article from GreekDataGuy](https://towardsdatascience.com/solving-the-monty-hall-problem-with-bayes-theorem-893289953e16).

I had tried a few times to solve the problem either by myself, or by reading other people’s solutions. I noticed that one of the reasons I was previously getting stuck with was by switching continously between my scribbles on the paper and the mental image of ****************************************where actually the car and the goats are****************************************. However in a real Monty Hall situation you do not get to see where the car is, and this is an important point that in my opinion should be kept as it is also when trying to solve the problem. After all, the rules of probability were devised exactly to reason in situations of uncertainty, but if we get to know in advance where the car actually is, the core uncertainty in the problem dissolves.

So what worked for me - and differently from all the other expositions of the Monty Hall problem I have seen so far - was ***not*** to start with “let’s assume that the car is behind door *n*”. Instead, let’s not assume anything about where the car actually is, and let’s focus only on the information that are actually available to us:

- the door I choose, for instance door 1
- the door that Monty Hall opens ************************after I have chosen door 1************************. For instance door 2.

Indeed Monty **************************does not open one of the two remaining doors at random**************************. His choice of the door to be opened is conditioned - and in 2 out of 3 cases determined - by which door I choose, and by which door has the car behind it. In other words, he can only choose one door with a goat behind, among the two that I have not chosen. 

This observation - the fact that the choice of which door Monty Hall opens is conditioned on which door I chose - is so natural and given for granted that I did not notice at first how crucial it is for calculating the probability of winning the car. 

So, we know which door I initially chose, and which door Monty opens given my choice. The only ********information which is still uncertain is ****************where the car is****************, and we should focus our probability calculation on this, given what we know.

Specifically, we should focus our attention on calculating the probability that Monty Hall would choose to open the door he did open - door 2 in this case - in either one of the three cases in which the car is behind door 1, 2 or 3.

 

| Probability that Hall opens door 2 given that the car is behind door 1,2,3 and I chose door 1 | Motivation |
| --- | --- |
| $p(H = 2 | C = 1) = 0.5$ | because goats are behind both doors 2 and 3 |
| $p(H = 2 | C = 2) = 0$ | because Monty Hall cannot open the door with the car  |
| $p(H = 2 | C = 3) = 1$ | because I chose door 1, and the car is behind door 2, so he can only open door 3 |

Once we have calculated these probabilities, we can get to the most important question: what is the probability that I will win the car in either of the two options: keep the initial choice of door 1, or switch to door 3 - as Monty Hall opened door 2.

| $p(C=1|H=2)$ | probability of winning the car if I keep my original choice of door 1 |
| --- | --- |
| $p(C=3 | H = 2)$ | probability of winning the car if I switch to door 3 |

Note that the conditional probabilities I would like to calculate here are the opposite of those I previously estimated. 

Therefore to estimate $p(C|H)$ I just need $p(H|C)$ and Bayes’ rule:

$$
p(C|H) = \frac{p(H|C) \cdot p(C)}{p(H)}
$$

We know already our ***Prior***, that is the probability of choosing the door with the car before knowing anything else: $p(C) = 1/3$.

We also know the denominator of the equation, that is $p(H)$ - or ***************************Marginal probability** of H* - since this is the weighted sum of all the initially calculated $p(H|C) = (1/2 + 0 + 1)/3 = 1/2$.

Finally, we know - again from the initially calculated conditional probabilities - what is the ******************************Likelihood********************* of both 

- $p(H=2 | C=1) = p(Keep Door 1) = 0.5$
- $p(H=2 | C=3) = p(SwitchToDoor3) = 1$

Now we just have to plug in our numbers to have the answers:

If I keep my original choice of door 1, the probability of having picked the door with the car is 1/3.

$$
p(C=1|H=2) = \frac{p(H=2|C=1) \cdot p(C=1)}{p(H=2)} = \frac{1/2 \cdot 1/3}{1/2} = 1/3
$$

On the other hand, if I switch to door 3, my probability of winning the car increases to 2/3.

$$
p(C=3|H=2) = \frac{p(H=2|C=3) \cdot p(C=3)}{p(H=2)} = \frac{1 \cdot 1/3}{1/2} = 2/3
$$

# What is difficult about the Monty Hall problem

All problems involving probability are quite difficult for humans. Or as it is usually put, very *counterintuitive*. It’s interesting that this is the case, since we constantly have to deal with situations loaded with uncertainty.  

The following was the strategy that led me to understand how to deal with this problem

## Focus on the right question

When we hear all the details of the problem, we realize there are ****many****, and we immediately try to form a mental movie with images - showing us where the car actually is - and sequences of event - first the guest’s choice of the door, then the choice of Monty Hall about which door to open. With this mental construct in mind, we start to work on the problem.

The main question is “what is the probability of me getting the car if I keep my choice or if I switch”. This is correct, but not complete. Probability is *******always******* conditional to something happening. When we formulate this question, we forget that the probability of getting the car is conditional to *both:*

1. the door that we just choose
2. the door behind which there is a car
3. the door that Monty Hall will open

 precisely in ****this**** order. In fact, the decision of Monty Hall to open a certain door is conditional to both which door I chose and which door hides the car

# The simplest possible explanation of the Monty Hall problem

Once you choose one of the three doors, your excitement for the car makes you focus on your 33% chance of having chosen the door with the car - because we are humans and we prefer to focus on favourable chances, simply neglecting the unfavourable ones and acting as if they wouldn’t exist :O)

Instead, you should keep cool and focus on the much less appealing fact that whichever door you chose, you have a 66% chance that there is a goat behind it (!)

You might feel bummed by this, but you would be mistaken: if you chose a door with a goat behind, it’s your lucky day! 

As a matter of fact, you should *wish* you chose one of the two doors with a goat behind, since *that would force Monty Hall to open the only other door with a goat behind* - because he cannot open neither the door with the car nor the door you chose. 

This will automatically increase your chances of winning the car from the original 33% up to 66% - provided that you switch the door. Let me explain why:

*Whichever* door you choose, 

- two third of the times you will choose a door with a goat
- then, two third of the times Monty Hall will be forced to open the only other door with a goat, effectively indicating you what is the door with the car - the one he didn’t open
- hence if you always switch, two third of the times you will switch to the door which has a car behind

Hopefully this explanation is simple enough to convince you that if you keep the original choice, you maintain your original 1 out of 3 chances of having chosen the right door, while if you switch, your chances of getting the car increase to 2 out of 3.

If you are still not convinced, you can try by yourself playing the Monty Hall game online on [this beautiful app by mathwarehouse.com](https://www.mathwarehouse.com/monty-hall-simulation-online/).