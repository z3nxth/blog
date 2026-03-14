---
title: Syntactic and Semantic Consequences
date: 2026-03-14
categories: Logic and Philosophy
tags:
  - logic
  - guide
---

Whilst the science of logic can feel abstract, at its core, it is understanding how conclusions follow from premises. This act of 'following' can happen twofold (fundamentally) through syntactic and semantic consequences.

If you've dabbled in logic here and there, the terms "syntactic consequence" and "semantic consequence" may have been strewn around in a fanciful or perplexing manner. Breaking them down shows us they're far from as convoluted as we imagine them to be.
#### What is a "consequence"?
In logic, a consequence is used to describe when a statement follows from some premise. This following can happen in two ways:
- Syntactically (following the rules of a formal system [eg. proofs or derivations])
- Semantically (following the truth of statements in all possible scenarios [eg. models and meanings])

### Syntactic Consequences
This is a consequence in which you can derive a statement using *formal rules*.
Symbolically, we can write this as
$$
\Gamma \vdash \phi
$$
where:
- Γ is our set of premises
- The symbol ⊢ means "can prove [the following] based on rules"
- φ is our conclusion.

We don't concern ourselves with if Γ or φ are true in the real world. We only followed formal *proof* rules.
##### Example
Γ:
1. $$P \implies Q$$ (in other words, if P then Q)
2. $$P$$ (P is true)
If we [affirm the antedecent](https://en.wikipedia.org/wiki/Modus_ponens) (Modus Ponens: i.e., if P is true, Q is true, P is true ∴ Q is true), we can now derive Q:

$$
\{P \implies Q, P\} \vdash Q
$$

Remember, we didn't care if P or Q were actually true in the real world, we simply followed the formal proof rules.
### Semantic Consequences
This is a consequence in which a statement follows because of its **truth in every possible interpretation**. 

Symbolically:
$$
\Gamma \models \phi
$$
where:
- Γ is our set of premises
- The symbol ⊨ means "logically entails" (is true in every possible scenario. Note that another word for "every possible scenario" is model, which we will employ.)
- φ is our conclusion.

Here, instead of applying proof rules, we consider all models in which the premises could possibly be true. If in every model the conclusion must also be true, the conclusion is a semantic consequence of the premises.
##### Example
Consider the same premises:
1. $$P \implies Q$$
2. $$P$$
If both of the above statements are true, Q must be true *in all models*, or in other words, **Q is a semantic consequence of Γ.** Therefore, there is *no possible scenario in which Γ is true and Q/conclusion is false.*

We can thus write:
$$
\{P \implies Q, P\} \models Q
$$

Unlike our syntactic consequences, this conclusion was reached through the examination of the truth of statements, not through proof rules.
### Summary of differences
In summary:
Both consequences describe when something follows from premise(s) Γ, but both approact this differently:

- Syntactic consequence is about proofs, so we derive conclusions through applying inference rules.
- Semantic consequence is about truth, and we check whether the conclusion must be true in every situation where the premises are true.

Recall that syntax pertains to the formal manipulation of symbols while semantics concern themselves with meaning and truth throughout interpretations.

### Bringing syntax and semantics together
Whilst at first glance, it may be hard to tie the two together, one of the most important results in logic shows that these two perspectives fundamentally agree. When discussing propositional logic, if something can be proven by Γ then it is also true in all models when Γ is true, and vice versa.

or symbolically:

$$
Γ \vdash ϕ \iff Γ \models ϕ
$$
This means our proof mechanisms are both sound (they only prove truths) and complete (they can all prove logical truths).

Thus, whether we reason through formal proof rules or through truth in models, we arrive at the same conclusions.

It's always interesting to see how elegant logic really is when one can arrive at the same destination from both syntax and semantic.
