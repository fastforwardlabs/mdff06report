## Ethics and Regulations

We've already touched on some of the reasons why interpretability is essential
to ensure the application of machine learning is not dangerous, discriminatory,
or forbidden by regulations. In this chapter we'll discuss this in more detail.

![Regulations can require information about how a model works. If your model is uninterpretable you will be unable to comply.](figures/5-06.png)

### Discrimination

The issue of discrimination is intimately tied up with interpretability.
Protected classes have suffered (and continue to suffer) discrimination in
sensitive situations such as employment, lending, housing, and healthcare.
Decisions in such areas are increasingly made by algorithms. Legislation such
as the US Civil Rights Act therefore directly impacts machine learning (see
<<regulations-box>>). Complying with legislation is the least we can do:
ethical concerns should also constrain our use of machine learning. And of
course, it is often good business to build a product that serves as many people
as possible. A product that depends on a discriminatory model suffers in this
regard.

::: info
##### *The legal landscape*

The legal landscape is complex and fast-moving. Here are a few of the relevant
regulations:

 - Civil Rights Acts of 1964 and 1991
 - Americans with Disabilities Act
 - Genetic Information Nondiscrimination Act
 - Equal Credit Opportunity Act
 - Fair Credit Reporting Act
 - Fair Housing Act
 - Federal Reserve SR 11-7 (Guidance on Model Risk Management)
 - European Union General Data Protection Regulation, Article 22 (see
   "<<gdpr>>")

:::

To make this discussion a little more concrete, let's consider a specific
context in the United States. The Equal Employment Opportunity Commission
(EEOC), which derives much of its authority from the Civil Rights Act, defines
_disparate impact_ as a "selection rate [for employment] of a protected class
that is less than 4/5 the rate for the group with the highest rate." An
organization can claim that a procedure is "business-related" if it correlates
with improved performance at p < 0.05. This might be true if, for example, a
graduate degree is required, since members of certain protected classes are
less likely to hold such degrees. The EEOC must then argue that there is a less
disparately impactful way to achieve the same goal (e.g., the employer could
use an aptitude test rather than require a PhD).

The previous paragraph raises many specific questions. What
is the selection rate as a function of protected class membership? Is there a
correlation between class membership and job performance? Does there exist a
quantifiably less disparately impactful alternative to the current procedure? A
conscientious model builder should be able to answer these questions, but that
is difficult or impossible if the model is uninterpretable.

As another example, consider Section 609(f)(1) of the Fair Credit Reporting
Act. This requires that consumers be provided "all of the key factors that adversely
affected the credit score of the consumer in the model used, the total number
of which shall not exceed 4." Again, it may be impossible to fulfill this
requirement if the model is uninterpretable.

![The Fair Credit Reporting Act requires the consumer be informed about key factors.](figures/5-05.png)[style="right"]

When discrimination is the result of an algorithmic decision, it can be
difficult for those affected by the decision, the regulator, or the
organization that deployed the algorithm to determine the reason (or even to
confirm whether discrimination took place). Techniques that ensure
interpretability, such as those discussed in <<how>>, are essential to ensure
we build models that do not discriminate and that therefore comply with the
law, are ethical, and are good for our businesses.

::: info

