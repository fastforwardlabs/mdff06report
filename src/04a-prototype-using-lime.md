## Prototype

In order to bring interpretability to life, we chose to focus our prototype on
LIME's ability to explain individual algorithmic decisions.

Alongside the rate at which a business acquires customers, the rate at which it
loses them is perhaps the most important measure of its performance. Churn is
well defined for an individual customer in a subscription business. This makes
it possible to collect training data that can be used to train a model to
predict whether an individual customer will churn. Our prototype uses LIME to
explain such a model.

If the relationship between the attributes of a customer and their churning is
causal, and the model that predicts churn from these attributes is accurate,
LIME raises an interesting possibility. If you can explain the model's
prediction for a customer, you can learn _why_ the customer is going to churn,
and perhaps even intervene to prevent it from happening.

Establishing a causal relationship can be tricky, and not all attributes can be
changed. In such cases it may not make sense or be possible to intervene. For
example, if a model predicts a customer is going to churn because they are over
a certain age, there's not much you can do about it. But even if it's not
possible to intervene, the ability to group customers according to the
attributes that are most concerning is a powerful way to introspect a business.
It may bring to light groups of customers you want to focus on, or weaknesses
in the product.

Using the churn model in this way depends on it being interpretable, but a
complicated business with thousands of customers, each of whom it knows a lot
about, may need a model using techniques such as uninterpretable random forests
or neural networks.

This is a job for model-agnostic interpretability. By applying LIME to an
arbitrarily complicated model, we can have it both ways: an accurate model
describing an intrinsically complicated dataset, that is also interpretable.

### Customer Churn

We used a public dataset of 7,043 cable customers, around 25% of whom
churned.^[[https://www.ibm.com/communities/analytics/watson-analytics-blog/predictive-insights-in-the-telco-customer-churn-data-set/](https://www.ibm.com/communities/analytics/watson-analytics-blog/predictive-insights-in-the-telco-customer-churn-data-set/)]
There are 20 features for each customer, which are a mixture of intrinsic
attributes of the person or home (gender, family size, etc.) and quantities
that describe their service or activity (payment method, monthly charge, etc.).

We used `scikit-learn` to build an ensemble voting classifier that incorporated
a linear model, a random forest, and a simple neural network. The model has an
accuracy of around 80% and is completely uninterpretable.

### Applying LIME

As [described above](#lime) in Chapter 3, LIME explains the classification of a particular
example by perturbing its features and running these perturbed variations
through the classifier. This allows LIME to probe the behavior of the
classifier in the vicinity of the example, and thus to build up a picture of
exactly how important each feature is to this example's classification.

Before it can do this, LIME needs to see a representative sample of training
data to build an "explainer" object. It uses the properties of this sample to
figure out the details of the perturbation strategy it will eventually use to
explain an individual classification. This process is a little fiddly in the
reference implementation that we
used,^[[https://github.com/marcotcr/lime](https://github.com/marcotcr/lime)] because it requires some
bookkeeping information about categorical features. But once the explainer has
been instantiated it can be saved for future use, alongside the model.

The explainer then requires two things to explain an individual classification:
the features of the example to be explained, and the classifier (in the form of
a function that takes the features as input and returns a probability). 

This yields an "explanation" for a given example and class, which is just a
list of weights or importances for all or a subset of the features. These are a
measure of the sensitivity of the classifier to each feature, in the local
region of the particular example. A large positive number means that the
particular feature contributes a lot toward the example's current
classification. A large negative number, on the other hand, means that a
feature's value implies that the example belongs in a different class.
Numbers close to zero indicate that a feature is unimportant.

This code will give us a list of `(feature, importance)` tuples: 


::: info

    from lime.lime_tabular import LimeTabularExplainer
    explainer = LimeTabularExplainer(training_data=X,
                                 training_labels=y,
                                 feature_names=feature_names,
                                 class_names=class_names)
    # clf is the churn classifier we want to explain
    e = explainer.explain_instance(customer, clf.predict_proba)
    print(e.as_list())

:::

For our use case, we are interested in the features with the largest positive
importances, which tell us which features are most responsible for the model
thinking the customer will churn.

::: info

##### *Computational resources*

In order to comprehensively probe the area around an example, LIME needs to
perturb every feature, and then build a linear model with the same features.
This means the time it takes to construct an explanation is most sensitive to
the number of features in the data.^[Mathematically, its complexity
is O(F^3^ + P F^2^ + P O(model)), where F is the number of features in the
data, P is the number of perturbations LIME makes, and O(model) is the
complexity of the model.] In our tests LIME explained a classification of our
model, which had 20 features, in around 0.1s on a commodity PC. This allowed
for a responsive user interface. We also applied our prototype to a
proprietary churn dataset with 100 features. That increased the time required
for an explanation to 1s on the same hardware. We could have increased the
power of the hardware or reduced the number of perturbations below the
standard 5,000 to speed this up if necessary.

:::

We wrapped our dataset, `scikit-learn` classifier, and LIME explainer in a
standard Python Flask web API. This allows frontend applications to get
customer data, the corresponding churn probabilities, and the LIME explanations
for those probabilities.
