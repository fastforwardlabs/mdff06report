## The Challenge of Interpretability

In the previous chapter we saw the power of interpretability to enhance trust,
satisfy regulations, offer explanations to users, and improve models. But we
also saw that there is a fundamental tension between these goals and a model's
ability to get decisions right. Traditionally, you can have an interpretable model
or you can have an accurate model, but you can't have both.

![FIGURE 3.1 How do we get a model that is both highly interpretable and highly accurate?](figures/3-09.png)

In this chapter we'll first explain the technical reasons for this tension
between interpretability and accuracy. We'll then look at two ways to have your
cake and eat it. We'll take a tour of a handful of new "white-box" modeling
techniques which are extremely interpretable by construction, but retain
accuracy. We'll then introduce the idea that is the technical focus of the
report: interpretation of black-box models by perturbation and, in particular,
LIME.^[This
chapter is not an exhaustive discussion of techniques that can be used to make
models more interpretable. We focus on the new ideas we're most excited
about. For a more complete list, we heartily recommend the clear and
comprehensive white paper *["Ideas on Interpreting Machine Learning"](https://www.oreilly.com/ideas/ideas-on-interpreting-machine-learning)* from H2O.]

### Why Are Some Models Uninterpretable?

What is it about a model that makes it uninterpretable? Let's first look at the
gold standard of interpretability -- linear models -- in the context of a
classification problem (the difficulties with regression models are not
qualitatively different). Suppose we're classifying borrowers as likely to
repay or not. For each applicant we will have access to two pieces of
information: their annual income, and the amount they want to borrow. For a
training sample we also have the outcome. If we were to plot the training data,
we might see something like this:

![FIGURE 3.2 Linear models are easy to understand and explain.](figures/2-11.png)

At a high level, this training data shows that people who repay tend to earn a
lot and borrow a little. But the details are important. You can draw a straight
line on this chart that separates the repayers and
non-repayers. You can then build an accurate model by simply asking the question, "Is
the applicant above or below the line?" Formally, this is a _linear model_; i.e.,
one in which the predicted repayment probability is a linear function of income
and loan amount.^[This discussion skips a mathematical detail -- the
sigmoid function -- but without loss of generality.] In other words:

    Probability of repayment = A × income + B × loan amount

where the coefficients `A` and `B` are two numbers. 

Such a model is interpretable. `A` is just a number, and not a function of any
other number, so we can easily check whether it is positive. If it is, we can
know with certainty that repayment probability increases with income in our
model. This directional behavior probably matches the expectations of domain
experts, which is reassuring to us and to regulators. The structure of the
equation means that this trend will _always_ be true. It's mathematically
impossible for some obscure combination of income and loan amount to imply that
repayment probability decreases with income. That mathematical certainty means
we can be confident that there is no hidden behavior lurking in the model. Our
trust in the model is high. And we can use the numbers `A` and `B` to tell a
borrower why we think they are unlikely to repay in precise but plain words
(e.g., *"Given your income, you are asking to borrow $1,000 too much."*).

![FIGURE 3.3 Given a new data point, we can explain why it is classified the way it is.](figures/2-12.png)

Let's look at a tougher problem. Suppose we plot the longitude and latitude of
temperature sensors in a field, and mark with a check or cross whether the
yield of corn was high or low:

![FIGURE 3.4 Many problems are not linearly separable.](figures/2-13.png)

As you can see, there is no way to draw a straight line
that separates the high-yield and low-yield areas of this field. That means it
will be impossible to build an accurate and maximally interpretable linear
model solely in terms of latitude and longitude.

The obvious thing to do in this particular case would be to "engineer" a
feature that measured distance from the center of the field (which is a
function of both longitude and latitude). It would then be simple to build a
model in terms of that single derived feature. Feature engineering, however,
is time-consuming and can require domain expertise. Let's suppose we didn't
have the time or expertise. In that case we might graduate from a linear model
to a Support Vector Machine (SVM).

An SVM essentially automates the process of engineering our "distance from the
center of the field" metric. It does this by distorting the 2D surface on which
the points in the previous figure sit into three or more dimensions, until it
is possible to separate the high- and low-yield areas of the field with a plane.

![FIGURE 3.5 The classification for the nonlinear crop data.](figures/2-19.png)

This model will be accurate, but the distortion of the inputs means that it no
longer operates in terms of our raw input features. We cannot write down a
simple equation like our loan repayment equation that allows us to say with
confidence exactly how the model responds to changes in its inputs in all parts
of the field. There _is_ an equation, but it's longer and more complicated. It
has therefore become harder to give a simple explanation of why an area is
predicted to have high or low yield. If a farmer wants to know whether moving
to the west will increase yield, we have to answer that it depends on how far
north you are. Our model's internal structure is a step removed from the
relatively intuitive raw input.

![FIGURE 3.6 There is no longer a simple explanation for why a data point is classified the way it is.](figures/2-14.png)

If we take one more step up in problem and model complexity, the internal
structure of the model gets still more removed from the input. A neural network
used to classify images does an exponentially large number of transformations
similar to but more complex than the single one performed by an SVM. The
equation it encodes will not only be very long, but almost impossible
to reason about with confidence.

![FIGURE 3.7 More complex models create a space that is even more difficult to explain.](figures/2-15.png)

A random forest model is often used where the problem is hard and the main
concern is accuracy. It is an _ensemble_, which means that it is in a sense a
combination of many models. Although the constituent models are simple, they
combine in a way that makes it extremely difficult to summarize the global
model concisely or to offer an explanation for a decision that is locally true.
It is all but impossible to rule out the possibility that the model will exhibit
nonsensical or dangerous behavior in situations not present in the training
data.

### White-box Models

The least interpretable models, such as neural networks, are free to choose
from an almost infinite menu of transformations of the input features. This
allows them to divide up the classification space even if it is not linearly
separable. New white-box models have a smaller menu of transformations to choose
from. The menu offers a big boost in freedom to classify with accuracy, but is
carefully chosen with interpretability in mind too. Generally speaking, this
means that the model can be visualized or is sparse. Visualization is one of
the most powerful ways of immediately grasping how a model works. If it is not
possible, then the model is much harder to interpret. Models that are sparse,
meanwhile, are mathematically simple in a way that raises the possibility that
they can be written down as a set of simple rules.

![FIGURE 3.8 White-box models are highly interpretable. Recent innovation is focused on increasing their accuracy.](figures/3-10.png)

#### GAMs

Generalized additive models (GAMs) are a great example of this carefully
controlled increase in model flexibility. As we saw earlier, a linear
classification model assumes that the probability a given piece of data belongs
to one class rather than another is of the form:

    Ax + By + Cz

where the coefficients `A`, `B`, and `C` are just constant numbers, and `x`, `y`, and `z` are
the input features. A GAM allows models of the form:

    f(x) + g(y) + h(z)

where `f` and `g` are functions. This model is "generalized" because the
constant coefficients have been replaced with functions -- in a sense, the
constant coefficients are allowed to vary, which gives the model more
flexibility. But it is "additive" because the way in which `f(x)`, `g(y)`, and
`h(z)` are combined is constrained, which means the flexibility is not total.
In particular, the functions `f`, `g`, and `h` are each functions of one of the
features only, and the terms `f(x)`, `g(y)`, and `h(z)` must be combined by
simple addition.

This careful loosening of the constraints of linear models allows higher
accuracy, but retains the ability to construct _partial dependence plots_.
These are simply graphs of `f(x)`, `g(y)`, and `h(z)` that allow us to
visualize the behavior of the model. These graphs can be examined to ensure
that there are no dangerous biases lurking in the model or, if necessary, to
demonstrate to a regulator that a model responds _monotonically_ to a
particular feature.^[`f(x)` is monotonic if it _always_ increases when
`x` increases.]

![FIGURE 3.9 Partial dependence plots let you see the relationship between the prediction and key features.](figures/3-02.png)

GA^2^2Ms extend GAMs by allowing _pairwise_ interacti^ons, or models of the form:

    f(x) + g(y) + h(z) + p(xy) + q(yz)

That is, they allow a tightly constrained version of the kind of feature
engineering we saw in the field yield example earlier. There is in principle
nothing to stop us introducing a model with another term, `r(xyz)`, but GA^2^Ms
stop here for a very good reason: you can make a partial dependence plot of
`p(xy)` by drawing a heatmap, but it is extremely difficult to make a partial
dependence plot of three variables. It is the partial dependence plots that
give the model its interpretability.

![FIGURE 3.10 Partial dependence plots with pairwise interactions.](figures/3-03.png)

We've described a situation with just three features, `x`, `y`, and `z`. But
it's more common to have many more features. This raises the possibility of
exponentially many pairwise terms. For example, with 50 features there are over
a thousand possible pairwise terms. Trying to inspect all these plots would
diminish interpretability rather than enhancing it. For this reason, GA^2^Ms
include only the `k` pairwise terms that most improve accuracy, where `k` is a
small number determined like any other hyperparameter.^[See Lou et al.
(2013), [*"Accurate Intelligible Models with Pairwise Interactions,"*](http://www.cs.cornell.edu/~yinlou/papers/lou-kdd13.pdf) and the reference
[Java implementation](https://github.com/yinlou/mltk).]

#### Rule Lists

Rule lists are predictive models that learn simple flow charts from training
data. The models are made up of simple `if...then...else` rules that partition
the input data. These rules are the building blocks of rule lists. For
example, it's possible to predict survival of passengers on the _Titanic_ using
a rule list like this:

![FIGURE 3.11 An example rule list predicting the survival of passengers on the Titanic.](figures/3-14.png)

Rule lists are special cases of decision trees, where all the leaves are on the
same side of the tree. As such, they are highly interpretable.

Bayesian rule lists (BRLs)^[[https://arxiv.org/abs/1511.01644](https://arxiv.org/abs/1511.01644)] and
falling rule lists (FRLs)^[[https://arxiv.org/abs/1411.5899](https://arxiv.org/abs/1411.5899)] are recent
instantiations of this general approach. Given a catalog of rules learned from
data, BRLs use a generative model to select a smaller subset of highly probable
rules that best describe the data the rules were learned from. In the case of
FRLs, the model is structured so that the selected rules are ordered by
importance, which allows users to more easily identify the important
characteristics.

Rule list methods are a good fit for categorical data. Numerical data can be
discretized, but if the statistical relationships among input attributes are
affected by discretization, then the decision rules learned are likely to be
distorted.

#### SLIMs

The APGAR score for newborn infants (see [Explaining Decisions](#explaining-decisions)) is calculated by
assigning scores of 0, 1, or 2 to five attributes of the baby and adding them
up:

    APGAR = appearance + pulse + grimace + activity + respiration

As we discussed, the fact that this score can be calculated quickly and reasoned
about easily is an important feature. Scores like this are in widespread use in
clinical medicine. But this extreme simplicity necessarily comes at the expense
of accuracy.

In the same way GA^2^Ms use a tightly controlled freeing up of a linear
classifier to increase accuracy, Supersparse Linear Integer Models
(SLIMs)^[[https://arxiv.org/abs/1502.04269](https://arxiv.org/abs/1502.04269)] make a small change to
scoring systems to increase their accuracy. That change is to permit each
attribute to be multiplied by an _integer_ coefficient. With this additional
freedom, the APGAR score might become:

    APGAR = 5 appearance + 3 pulse + 2 grimace + 7 activity + 8 respiration

A score like this is almost as easy to work with as the original APGAR score,
and potentially hugely more accurate. The challenge is figuring out how to
choose the numbers. SLIMs cast this problem as a supervised machine learning
problem and use the tools of integer programming to ensure the coefficients
are round numbers.
