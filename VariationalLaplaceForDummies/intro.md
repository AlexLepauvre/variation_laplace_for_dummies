# Variational Laplace for Dummies

<div class="alert alert-info"><h4>Note</h4><p> 
    This notebook is still work in progress and the content has not been fact checked! <a href="url">here</a>.
</p></div>

Introduction

Welcome to Variational Laplace for dummies: A Journey from Zero to Understanding. This book is born out of a personal journey—a quest to understand complex statistical methods without the heavy mathematical background often assumed by experts in the field.

As a biologist with a PhD in consciousness neuroscience, my academic path included minimal formal training in advanced mathematics. My experience with statistics was rooted in practical applications: from simple t-tests to more complex linear models, and incorporating machine learning and multivariate methods into my research. However, as I delved deeper into the nuances of scientific inquiry, I found myself drawn to Bayesian statistics.

Bayesian methods appealed to me for their flexibility and their ability to provide a formal quantification of evidence. They align closely with the kinds of questions that drive scientific exploration: "How much evidence do I have for a particular hypothesis or theory?" Specifically, I became interested in the Variational Laplace method. It offers a way to compute the model evidence component of Bayes' theorem for a wide array of models used in neuroscience, including univariate and multivariate generalized linear models, whether mixed or not.

However, I quickly felt overwhelmed when reading papers and textbook explaining variational laplace. The existing papers and resources on Variational Laplace were dense, full of what looked like complex mathematics, and often assumed a level of prior knowledge I did not possess. It wasn't that the authors failed to explain the concepts, but rather that certain connections that were obvious to them and their target readers were lost on me. I ended up confused, mixing up concepts, not sure which symbol refers to what and so on. You probably know the feeling. 

I was however really determined to understand it, because I wanted to write papers leveraging this method quite heavily to answer questions I couldn't otherwise. To my surprise, I discovered that the core concepts could be grasped using the basic algebra and arithmetic I learned in high school. The real challenge was not the complexity of the mathematics itself but the way it was presented—by people better versed in mathematics, for people better versed in mathematic than I am.

That's why I wrote this book: variational Laplace by a dummy, for dummies (no offense intended). My goal is to provide a clear, accessible introduction to Variational Laplace for statistical modeling and inference, aimed at readers who, like me, may not have an extensive mathematical background. We will start with the simplest generalized linear model: a linear model with a single regressor. From there, we'll gradually build up to more complex models, adding additional regressors and eventually exploring multivariate modeling.

Throughout the book, I've employed several strategies to make the material as understandable as possible:

- Detailed Explanations: I explain concepts thoroughly, sometimes repeating ideas in different ways to reinforce understanding.
- Mathematical Formulas with Plain Language: Each formula is accompanied by a verbal explanation, clarifying how the mathematical expressions align with the concepts discussed.
- Python Code Translations: Every significant formula is translated into Python code. This not only helps in visualizing the concepts but also makes the material accessible to those who may be more comfortable with programming than with mathematical notation.
- Interactive Jupyter Notebooks: The content is presented using Jupyter notebooks, allowing for hands-on exploration. Readers can modify code, see immediate outputs, and engage with visualizations that reinforce the material.

While the primary focus of this book is on Variational Laplace as a statistical tool, it's worth noting its broader significance in neuroscience. The mathematics we explore here form the backbone of the Free Energy Principle (FEP) and frameworks like Predictive Processing, which are influential in modern neuroscience. By understanding the material in this book, you'll also be better equipped to delve into these advanced topics, even though they are not our main focus. Consider it an added bonus—a "freebie"—that enhances the value of your learning experience.

It's important to mention that this book is a work in progress. To ensure accuracy, I've collaborated with a co-author who is well-versed in mathematics. They've reviewed the content to confirm that it makes sense from a mathematical standpoint. Additionally, other colleagues have provided feedback to enhance clarity and correctness. However, as the material has not yet been fully verified and proofread by mathematicians, I encourage you to approach it with a critical mind and consider it a stepping stone rather than a definitive resource.

In writing this book, I aim to open doors for those who have felt intimidated or excluded by the complexity of advanced statistical methods. Whether you're a fellow biologist, a neuroscientist, or simply someone curious about Bayesian statistics, I hope this book serves as a friendly guide on your journey to understanding Variational Laplace and its applications.

Let's get started.


If you are reading this book, you are interested in understanding what variational Laplace is, how it is used to do Bayesian statistics, how it relates to neuroscience, active inference, predictive processing, or something along these lines. If you are like me and have very little knowledge in mathematics, you may have encountered many papers that try to explain variational Laplace and felt intimidated by it. That is not to say that people did a bad job at explaining it in existing text books and papers, there are great resources out there. However, most of these resources require the reader to be already familiar with quite advanced mathematical concepts. This notebook was created to be maximally accessible. I wrote this notebook as I was reading Peter Zeidman's "A primer on variational Laplace", and it is me explaining myself the bits of maths I was missing.

I will start by introducing Bayesian statistics using very simple example. I will build from very simple intuition that I believe we all share to introduce mathematical formalism and show how that helps us go beyond our simple intuition. I will show how Bayes theorem aims at reaching the best possible conclusion based on data when we are dealing with uncertainty.

I will then proceed to show that while the Bayes Theorem (which is a single equation) is really helpful, it can in most cases not solvable analytically. This means that we can't solve the equation by hand to then be able to plug in the values that we have to get the answer to our question. And even more unfortunately, for most interesting questions we try to answer with statistics (the weight of a regressor in a linear regression for example), computational solutions are also not feasible. This means that we can't try a bunch of options with a computer to find something that comes close to a solution, because there are way too many things we can try, and we would need to let our computer run for decades to get answers to even simple problems. Don't worry if this all sounds a bit confusing right now. The point is that while Bayes Theorem can tell us which is the best conclusion to reach based on some data, we very often can't get that answer straight away, because the maths required to do so are too complicated or simply impossible.

Following a detailed explanation illustrated with simple example as to why we cannot in most cases solve the Bayesian theorem, I will show that we can instead find mathematical tricks to solve something else. That something else should get us an answer that is close enough to what we would have gotten if we had actually solved the Bayes theorem. Even better, we can know that the larger that value gets, the closest we are approaching the answer the Bayes theorem would have given us. This means that we can strive to always get higher values to get a more accurate answer, and you will see then why that is a good thing. This quantity that we will seek to compute is the so called Free Energy. In order to compute this quantity, we need to use a lot of mathematical tricks to make impossible equations possible, and these series of tricks is what is referred to as Variational Laplace.

Hopefully, once we are done with all of this, you should have a good understanding of how Variational Laplace works. At least that's how it worked for me.

```{tableofcontents}
```
