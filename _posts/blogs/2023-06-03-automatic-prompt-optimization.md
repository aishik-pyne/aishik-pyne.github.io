---
title: Automatic Prompt Optimization (ADO) with "Gradient Decent" and Beam Search
thumbnail: /assets/img/blog/2023-06-03-tree-of-thoughts/thumbnail.jpg
featured_image: /assets/img/blog/2023-06-03-tree-of-thoughts/banner.jpg
layout: blog
tags:
    - featured
    - paper-summary
    - LLMs
---

Given you have a task $x$, and an Large Language Model $LLM_p(x)$, what is a best prompt $p$ which produces the desired solution $y$? 

<!-- more-->

[“Automatic Prompt Optimization with ‘Gradient Descent’ and Beam Search.”](http://arxiv.org/abs/2305.03495) tries to answer this question, by optimizing the discrete prompt $p$, with non-paramteric "gradient decent" and [beam search](https://en.wikipedia.org/wiki/Beam_search#:~:text=In%20computer%20science%2C%20beam%20search,that%20reduces%20its%20memory%20requirements).

 

# Problem Formulation

The proposed Automatic Prompt Optimization (APO) framework assumes access to:
1. An **initial prompt** $p_0$ which is usually naively handcrafted to solve the task $T(x) \rightarrow y$, where the the prompt $p_0$, query $x$ and ground-truth output $y$ all belong to the space of coherent natural language $\mathcal{L}$
2. An **[i.i.d](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables) dataset**, $D_{tr} = \{(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)\}$ corresponding to the task $T$
3. A black box **LLM API** $LLM(p, x) \approx \argmax_{y\in \mathcal{L}} P_{LLM}(y|p, x)$, where $y$ is the most likely text continuation of the concatenated prompt $p$ and query $x$
4. A **metric** function $m(p, D_{te})$, which quantitatively measures how good the prompt $p$ is on a test dataset $D_{te}$

# Propsed Algorithm for APO

The proposed algorithm, iteratively refines the prompt $p_0$ to produce $\hat{p}$, which is an approximation of the optimal prompt $p^* = \argmax_{y\in \mathcal{L}} m(p, D_{te})$. It does so by performing "textual gradient decent" which "guides" the prompts towards improvement. Then it uses these gradients to beam search the space of coherent natural language

![Illustration of APO using Gradient Descent](/assets/img/blog/2023-06-03-automatic-prompt-optimization/apo_algo.jpg)  
*[R. Pryzant et al. 2023]((http://arxiv.org/abs/2305.03495)) Illustration of APO: The text dialogue tree we use to mirror gradient descent and overcome the discrete optimization barrier. First, a feedback prompt ∆ generates the gradient g from input data (x, y) and starting prompt p0 and prediction ˆ y (left). Second, an editing prompt δ applies the gradient g to the prompt p0 to produce an improved prompt p′ (right).*


Since this body of work uses the term "gradient descent" from a well-known literature in optimization theory, I find that it's imperative **we draw an analogy** of the *textual gradient descent* to the *regular gradient descent*.

In a regular gradient descent, we have a function $f$ parameterized by $\theta$, which given an input $x$ returns an output $y$. Our objective is to find the best parameters $\theta^*$, such that the error in prediction a.k.a loss $J(\theta) = y - \hat{y}$ is minimized. Iteratively, $\theta$ is updated by a small amount $\alpha$ in the opposite direction of the gradient $\frac{\partial J(\theta)}{\partial \theta}$ until convergence.

$$
f_\theta(x) = \hat{y} \\
J(\theta) = y - \hat{y} \\
\theta_{t+1} = \theta_t - \alpha \frac{\partial}{\partial \theta}  J(\theta)
$$

In this work, we can treat function $f$ to be the **LLM API** and the output of the **LLM API** to be parameterized by the prompt $p$. The loss term here is the error in prediction $J(\theta) \approx LLM_p(x) - y$. Finally the updated prompt $p_{t+1}$ is the output of updating the current prompt $p_t$ using the gradients $\frac{\partial J(\theta)}{\partial \theta}$. 

**Note:** All the operation here are in a discrete "coherent natural language space" $\mathcal{L}$ instead of the the real number space $\mathbb{R}$. Thus, each operation in the $\mathbb{R}$ space in *regular grad-descent* must have an analogy in the *textual grad-descent* process. 


## Gradient Descent with Prompts

Gradients provide a direction in which the parameter has to be updated. In this setting, gradient descent refers to 

1. Evaluating a prompt on some batch of data $D_{te}$ and collect the prediction errors: $\mathcal{E}$
2. Creating a local loss signal, which has qualitative information about what can be improved in the prompt to reduce $\mathcal{E}$
3. Editing or Updating the prompt $p_t$ in the opposite semantic direction of the gradient.

This is achieved by using the **LLM API** again with **2 static prompts**.

1. $\nabla$ prompt: This prompt creates the local loss signal $g = LLM_{p^{\nabla}}(p_t, \mathcal{E})$ which produced the summary of flaws of $p_t$ 
2. $\delta$ prompt: This prompts generates new $k$ prompts $p_{t+1}$ by editing the previous prompt $p_t$ i.e. $LLM_{p^{\delta}}(p_t, g) \rightarrow \{p_{t+1}^0, p_{t+1}^1, \dots, p_{t+1}^k\}$


**Examples of the prompts used in paper**

- Example of $\nabla$ prompt
    ```
    I'm trying to write a zero-shot classifier prompt. 

    My current prompt is: "{prompt}" 

    But this prompt gets the following examples wrong: {error_string} 

    give {num_feedbacks} reasons why the prompt could have gotten these examples wrong.

    Wrap each reason with <START> and <END>
    ```
- Example of $\delta$ prompt
    ```
    I'm trying to write a zero-shot classifier. 
    
    My current prompt is: "{prompt}" 
    
    But it gets the following examples wrong: {error_str} 
    
    Based on these examples the problem with this prompt is that {gradient} 
    
    Based on the above information, I wrote {steps_per_gradient} different improved prompts. 

    Each prompt is wrapped with <START> and <END>. 

    The {steps_per_gradient} new prompts are:
    ```

## Beam search over prompts

Regular gradient descent provides a *guarantee* that the parameter update operation $\theta_{t+1} = \theta_t - \alpha \frac{\partial}{\partial \theta}  J(\theta)$ reduces the prediction error in the subsequent step. However, in this case since, the gradients are qualitative errors and an LLM is used to generate the updated prompt, we lose the *guarantee* that the prediction error will be reduced.

To tackle this loss of *guarantee*, beam search is used to maintain a *beam* or *pool* of prompts and search through $\mathcal{L}$. The beam search consists of two steps:

1. **Expansion Step**: Sample possible prompts (using an LLM) in the discrete space, and then 
2. **Selection Step**: Prune out the prompts which produce a bad evaluation score on the test dataset. 

### Expansion Step


The expansion step is used to generate many new candidate prompts from existing prompts, ideally better prompts directed by the gradients.

![Expansion Step](/assets/img/blog/2023-06-03-automatic-prompt-optimization/expansion_step.jpg)
*The algorithm for the expansion step*

**Note:** In Step 5, monte carlo successors refers to prompts which are differently worded but semantically similar to their inputs. (My personal take: I'm not sure why they call this kind of expansion the local monte-carlo search space)

### Selection Step

After the new prompts are generated, they are evaluated on the test set, and the weaker prompts are pruned out till the beam width is reached again. 

However, naively evaluating each prompt on the entire dataset is expensive [Prasad et al., 2022](https://arxiv.org/pdf/2203.07281.pdf). When under as evaluation budget, the selection step must be computed efficiently.

![Selection Step](/assets/img/blog/2023-06-03-automatic-prompt-optimization/selection_step.jpg)  
*Selection Steps: (1) Select using UCG (2) Selection using Successive Rejects.*

The authors propose two algorithms:

1. The UCB: This chooses the best arm in the multi-arm bandit formulations, where $b$ best arms are to be selected with the least "pulls" as possible.
2. Successive Rejects: This maintains a list of surviving prompts, where one prompt is rejected at each iteration. It is done by using all prompts on $n_t$ random points and then dropping the prompt which ges the lowest score. Unlike UCB this requires no hyperparameters. 

## Experiments

I would leave it to the reader to go through the experiments from the paper itself.

# Conclusions

This work is one of the many works which try to optimizize a prompt for a task. Other works include (1) Automatic Prompt Engineer [Zhou et al.](http://arxiv.org/abs/2211.01910) (2) GrIPS: Gradient-free, Edit-based Instruction Search for Prompting Large Language Models ([Prasasd et al.](http://arxiv.org/abs/2203.07281))

I like the authors approach to this work as they develop a method around the well established "gradient descent" and beam search concept.  
However, I would have liked if the paper:
1. Delved deep into why can LLMs critique themselves but not perform the desierd task? 
2. If LLMs can critique themselves, why is an iterative approach required? Why can't LLMs give the best prompt in one go?
3. Finally, can this approach lead to over fitting to the $D_{tr}$? i.e, if the prompt is optimizited for a Task which is represented by $D_{tr}$ and optimized based on performance on $D_{te}$, can it generalize to tasks in $T - \{D_{tr} \cup D_{te}\}$

--- 

## Citation

Cited as 

```
@article{pyne2023apo,
  title   = "Summary of Automatic Prompt Optimization",
  author  = "Pyne, Aishik",
  journal = "aishikpyne.com",
  year    = "2023",
  month   = "June",
  url     = "https://aishikpyne.com/blog/2023-06-03-automatic-prompt-optimization/"
}
```

## References

[1] R. Pryzant, D. Iter, J. Li, Y. T. Lee, C. Zhu, and M. Zeng, [“Automatic Prompt Optimization with ‘Gradient Descent’ and Beam Search.”](http://arxiv.org/abs/2305.03495) arXiv, May 04, 2023. doi: 10.48550/arXiv.2305.03495.

[2] A. Prasad, P. Hase, X. Zhou, and M. Bansal, [“GrIPS: Gradient-free, Edit-based Instruction Search for Prompting Large Language Models.”](https://arxiv.org/pdf/2203.07281.pdf) arXiv, Apr. 26, 2023. doi: 10.48550/arXiv.2203.07281.
