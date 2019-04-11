# Model interpretability: 2016 elections

## Overview

The 2016 presidential election has been analyzed extensively, maybe even obsessively.  But I thought I'd have a look at it anyway.  The data set used was county-level election results, combined with census data.  

One general goal was to maintain model interpretability, as opposed to finding the most accurate possible model.  Interpretability is generally a soft concept, and there is often a trade-off between interpretability and predictive accuracy.

## Data

I am not including the raw data on GitHub.  The election results can be found at 

https://github.com/tonmcg/US_County_Level_Election_Results_08-16

while the census data is from

https://www.kaggle.com/benhamner/2016-us-election

## US Counties

The data is by county.  County-level data is not very representative of the entire population.  For example, while Republicans won 84% of the counties, they lost the over-all popular vote.  There is a strong correlation between county population size and the probability of a Democratic victory in the county.  There are other strong correlations as well.

Counties do not have any particular significance to the electoral outcome - most electoral votes are awareded on a winner-take-all basis at the state level.

Given these facts, why analyze at the county level?  Laziness is one excuse - that's the form this data comes in.  But I also thought it might reveal some interesting trends and allow me to explore the concept of model interpretability.  Additionally, since the election outcome has been analyzed so thoroughly, I thought looking at the county level, which skews towards looking at the more rural and red parts of the US, might be interesting and a little different.

## Conclusions - what predicts electoral outcome in this data?

The top five predictors of electoral outcome are:

('RHI825214', 'White alone, not Hispanic or Latino, percent, 2014')

('EDU685213', "Bachelor's degree or higher, percent of persons age 25+, 2009-2013")

('HSG495213', 'Median value of owner-occupied housing units, 2009-2013')

('HSD410213', 'Households, 2009-2013')

('INC110213', 'Median household income, 2009-2013')

None of these are that surprising, given what has already been published about this election.  "Households" is the number of households in a county and correlates strongly with county population.  One interesting twist - while higher median income correlates with greater probability of a Republican victory, higher housing value correlates with a lower probability of Republican victory.


## Conclusions - what makes a model interpretable?

One quick answer would be that models with more degrees of freedom (i.e. more parameters) are less interpretable.  However, the concept of "degrees of freedom" becomes harder to nail down when regularization is in the picture (see e.g. https://web.stanford.edu/~hastie/Papers/df_paper_LJrev6.pdf ).  Here, though, I stick pretty much to the concept that lower degrees of freedom (or, more accurately, lower number of parameters, since there is some regularization) gives a more interpretable model.

I found simple logistic regression models to be interpretable.  As described in the slides, logistic regression with the best five predictors did nearly as well as a much more complicated LR model picked out with the RFECV method.  Adding three interaction features gave further improvement.

Decision trees gave poorer performance than logistic regression with the same number of parameters.

It is true that more complicated models (e.g. random forest) do not completely defy interpretation - one can look at feature importance, for example.  But even if feature importance sheds some light, random forests (or other models with very many parameters such as neural nets) remain relatively opaque.

## Conclusions - clustering of predictors

I clustered the predictors, using the absolute value of the correlation (either Pearson or Spearman) as a similarity measure.  This gives clusters which make some sense.  For example, the strongest set of correlations corresponds to features that scale with county population.  

Using these clusters to pick out features did not perform better than just picking out the best features with Recursive Feature Elimination (RFE).  However, it does provide some insight into the relationships between the features and could be helpful in other cases.

## Further work

It would make sense to repeat this analysis using linear regression as opposed to logistic regression.  I would not expect much difference in feature importance, for example, but if there were significant differences, it would be interesting to understand why.







