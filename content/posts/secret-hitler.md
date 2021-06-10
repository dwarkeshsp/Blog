---
title: "Find fascists in Secret Hitler using a Bayesian network"
date: 2021-06-09
katex: true
markup: "mmark"
---

Here is a link to the [rules](https://www.secrethitler.com/assets/Secret_Hitler_Rules.pdf) of Secret Hitler.

## Our Approach

Based on whether the policy played at the end of a turn is liberal or fascist, we want to update the probability that the president during that turn is fascist and the probability that the chancellor during that turn is fascist.

The difficulty is that the probability of the president being fascist is in part determined by the probability that the chancellor is fascist and visa versa. For example, if a fascist policy is played, but we have a high prior on the chancellor being fascist, then this should be somewhat exculpatory for the president.

Therefore, a straightforward application of Bayes rule will not do. We will need to create a Bayesian network which reveals the relationship between the variables at play here. But before we do that, let us define the variables.

### Variables

$$P(P_f)$$: probability that the president is fascist

$$P(C_f)$$: probability that the chancellor is fascist

$$P(L)$$: probability that a liberal policy is played

$$P(F)$$: probability that a fascist policy is played

$$P(x, y)$$ where $$x + y = 3$$: probability that the president draws $$x$$ liberal cards from the pile and $$y$$ fascist cards

$$P(x, y)$$ where $$x + y = 2$$: probability that the chancellor recieves $$x$$ liberal cards from the president and $$y$$ fascist cards

Since we observe $$L$$ or $$F$$ at the end of every turn, our program must use this information to update the probabilities of the chancellor and president being fascist.

Specifically, **our program must be able to figure out $$P(P_f | L)$$, $$P(P_f | F)$$, $$P(C_f | L)$$, and $$P(C_f | F)$$**.

### The Network

![network](/static/bayesian-network.png)

As you can see, the president has some probability of drawing 1 of 4 different combinations of 3 cards: 3 liberals 0 fascists, 2 liberals 1 fascist, and so on. Which 2 cards he hands down to the chancellor depend on what he drew and whether he is a fascist. That is unless he draws 3 of the same kind, in which case it doesn't matter if he is a fascist or not.

The chancellor recieves 2 cards, which could be both liberal, both fascist, or 1 of each. He only gets to make a decision when he recieves 1 of each, and again that decision will depend on whether he is a fascist.

### Applying Bayes Rule

Bayes rule tells us that:

$$
P(A | B)  = \frac{P(B | A) P (A)}{P(B)}
$$

Suppose we just witnessed a fascist policy get played. How would we update the proabilities that the president and chancellor are fascist? Let's start with the president and use Bayes rule:

$$
P(P_f | F) = \frac{P(F | P_f) P (P_f)}{P(F)}
$$

$$P(P_f)$$ is simply our prior that the president is fascist. This value starts off equal for all players and changes as more turns get recorded.

$$P(F)$$ is the probability that a fascist card would get plaed.
