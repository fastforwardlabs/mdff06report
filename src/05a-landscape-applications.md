## Landscape

In this chapter we discuss specific real-world applications of
interpretability, based on interviews with working data scientists. We also
assess the offerings of vendors who aim to help data scientists and others
build interpretable models.

### Interviews

In our interviews and discussions with data organizations where
interpretability is a concern, we found that most chose white-box models in
order to maintain interpretability. These companies are so concerned with
knowing _how_ a model produced a given result that they are willing to
potentially trade off the accuracy of the result.

![FIGURE 5.1 Currently, companies concerned with interpretability trade accuracy for higher interpretability. Technologies like LIME could change that.](figures/5-03.png)

Some companies using black-box models were unwilling to provide details about
interpretability, or the applications of their models. We suspect that their
unwillingness to discuss may be rooted in regulatory concerns. Specifically, at
least until there are regulatory rulings to bring certainty, it remains unclear
whether a black box-compatible tool such as LIME is sufficient to meet
regulations requiring explanation of model behavior.

#### Recommendation Engines

Recommendations are a straightforward, relatively low-risk, user-facing
application of model interpretation. In the case of product recommendations,
users may be curious _why_ a certain item was recommended. Have they purchased
similar products in the past? Did people with similar interests purchase that
product? Do the recommended products share certain features in common? Are the
products complementary? Or is there some other reason? This explanation builds
trust with the users (especially if the recommendations are good!) by showing
them what's happening behind the scenes.

![FIGURE 5.2 Model interpretation can be used to explain product recommendations.](figures/5-01.png)

#### Credit Scores

Customer credit evaluation uses interpretable models extensively. When
evaluating a customer's credit score, and particularly when _denying_ a
customer credit, it is necessary to explain the basis for the credit decision.
Traditionally this is done with simple models that are inherently
interpretable. Technologies like LIME permit the use of more complex and
potentially more accurate models while preserving the ability to explain the
reasons for denial or assigning a particular score. The ethical considerations
associated with these kinds of decisions are discussed in <<ethics>>.

#### Customer Churn

As we demonstrate with our prototype, churn modeling is another clear case for
interpretability. Knowing when a user is likely to defect is helpful on a
number of levels. It's useful for predicting revenue streams, testing
effectiveness of promotions or marketing campaigns, and evaluating customer
service efficacy. Interpretation of churn models compounds their utility. For
example, as shown in our prototype (<<prototype>>), interpretation of churn
data can explain _why_ a given customer or set of customers are likely to
churn. This information offers customer service personnel the ability to retain
more customers by helping them identify those who may be considering taking
their business elsewhere and offering insights into the reasons driving that
prediction. Armed with this knowledge, the representative can offer a customer
a promotion or better-suited product and improve the chance of their staying.

#### Fraud Detection

Predictive models can help identify fraudulent activities such as credit or
bank transactions, or insurance claims. Flagging these transactions creates a
high-risk list for risk management personnel (or criminal activity
investigators, including police) to investigate further. Models that provide
deeper understanding than a simple fraud flag or likelihood indicator and show
investigators the reasons a transaction was flagged can lead to improvements in
resource allocation and greater effectiveness in catching fraudsters. This
additional context can help investigators to dismiss some flagged transactions
as appropriate, quickly eliminating false positives and enabling them to focus
investigative resources elsewhere. It may also help them to plan
investigations, providing hints on where to look for evidence.

#### Anomaly Detection

Predictive models can be used to predict failures in a variety of systems,
including computer systems and networks, or even mechanical systems or
industrial plants. Airlines have used such models to schedule maintenance on
airplane engines that are predicted to have trouble. This is helpful, but
_interpretable_ models can identify the _reason_ a system is likely to
experience a failure and suggest targeted interventions to remedy, mitigate, or
entirely prevent the problem. Fed back into the system, this understanding can
suggest improvements to the design of more robust new systems.

![FIGURE 5.3 Interpretation can be used to identify possible causes of a prediction of engine failure.](figures/5-02.png)

#### Healthcare

Models that support diagnosis or treatment must be interpretable in order to be
safe. The danger of subtle problems with training data is particularly acute
because ethical constraints make it difficult to modify or randomize care in
order to collect unbiased data. The requirement to "do no harm" can only be
fulfilled with certainty if the model is understood. And as in the case of
churn analysis, good models whose decisions can be explained offer hints
toward (or even outright instructions for) the best next steps for a given
case.
