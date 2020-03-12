## Future

Model-agnostic interpretability techniques such as LIME are a breakthrough that
begins to make the goals discussed in <<why>> practical. But the need for
interpretable machine learning is only going to grow over the coming years.


![FIGURE 7.1 Interpretability will become even more important as machine learning is applied in situations where failure can have disastrous consequences.](figures/7-01.png)

Two drivers of this growth are particularly important. The first is that
machine learning is being applied more broadly. This technology, which
sometimes seems like "magic," will increasingly be applied in situations where
failures can have disastrous consequences, such as systemic damage to the
economy or even loss of life (see <<safety>>).

The second driver is that the most advanced and accurate approaches to machine
learning are also the least interpretable. This is an inevitable consequence of
the interpretability/accuracy trade-off discussed in <<downside>>. Competitive
pressure will require more and more businesses to use these accurate black-box
models, which will result in the more widespread use of model-agnostic
interpretability techniques.

### Near Future

In the next one to two years, we expect to see approaches like LIME in
increasingly wide use. In the short term, our prototype could be applied to
essentially any binary classifier of tabular data, and become a powerful
internal tool. Indeed, the basic idea may become a commodity vendor machine
learning technology (see <<vendors>>).

With a little extra work, LIME's output could be used to generate natural
language explanations that can be shown to non-technical end users. For
example, suppose a product recommender were able to give an explanation of its
recommendations that was both accessible and accurate. Then, a user
dissatisfied with the recommendations could correct the model's
misunderstanding, perhaps by marking a piece of content as unliked.

![FIGURE 7.2 Interpretability can help explain algorithmic decisions to users.](figures/7-02.png)

As discussed in <<safety>>, our comfort with machine learning is dependent on
the extent to which we feel as though we understand how it works. Natural
language explanations will be invaluable in gaining support for machine
learning in wider society, amongst those who might find the raw output of
something like LIME hard to understand.

Regulated industries like finance are among the most competitive, so the
potential upside of deploying the best models in such industries is huge. But
as we have seen, the "best" (most accurate) models are often the least
interpretable. Model-agnostic interpretation promises to open a floodgate that
allows the most accurate models to be used in situations where previously it
had not been possible because of regulatory constraints.

![FIGURE 7.3Model-agnostic interpretability can provide a sanity check for models created through automatic machine learning.](figures/7-04.png)

Model-agnostic interpretability will also drive the increasing popularity of
_automatic machine learning_. Automatic machine learning is when a parent
algorithm configures and trains a model, with very little human involvement.
This possibility is rather alarming to many experts, and precludes the
possibility of offering explanations to users or regulators. This concern is
alleviated if you are able to sanity check the model's behavior using a system
such as LIME, or if the automated process is constrained to use interpretable
models such as those discussed in <<whitebox>>.

### Longer Term

In the next three to five years, we expect three concrete developments. The
first is the _adversarial_ application of model-agnostic interpretability --
that is, use of LIME by someone other than the owner of the model. Regulators
will use these techniques to demonstrate discrimination in a model.
Explanations derived from LIME-like techniques will be used by courts to assign
blame when a model fails.

![FIGURE 7.4 Regulators will be able to use model-agnostic interpretability to inspect models.](figures/7-03.png)

Second, we expect current research into the formal computational verifiability
of neural networks to bear fruit. In this report, we have focused on
interpretability from a human point of view. It's easier for a human to be
satisfied that the behavior of an algorithm will be correct if they understand
it. But in some safety-critical situations, human understanding is only
tangentially related to _verifiability_, the construction of formal proofs that
an algorithm will always behave in a certain way. Humans may never be able to
reason confidently about the internals of neural networks, but work has begun
on using fundamental ideas from computer science and logic to allow computers
to answer with certainty questions such as "If this autopilot detects a plane
on a collision course, will it take evasive action?" This is computationally
and theoretically challenging work, and it has a way to go before it is
practical, and further still to satisfy regulators,^[For an
introduction to this field, we recommend [*"Reluplex: An Efficient SMT Solver for
Verifying Deep Neural Networks"*](https://arxiv.org/abs/1702.01135) and this informal two-part article: *"Proving that safety-critical neural networks do what they're supposed to: where we are, where we're going"* [Part 1](http://bit.ly/2sDpoD1), [Part 2](http://bit.ly/2tOwXGW).] but it will be integral to the
wide-scale deployment of neural networks in safety-critical situations where
verifiability is a requirement.

Finally, interpretability techniques will also fuel development of machine
learning theory. Theory is not an academic luxury. Without it, machine learning
is trial and error. The very best deep learning models are not only
uninterpretable individually, but we have very little theory about why or how
they work as a class of algorithms. Interpretability has a role to play in
making deep learning research less a case of trial and error and more a case of
principled, hypothesis-driven experimentation. This is great news for machine
learning and artificial intelligence.

::: info
##### *The upside of uninterpretability*

Truly uninterpretable models are black boxes, which leak as little information
as possible to the end user. This can be a feature, rather than a bug. Opacity
is useful in publicly accessible machine learning APIs. A linear model is fully
specified by a number (or coefficient) for each of its input features. If a
model is known or suspected to be linear, and can be asked to make predictions
quickly and cheaply, then it can be _stolen_ with a finite and perhaps very
small number of API calls.^[See e.g., [https://arxiv.org/abs/1609.02943(https://arxiv.org/abs/1609.02943).]
And a model that can be stolen can also be _gamed_ -- i.e., the input can be
adjusted to get the desired output. The more uninterpretable the model, the
less vulnerable it is to theft and gaming.

:::
