---
author: "Srini"
title: "Bayes Rule"
date: "2020-01-22"
description: "Bayes Rule - How to make sense of it"
type: "post"
draft: false
banner: "bayes_rule.jpg"
summary: "Bayes rule is very famous. How do you make sense of it?"
---
I have read/heard different explanations for what Bayes rule means but never really grasped the simple intuitive meaning behind it until today. So I thought of writing about it.

The Bayes rule is written as  
$$P(A|B) = \frac{P(B|A) P(A)}{P(B)}$$  
Where $A$ and $B$ are events.  

Let us start from an application where Bayes rule is used. Let us say we have a sensor that measures some physical quantity. To keep things simple let us consider a discrete example. Let the sensor measure if a light is on or off.

For the sensor to be useful, we would like to draw a conclusion where the light is on or off based on what the sensor reads. To formulate this as a probability,
$P(X=on, Z)$

This is the probability that the light is on given the sensor measurement. Here $X$ and $Z$ are random variables where $X$ represents the state of the system and $Z$ represents the measurements we make with the sensor. In this simple case $X$ and $Z$ are single random variables. In a real scenario there could be multiple of those.

Now if we can calculate at this probably value easily then we do not need Bayes rule. But there is no easy way to arrive at this due to the fact that it is not a **causal relationship**. That is we are trying to diagnose the state from an observation/measurement. This is called **diagnostic relationship**. The probability of the causal reasoning is written as:  
$$P(Z|X=on)$$

That is, the probability of the measurement given that we know the state. So can we measure this quantity? Of course yes. The simplest method is to turn on the light several hundred times and record the sensor observations. From those observations we can calculate the frequencies to arrive at the causal reasoning.  
$$ P(Z=on|X=on) = \frac{number\ of\ times\ the\ sensor\ reported\ the\ light\ was\ on}{total\ number\ of\ times\ the\ light\ was\ on}$$
$$P(Z=off|x=on) = 1 - P(Z=on|X=on)$$

Similarly we can derive $P(Z=off|X=off)$ and $P(Z=on|X=off)$  

Now the interesting part is, Bayes rules explains the relationship between the causal reasoning and diagnostic reasoning.  
$$ P(X=on|Z) = \frac{P(Z|X=on)P(X=on)}{P(Z)}$$

This is the most important insight (at least in my opinion) one should get when learning Bayes rule. That is, it explains the relationship between the causal reasoning (which is easy to measure) and the diagnostic reasoning (which is needed by the system). So generally the term on the right tries to estimate the state of the system given some measurements/observations.

What are the other terms in the equation?

$P(X=on)$ is the probability of the state variable or its often called the _prior_. In this case it is the probability of the light being on. Sometimes it is easy to measure by observing to state directly and counting the frequencies. But it becomes impractical if the number of state variables are larger. The prior is generally sent based on some heuristics. But if it is not known, it can be set to 0.5 (or random chance). The denominator term $P(Z)$ is the normalizing term. It expands to  
$$P(Z) = P(Z|X=on)P(X=on) + P(Z|X=off)P(X=off)$$

The first term on the summation is actually the numerator. $P(Z)$ is called the normalizing term because it ensures that the ratio ends up as a probability (quantity between 0 and 1).

If we observe closely we notice that we will need the entire probability distribution table to calculate the denominator which is mostly impractical as the number of state variables and measurement variables are usually large. So, for some calculations the denominator is not explicitly calculated. This is done by treating it as a constant $\eta$  
$$P(X=on|Z) = \eta P(Z|X=on)P(X=on)$$
The constant $\eta$  is then marginalized and eliminated. We will see where and how it is done in a different post.
