## notes: Collaborative Filtering for Implicit Feedback Datasets, ICDM 2008

Notes for [Collaborative Filtering for Implicit Feedback Datasets](http://yifanhu.net/PUB/cf.pdf), Hu, Koren and Volinsky, ICDM 2008

### Background and related work
#### Collaborative Filtering (CF)
* CF methods suffer from cold start. Tapestry 1992 was first CF system
* Neighborhood models: user-oriented Vs item-oriented methods
    * item-oriented are often more scalable and accurate (why? fewer items than users?)
    * item-oriented are often explainable (users discern items similar to they bought, not users similar to themselves)
* Implicit feedback is derived from user actions. Differences from explicit
    * no negative feedback
    * inherently noisy
    * numerical values express confidence not preference
    * evaluation requires appropriate care
    * missing data is inherently handled e.g. 0 views of show means no feedback
    * less clear how to bias correct and compute similarities between items. In their application, values are show-viewing frequencies (vs ratings for explicit feedback) and frequencies are very different scales. See [Deshpande & Karypis, 2004].
    
#### Latent Factor Models
* alternate approach to CF. Uses matrix factorization approach like SVD, pLSA, neural networks or LDA, to derive user and item representations aka "embeddings", and computes the user-item similarity as dot product. This paper focuses on SVD.
* usually includes some form of regularization on norm of embedding and stochastic gradient descent
as of writing, largest computation was on Netflix dataset 2007

### Model
* Two variables: rating $r$ and confidence in rating $c = 1+ \alpha r$. Higher $r$, Higher $c and $r=0$ implied confidence is low because no feedback does not necessarily imply dislike
* Parameters: $\alpha$ for confidence weighting and $\lambda$ for regularization
* Scalability: "[for implicit feedback model] optimization should focus on all pairs, not just observed pairs". Can be 10^9 pairs. Cannot do gradient descent. Propose  
* Generating recommendation: for every user, find the top-K items with highest dot-product of user-factor and item-factors

### Optimizer
* Alternate least squares: because when you fix either item or user factors, cost function becomes quadratic. 
* Implicit feedback: ALS is for dense matrix so $Y^tY$ is expensive to compute. In explicit feedback matrix is sparse.
* They applied algebraic rewrites to make ALS scalable. 
    * Fix: __exploiting structure of variables__... rewrite $Y^tC_uY$ as $Y^tY + Y^t(C_u-I)Y$, $C_u$ is sparse as it only has non-zero elements where $r_u$ is non-zero
    * Opinion: their gain is from rewriting algebra, but they explicityly created a knob to make the matrix sparser first. Without this knob, rewrite is not effective.

### Unresolved questions
* Notation: Should Equation (3) use $r$ and not $p$? Section 4 $r$ is defined as dot product, and $p$ is the binary indicator variable. Next page, $p$ is redefined as dot-product.   
* ALS Algorithm
    * How is Y initialized in __Let us assume item fctors are gathered in $Y_{nxf} matrix. Before looping through users, we compute fxf matrix Y^tY__
    
    
### Legend
* Opinion: my thoughts. Can be wrong/will be revised on better understanding.
* Italics: verbatim quote from paper
