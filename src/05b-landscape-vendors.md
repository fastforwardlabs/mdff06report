[[vendors]]
=== Data Science Platforms

White-box models are interpretable by design. At the time of writing of this
report, there were no vendors focused on providing interpretability solutions
for black-box models. Interpretability-as-a-service is not available (yet), but
it is a capability within some data science platforms.

Data science platforms seek to provide one central place for data science work
within an organization. They promise to increase collaboration and
cross-pollination within and across teams, as well as building transparency
into data science work. They aim to offer infrastructure and software solutions
to common bottlenecks in the data science workflow that quickly deliver
reliable, reproducible, and replicable results to the business. Some data
science platforms aspire to enable non-data scientists (e.g., software
engineers, business analysts, executives) to do data science work.

Platforms should therefore be evaluated on their solutions to common
bottlenecks in the entire data science workflow. In addition to
interpretability, these include:

 - Easy access to scalable computing resources (e.g., CPUs, GPUs)
 - Management and customization of the compute environment, software
   distributions, libraries
 - Access to open source tools and libraries (e.g., Python, R, `scikit-learn`)
 - Data exploration and experimentation
 - Code, model, and data versioning
 - Path from prototype to model deployment
 - Monitoring tools for deployed solutions
 - Collaboration, communication, and discovery tools

Domino Data Lab's service, for example, does not include off-the-shelf
interpretability solutions, but it is nevertheless a high-quality data science
platform.footnote:[For a recent complete list of data science platforms, see
http://www.kdnuggets.com/2017/02/gartner-2017-mq-data-science-platforms-gainers-losers.html.]

Still, as we emphasize throughout this report, interpretability is an important
consideration. As trained data scientists become less involved in model
selection, training, and deployment, we need tools to enable us to trust models
trained automatically or by non-experts. The following offerings stand out for
their interpretability solutions.

==== H2O.ai

H2O.ai (Mountain View, CA; founded 2011; Series B Nov 2015) provides an open
source platform for data science, including deep learning, with an enterprise
product offering. The developers summarized their knowledge and offering with
regard to interpretability in "Ideas on Interpreting Machine Learning," a white
paper published by
O'Reilly.footnote:[https://www.oreilly.com/ideas/ideas-on-interpreting-machine-learning]
Their work includes a twist on LIME called k-LIME. k-LIME trains _k_ linear
models on the data, with _k_ chosen to maximize R^2^ across all linear models.
This approach uncovers regions in the data that can be modeled using simpler
linear, interpretable models, offering a solution that sits comfortably between
global and local interpretability.

Of all the platforms evaluated for this report, H2O's developers have thought
the most extensively about interpretability and how to best explain complex,
nonlinear relationships between inputs (i.e., features) and outputs (i.e.,
labels). In addition, they are actively working on novel solutions to aid data
exploration and feature engineering, including visualization tools to help
humans, who are adapted to perceive a three-dimensional world, understand
relationships in higher-dimensional spaces.

https://www.h2o.ai/

==== DataScience.com

DataScience.com (Culver City, CA; founded 2014; Series A Dec 2015) released its
data science platform in October 2016 and
Skater,footnote:[https://www.datascience.com/resources/tools/skater] a Python
library for model interpretation, in May 2017. Skater includes implementations
of partial dependence plots and LIME. The team at DataScience.com developed
their own in-house sampler for LIME with the aim of improving the efficiency of
the algorithm. The company provides robust solutions for both global and local
interpretability as part of its data science platform and services offering. We
welcome its decision to open-source Skater, a meaningful contribution to the
data science community.

https://www.datascience.com/

==== DataRobot

DataRobot (Boston, MA; founded 2012; Series C March 2017) provides a data
science platform with the ambition to automate the data science workflow, from
data exploration to model deployment, to enable data scientists and non-data
scientists alike to build predictive models. DataRobot provides tools to
estimate the maximal correlation between continuous features and target
variables, allowing us to measure the strength of linear and nonlinear
relationships between continuous variables. For continuous and categorical
target variables, DataRobot allows us to construct partial dependence plots.
Word clouds visualize the relative importance of individual words to the
decisions of a given algorithm. Finally, DataRobot includes implementations of
white-box algorithms, including the RuleFit
algorithm.footnote:[http://statweb.stanford.edu/~jhf/ftp/RuleFit.pdf]

A current limitation to DataRobot's interpretability solutions is that maximal
correlation, word clouds, and partial dependence plots cannot capture complex
relationships between sets of variables and their combined impact on the target
variable. Likewise, the white-box algorithm RuleFit may not always be the best
algorithmic choice for a given machine learning use case.

https://www.datarobot.com/

==== Bonsai

Unlike all the other companies covered in this section, Bonsai (Berkeley, CA;
founded 2014; Series A May 2017) does not offer a data science platform but
rather a platform to develop interpretable models to increase automation and
efficiency of dynamic industrial systems (e.g., robotics, warehouse operations,
smart factories) based on deep reinforcement learning.

Bonsai aims to build an interpretable solution, and to speed up the training of
models, by asking humans to guide the algorithm as it learns. Specifically,
humans need to identify the key subtasks and determine the best order in which
to perform these in order for the algorithm to achieve mastery; that is, humans
need to identify the best learning path.footnote:[See Bengio et al. (2009),
link:http://dl.acm.org/citation.cfm?id=1553380["Curriculum Learning."]]
According to the Bonsai developers, this approach allows the algorithm to train
faster by leveraging human knowledge to reduce the search space for the
solution. It also ensures that human concepts map onto machine-solvable tasks,
thereby facilitating an intuitive understanding of the capabilities of the
algorithm. In a sense, the Bonsai platform forces models to "think like us," to
use similar subtasks or concepts in problem solving -- a different but
intriguing approach to interpretability than that covered in this
report.footnote:[AlphaGo, an AI Go player, can not only beat humans at Go.
link:https://news.ycombinator.com/item?id=11259022[According to some players],
it seems to use fundamentally different concepts and strategies than humans.
The approach taken by Bonsai would nudge AlphaGo to "think" more like human
players.]

https://bons.ai/
