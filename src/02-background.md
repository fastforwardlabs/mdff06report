[[why]]
## The Power of Interpretability

If a model makes correct decisions, should we care how they are made? More
often than not, the answer is a resounding yes. This chapter explains why.

[[what]]
### What Is Interpretability?

From the point of view of a business deploying machine learning in a process or
product, there are three important kinds of interpretability:

 - *Global* -- Do you understand the model _as a whole_ to the extent required
   to _trust_ it (or to convince someone else that it can be trusted)?
 - *Local* -- Can you explain the _reason_ for a _particular decision_?
 - *Proxy* -- When the model is a perfect proxy for the system you are
   interested in, can you say how the model works, and thus learn about how the
   real system works?

[[figure_global_interpretability]]
![Global interpretability shows feature importance for the model's prediction at a global level. Local interpretability shows feature importance for the model's prediction at a record-by-record level.](figures/2-03.png)

When one or more of these conditions holds, it makes our use of data safer and
opens the door to new kinds of products.

[[figure_trusted_model]]
![If you trust a model, interpretability can provide you with concrete actions to pursue.](figures/2-04.png)

[[trust]]
### Enhancing Trust

Data scientists have a well-established protocol to measure the performance of
a model: _validation_. They train a model with perhaps 80% of their training
data, then measure its performance on the remainder. By assessing the model
using data it has never seen, they reduce the risk that a powerful model with a
lot of flexibility will simply memorize the training data.

[[figure_model_validation]]
![Model validation can prevent overfitting.](figures/2-06.png)

This possibility, known as overfitting, is a concern because the model will one
day be deployed in the wild. In this environment, by definition, it cannot have
seen the data before. An overfitted model does not capture fundamental, general
trends in data and will perform poorly in the real world. Validation during
training diminishes this risk. It is an absolute minimum requirement for
building a trustworthy model.

But memorization or overfitting is not the only danger that lurks during
training. If the training data has patterns that are not present in real-world
data, an apparently good model will detect these patterns and learn to depend
on them, and then perform poorly when deployed.

Some differences between training and real-world data can be very obvious. For
example, if you train a self-driving car on public roads in a country where
people drive on the left, and then deploy that car in the United States, you're
asking for trouble.

[[figure_model_validation_abstract]]
![Validation does not help when training data and real-world data are too different.](figures/2-07.png)