##### *Resources on algorithmic discrimination*

 - Barocas and Selbst (2016), "Big Data’s Disparate
   Impact."^[[https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2477899](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2477899)] This
   clearly written and well-organized non-technical paper focuses on the
   practical impact of discriminatory algorithms. Part A is an invaluable list
   of all the ways in which an algorithm can be discriminatory. Parts B and C
   pay particular attention to the status and future of US law.
 - O'Neil (2016), _Weapons of Math Destruction: How Big Data Increases Inequality and
   Threatens Democracy_ (Crown Random
   House)^[https://weaponsofmathdestructionbook.com/](https://weaponsofmathdestructionbook.com/)]. O'Neil's book is
   a wide-ranging polemic that we highly recommend to anyone who works with
   data scientists, or is one. It considers the issues touched upon in this
   chapter throughout.

:::

### Safety

Algorithms must be audited and understood before they are deployed in contexts
where injury or death is a risk, including healthcare (as discussed in
<<trust>>, and <<healthcare>>) and driverless vehicles. Deep understanding of
an algorithm may be necessary not only to reduce its physical danger, but also
to reduce legal risk to the owner of the algorithm.

There is also a social obligation (and market incentive) to explain these
high-stakes algorithms to society. The 2016 IEEE white paper "Ethically Aligned
Design"^[[http://standards.ieee.org/develop/indconn/ec/ead_v1.pdf](http://standards.ieee.org/develop/indconn/ec/ead_v1.pdf)] puts
it well: "For disruptive technologies, such as driverless cars, a certain level
of transparency to wider society is needed in order to build public confidence
in the technology."

The financial system is a special case. While there is not an immediate
physical danger, the potential consequences of badly behaved algorithms are
potentially grave and global. With this in mind, the financial services
industry in the United States is bound by SR 11-7, Guidance on Model Risk
Management,^[[https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm)]
which among other things requires that model behavior be explained.

##### *GDPR Article 22 and the right to an explanation*

The European Union's General Data Protection Regulation will apply in the EU
from May 2018.^[[http://www.privacy-regulation.eu/en/22.htm](http://www.privacy-regulation.eu/en/22.htm)]
There has been much debate about the intentions and practical
consequences of this wide-ranging regulation. A 2016 paper created
considerable excitement and concern by arguing that Article 22 "creates a
'right to explanation,' whereby a user can ask for an explanation of an
algorithmic decision that was made about them."^[[https://arxiv.org/abs/1606.08813](https://arxiv.org/abs/1606.08813)]
Without the careful application of
approaches such as LIME to craft user-friendly explanations in plain words,
such a regulation would seem to make it illegal to apply random forests and
neural networks to data concerning the 500 million citizens of the EU. A
response with the unambiguous title "Why a Right to Explanation of
Automated Decision-Making Does Not Exist in the General Data Protection
Regulation"^[[https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2903469](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2903469)]
rejected this interpretation, conceding only that the regulations
create a right to an "explanation of system functionality." This view is
consistent with that of a global accounting firm we talked to while writing
this report, but there is lack of consensus.^[The UK's decision to leave
the EU further complicates things in that jurisdiction. See paragraphs 43-46 of
the UK House of Commons Science and Technology Committee report ["Robotics
and Artificial Intelligence"](http://bit.ly/2urMtJr) for the current UK government position.]
Hopefully things will become clearer when the regulation comes into force;
in the meantime, for further information we recommend the clear, practical
article "How to Comply with GDPR Article 22" by Reuben
Binns.^[[http://www.reubenbinns.com/blog/how-to-comply-with-gdpr-article-22-automated-credit-decisions/](http://www.reubenbinns.com/blog/how-to-comply-with-gdpr-article-22-automated-credit-decisions/)]

:::

### Negligence and Codes of Conduct

Professions like medicine and civil engineering have codes of conduct that are
either legally binding or entrenched norms. To not follow them is considered
negligent or incompetent. The relatively immature professions of software
engineering and data science lag a little in this area, but are catching up. We
think and hope that applying the techniques discussed in this report will
become a baseline expectation for the competent, safe, and ethical application
of machine learning. Indeed, the IEEE and ACM have both recently proposed community standards
that address precisely the topic of this report.

The 2016 IEEE white paper "Ethically Aligned
Design"^[[http://standards.ieee.org/develop/indconn/ec/ead_v1.pdf](http://standards.ieee.org/develop/indconn/ec/ead_v1.pdf)] is
unambiguous in its assertion that ensuring transparency is essential to users,
engineers, regulators, the legal system, and society in general. To this end,
the organization has established a working group to define a formal
professional standard, "IEEE P70001: Transparency of Autonomous
Systems."^[[https://standards.ieee.org/develop/project/7001.html](https://standards.ieee.org/develop/project/7001.html)] This
standard may become a familiar Request for Proposal (RFP) requirement, like the
equivalent ISO standards on security and data protection.

The ACM's 2017 Statement on Algorithmic Transparency is non-binding but
similarly clear: "systems and institutions that use algorithmic decision-making
are encouraged to produce explanations regarding both the procedures followed
by the algorithm and the specific decisions that are made. This is particularly
important in public policy
contexts."^[[http://www.acm.org/binaries/content/assets/public-policy/2017_usacm_statement_algorithms.pdf](http://www.acm.org/binaries/content/assets/public-policy/2017_usacm_statement_algorithms.pdf)]
