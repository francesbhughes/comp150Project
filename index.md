# Comp 150: Algorithms and Data Structures 

This is a discussion of Total Stability in Stable Matching Games and an analysis of the Sushmita Gupta, Kazuo Iwama, and Shuichi Miyazaki's algorithm on how to provide total stability in matching. 

## Background Information 

The Stable Marraige Problem (SMP) is seen as a game where each player wanted to obtain the best possible partner by manipulating his or her preference list. Given a set of n women and n men, each man has ranked n women in terms of preference and each woman has ranked n men in terms of preference, find the best possible pairing where each man and woman have a partner. 

Gale and Shapley (GS) devised an algorithm to solve the stable marraige problem. The goal of the GS algorithm is to find a stable matching, which occurs if a matching does not have a blocking pair. A blocking pair is a pair of a man and a woman who prefer each other to their current matching partners. 

There may be more than one stable matching for a given set of men and women based on the proference lists, iee there are men-optimal and women-optimal stable matchings. For the simplicity of this project and given the Gupta, Iwama, Miyazaki algorithm, we will focus promarily on a men-proposing algorithm, which given the GS algirthm find the men-optimal stable matching in linear time. 

The SMP is much like any other game - each player is inheretly selfish and wants to obtain the best possible partner, even if it means stealing one from the other players. Because of this, there are ways to manipulate the system in order to obtain a better partner. Specifically, given this is a men-proposing algorithm, women may use a list Q in the game which differs from their true preferences P. 
For this algorithm we will then consider the true preferences P = (P(M), P(W)) and the stated preferences Q = (P(M), Q(W)) where P(W) and P(M) are the sets of true preferences and Q(W) is the set of stated preferences for the women. The goal of the algorithm is to check is Q is Nash Equilibrium. A strategy Q is at Nash Equilibrium with respect to P is no single player can get a better partner by changing his/her list in Q while all others use their respective lists in Q. 

The Model for the SMP consists of:
```markdown
(i) A true strategy, P
(ii) An arbitrary stated strategy, Q
(iii) Two different notions of stability for : 
   (a) one based on the abses of P-Blocking Pairs
   (b) another based on Nash Equilibrium with respect to P
```
An example of the differences between a true strategy P = (P(M), P(W)) and a stated strategy Q = (P(M), Q(W)): 

Men: | P(M)    |  Women |  P(W)     |   Q(W)    |
|----| ------- | ------ | --------  |-----------|
1:   | a b c d |    a:  | 2 3 *1* 4 | *3* 4 1 2 |
2:   | b a c d |    b:  | 1 *2* 3 4 | *1* 2 3 4 |
3:   | b c a d |    c:  | 2 *3* 4 1 | *2* 3 4 1 |
4:   | a d b c |    d:  | *4* 1 2 3 | *4* 1 2 3 |

This example illustrates the difference between the cases when P = Q and when P ≠ Q. As we can see, P and Q only differ in the list of a. 

A strategy Q with respect to the strategy P is totally stables if (a) µQ is P-stable, where µQ is the men-optimal stable matching and (b) for any woman, Q' yields no better partner for w in terms of P.

The main goal of Gupta, Iwama, and Miyazaki's algorithm is to extend the work done in the GS algorithm beyond to a more general setting of when P ≠ Q. This will be examined next. 

## Gale-Shapley Algorithm (Men-Proposing) 
The men-proposing Gale-Shapley algorithm (GS-M) would work by doing the following: a man who is not matched proposes to the woman at the top of his list. When a woman w received a proposal from man m, she accepts the proposale if it is her first propposal or if she prefers m to her current partner m'. If w prefers her current partner to m, then she rejects m. If m is rejected, then he will move to the next woman in his list and continue the process. This happens for every man until there is no man left unmatched. If there are two or more unmatched men, then the man with the smallest index is first to propose making this algorithm is purely-deterministic. 

## Gupta, Iwama, Miyazaki Algorithm
They have set out to solve to answer the questions, is Q totally stable when there are two inputs: a true strategy P = (P(M), P(W)) and a states stratefy Q = (P(M), Q(W)). 

**Algorithm 1:** A(Q,w) 
```markdown
**Input:** Stategy Q = (P(M), Q(W)) and a woman w 
**Output:** Sets N<sub>w<sub>(Q) = {m ∈ M | ∃ Q' ∈ S (<sub>QW</sub>) that yields µ<sub>Q</sub>o(w) = m}, and L<sub>w</sub>(Q) = Q'(m;w) | m ∈ N<sub>w</sub>(Q), Q' = (Q(-w), Q'(m;w)) yields µ<sub>Q</sub>o(w) = m}

1. Let x1 be the first active man in Q(w) 
2. Let N ← {x1} and L ← {Q(x1; w)}
3. **Explore** Q(x1;w)
4. **return** (N,L)

Procedure **Explore**Q'(x,I;w)

1. Let A ← {men who are active in Q'(x,I;w) after x}
2. **foreach** y ∈ A \ N **do**
   N ← N U {y} and L ← L U {Q'(y,x,I;w)}
   **Explore**(Q'(y,x,I;w)
```
The above algorithm outputs the set Nw(Q) of all possible partners m of a given fixed woman w with a manipulated strategy Q' = (Q(-w), Q'(w)), meaning the men-optimal stable matching will match w to m. We can use this algorithm n times (for each man) to obtain N<sub>w1</sub>(Q),..., N<sub>wn</sub>(Q). 

The **Explore** step takes a parameter Q(x, I,; w), which is the preference list of w, x is the front of the list, and I is the sublist. **Explore**Q(x,I;w) completes the male-proposing Gale-Shapley algorithm for Q' and creates a set of men A who propose to w after x. So, for each new y ∈ A, we add y to N, move it to the top of Q(x,I;w) and call **Explore** recursively.  

**Explore** is called once for every man in N, therefore the time complexity of the above algorithm is n * the time complexity to executive men-propsing Gale-Shapley (T(GS)). Overally this makes the time complexity O(<sup>n</sup>)

Algorithm 1 is proven current through various Theorem's and Lemma's described below: 

### Theorem 1 
```markdown
For a strategy **Q** and a woman w ∈ **W**, Algorithm 1 produces N = {m ∈ M | ∃ **Q**' ∈ Q~w~, s.t. µ~Q~o(w) = m} and for each m ∈ N, a list Q(m, I;w) such that for some partial list I, m is active in Q(I;w)
```

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/francesbhughes/comp150Project/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.





You can use the [editor on GitHub](https://github.com/francesbhughes/comp150Project/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.