But sometimes subtler discrepancies can exist in the training data without your
knowledge. A memorable example taken from a 2015 paper makes this point
clearly.footnote:[Caruana et al. (2015),
link:http://people.dbmi.columbia.edu/noemie/papers/15kdd.pdf["Intelligible
Models for HealthCare: Predicting Pneumonia Risk and Hospital 30-day
Readmission."]] Doctors and statisticians trained a model to predict the
"probability of death" of patients suffering from pneumonia. The goal was to
identify high-risk patients who should be admitted to hospital, and low-risk
patients for outpatient treatment. With enough data, the conceit of machine
learning is that it is able to identify patterns that doctors might miss, or
that might be glossed over by crude protocols for hospital triage.

When the researchers analyzed the model (using some of the techniques we
discuss in this report), they realized that the model wanted to treat patients
with asthma as low-risk outpatients. Of course, the model was wrong: people
with asthma are at _high_ risk if they catch pneumonia and should be admitted
to the intensive care unit. In fact, they often are, and it was this that
caused a problem in the training data. Asthma patients receive excellent care
when they have pneumonia, so their prognosis is better than average.

A model that captures this will perform well by the standard training metrics
of accuracy, precision, and recall, but it would be deadly if deployed in the
real world. This is an example of _leakage_: the training data includes a
feature that should not be used to make predictions in this way. In this case,
the model depended on a flawed assumption about the reason for the correlation
between asthma and pneumonia survival.

[[figure_asthma]]
.Models built from training data can lack context for certain relationships.
image::figures/2-01.png[]

It is obviously essential to be confident that you haven't embedded bugs like
this into a statistical model if it is to be used to make life-and-death
medical decisions. But it's also acutely important in any commercial setting.
At a minimum, we need to understand how a model depends on its inputs in order
to verify that this matches our high-level expectations. These perhaps come
from domain experts, previous models that have worked well, or legal
requirements. If we can't do this, then we can't be confident that it will
behave correctly when applied on data that was not in the training or
validation set. If we can, then we don't just get a feeling of confidence: we
gain the power to explain decisions to customers, to choose between models, and
to satisfy regulators.

This trust is particularly important in machine learning precisely because it
is a new technology. Rightly or wrongly, people tend to distrust novel things.
Machine learning will only earn the trust of consumers, regulators, and society
if we know and communicate how it works.

[[regulations]]
=== Satisfying Regulations

In many industries and jurisdictions, the application of machine learning (or
algorithmic decision-making) is regulated. This report does not offer legal
advice. To the extent that we discuss these regulations in any specific detail,
we do so in <<ethics>>. We bring them up here for two reasons.

First, if these regulations apply, they almost always imply an absolute
requirement that you build interpretable models. That is because the goal of
these regulations is often to prevent the application of dangerous or
discriminatory models. For example, you may be required to prove you haven't
overfitted. An overfitted model won't work in the real world, and deploying one
may hurt more than just your own bottom line. You may be required to prove that
dangerous predictive features haven't leaked in, as in the pneumonia treatment
model that sent asthma patients home. And you may be required to show that your
model is not discriminatory, as is the case if a model encourages a bank to
lend to borrowers of a particular race more often.

In a regulated environment, it is insufficient to show that these problems were
not present in your training data. You must also be able to explain the model
derived from this training data, to show that they can never occur in the model
either. This is only possible if the model is interpretable.

Second, even in industries where regulations don't apply, the regulations set a
standard for interpretability. They formalize and extend the question of
whether the model builder trusts the model to behave as desired. In some cases
they also emphasize the possibility of discrimination, which is something all
data scientists should bear in mind. A model that perfectly captures the real
world with 100% accuracy might seem desirable, but training data often embeds
society's biases. It may be unethical, if not illegal, to deploy a model that
captures and recapitulates these biases. Interpretability allows you to reason
about whether your model embeds biases before you go ahead and apply it
at scale.

[[explanations]]
### Explaining Decisions

Local interpretability -- the ability to explain individual decisions -- opens
up new analyses and features, and even new products. The ability to answer the
question "Why has the model made this decision?" is a superpower that raises
the possibility of taking an action to _change_ the model's decision. Let's
consider some examples of what you can do with that capability.

[[figure_local_actions]]
.Local interpretability means you can explain a model's predictions and even suggest actions.
image::figures/2-08.png[]

A model of customer churn tells you how likely a customer is to leave. A
_locally interpretable_ model -- that is, one in which you can explain a
particular prediction -- offers an answer to the question of _why_ this
customer is going to leave. This allows you to understand your customer's needs
and your product's limitations. It even raises the possibility of taking a
well-chosen action to reduce the probability of churn. This problem is the
focus of our prototype (<<prototype>>).

A model that predicts hardware failure in an engine or on a server is of course
extremely useful. You can send an engineer out to inspect an appliance that is
predicted to fail. But if the model is locally interpretable, then you can not
only warn that a problem exists: you can potentially solve the problem, either
remotely or by giving the engineer the reason, saving them time in the field.

A model that predicts loan repayment (or credit rating) is not only useful to
the lender, it is of enormous interest to the borrower. But showing borrowers
a credit rating number on its own is of limited use if they want to know what
they need to do to improve it. The consumer app Credit Karma
footnote:[http://creditkarma.com/] allows its users to figure this out for
themselves using a brute force method similar to the new algorithm that we use
in this report's prototype (see <<perturbation>>).

Interpretable models also tend to be more user-friendly. For example, the APGAR
score used at childbirth gives an integer score out of 10. The higher the
number, the healthier the newborn baby. The score is comprised of three
numbers, measured by eye and combined by mental calculation.
This heuristic is not machine learning, but it is algorithmic decision-making.
The simplicity of the APGAR score means that, in a fast-moving environment, the
obstetrician or midwife trusts its outputs and can reason about the problem
with the inputs in their head: the ultimate in usability. As we discuss in
<<downside>>, this simplicity comes at a cost: the model is less accurate than
a huge neural network would be. But it can often be worth trading a little
accuracy for interpretability, even in contexts less extreme than hospitals.

[[figure_apgar]]
.The APGAR score, used in evaluating the health of infants, shows how a simple model can inspire confidence because its operations are understandable.
image::figures/2-09.png[]

=== Improving the Model

An uninterpretable model suffers from the performance and regulatory risks
discussed earlier (see <<trust>>, and <<regulations>>), and closes the door on
products that take advantage of explanations (<<explanations>>). It's also much
harder to improve.

Debugging or incrementally improving an uninterpretable black-box model is
often a matter of trial and error. Your only option is to run through a list of
ideas and conduct experiments to see if they improve things. If the model is
interpretable, however, you can easily spot glaring problems or construct a
kind of theory about how it works. The problems can be fixed, and the theory
narrows down the possibilities for improvements. This means experiments are
driven by hypotheses rather than trial and error, which makes improvements
quicker.footnote:[This difficulty continues to plague deep learning. The models
are hard to interpret, which means the field lacks theory, and thus
improvements are made through a mixture of trial and error and intuition. See
<<long_term>>.]

A striking example of debugging is given in the paper introducing Local
Interpretable Model-agnostic Explanations (LIME),footnote:[Ribeiro, Singh, and
Guestrin (2016), link:https://arxiv.org/abs/1602.04938["'Why Should I Trust
You?': Explaining the Predictions of Any Classifier."]] the black-box
interpretability technique we use in this report's prototype (<<prototype>>).
In that paper, the authors describe a convolutional neural network image
classification model able to distinguish between images of wolves and Husky
dogs with high accuracy. LIME's ability to "explain" individual classifications
makes it obvious that the classifier has incorrectly learned to identify not
wolves and Husky dogs, but snow in the background of the image, which was more
common in the training images of wolves.

[[figure_wolf_example]]
.An explanation or interpretation of a model can reveal major problems, such as in this image classifier, which was trained to distinguish between wolves and Husky dogs but is using the snow in the background to tell the difference. Figure and example from the LIME paper (https://arxiv.org/abs/1602.04938).
image::figures/2-10.png[]

[[downside]]
=== Accuracy and Interpretability

So, why not simply use interpretable models? The problem is that there is a
fundamental tension between accuracy and interpretability. The more
interpretable a model is, generally speaking, the less accurate it is. That's
because interpretable models are simple, and simple models lack the flexibility
to capture complex ideas. Meanwhile, the most accurate machine learning models
are the least interpretable.

This report is about exciting recent developments that resolve this tension. In
the last few years, "white-box" models have been developed that are
interpretable, but also sacrifice minimal accuracy. Separately, model-agnostic
approaches that provide tools to peer inside accurate but previously
uninterpretable "black-box" models have been devised. The following chapters
discuss and illustrate these developments.

[[figure_interpretability_accuracy]]
.Choosing a model often involves a trade-off between interpretability and accuracy. This report is about breaking out of this trade-off.
image::figures/2-16.png[]
