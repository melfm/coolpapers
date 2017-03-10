## segDeepM: Exploiting Segmentation and Context in DNN for object detection


#### Keywords : Markov Random Fields (MRF), Enerygy minimization problem, potential functions.

http://www.cs.toronto.edu/~kyros/courses/2503/Handouts/mrf.pdf

### The segDeepM Model
- Defined as a Markov Random Field with random variables that reason about detection boxes, object segment and context.
- $p$ : rand var denoting location + scale of candidate bounding box
- $h$ : set of rand var for each class i.e. $ h = (h_1, h_2,..., h_C)^T$.
    - Each $h_C \in \{0,1,...,H(x)\}$ represents an index into the set of all candidate segments.
        - $h_c$ allows each candidate detection box to choose a segment for each class and score confidence.
    - $C = $ total number of classes of interest and $H(x)$ is the total number of segments in image $x$.

#### Main Idea
- Boost the confidence of boxes that are well aligned with a high scoring object region proposal for the target class
- Adjust its score based on the proximity and confidence of region proposals for other classes, i.e. context for the model

#### Energy of a configuration
\begin{equation}
    E(p, h;x) = \omega_{app}^T . \varnothing_{app}(p;x) + \omega_{seg}^T. \varnothing_{seg}(p,h;x) + \omega_{ctx}^T. \varnothing_{ctx}(p;x)
\end{equation}

- $\varnothing_{app}(x,p)$ -> candidate's appearance
- $\varnothing_{seg}(p,h;x)$ -> segmentation
- $\varnothing_{ctx}(p;x)$ -> contextual potential functions (features)

### Potential Functions
- Appearance: Each image in candidate box is warped to a fixed size 227 x 277 x3. Pass it thru CNN already trained in ImageNet.
For appearance feature $\varnothing_{app}(p,x)$ use the 4096-dimensional feature extracted from $fc7$ layer.

- Segmentation : Use grid features that aim to capture a loose geometric arrangement of the segment inside the candidate's box.
The weights for each feature will be learned allowing the model to adjust the importance of each feature's contribution to the joint energy.
Using the following features:
    #### SegmentGrid-In ->
    Attempt to place a box slightly bigger than the segment, localize it w.r.t the spatial distribution of pixels within each grid.
    #### Segment-Out - >
    Compute percentage of segment pixels outside the candidate box. The aim is to place boxes that are smaller compared to the 
    segments.
    #### BackgroundGrid-In ->
    Compute the percentage of pixels in each grid cell that are not part of the segment
    #### Background-Out ->
    Scalar feature measuring the % of segment's background outside the box.
    #### Overlap ->
    Measure the alginment of the candidate's box and the segment $S(h)$.
    #### SegmentClass ->
    Since we are dealing with so many segments, train the O2P rankers for each class which uses several region-aware features as input
    to the segmentation features.

- Context : CNN trained for the task of image classification where in most cases input image is larger than target object.
This means part of success may be due to learning contextual information and relationship between objects (e.g. sky for aeroplane, road for car)
But the appearance features used here are based on the candidate's box so missing useful information from the scene. So add additional feature that looks at a 
bigger scope.
    - Enlarge each input candidate box by a fixed percentage $\rho$ along horizontal and vertical direction.
    - For big boxes or close to boundary, clip the enlarged region to be fully inside the image.
    - Keep the labels the same, even if the box now contains other classes!
    - Warp the image to 227 x 227
    - While at it, give it a name -> $expanded$ CNN

### Inference
- Score each box $p$ :
\begin{equation}
    f_\omega(x,p) = max \underset{h}(\omega_{app}^T. \varnothing_{app}(x,p) + \omega_{ctx}^T. \varnothing_{ctx}(x,p) + \omega_{seg}^T .\varnothing_{seg}(x,p,h))
\end{equation}
    - The first two terms can be computed efficiently by matrix multiplication
    - Although there could be exponential number of candidates for $h$ we can greedily search each dimension of $h$ and find the best segment.

### Learning
- Given set of images with $N$ candidate boxes ${p_n}$ and their annotations ${y(x_n,p_n)}$, with a collection of segments
 for each image ${S(x_n, h_n)}$ and associated potentials ${\varnothing(x_n, h_n)}$ with $n = 1,...,N$ :
\begin{equation}
    min \underset{\omega} ||\omega||^2 + C \sum_{n=1}^N max(0,1 - y(x_n, p_n)f_\omega(x_n, p_n))
\end{equation}
    - $\omega$ is a vector of all weights
    - This learning problem is a latent SVM, where we treat the assignment variable $h$ as a latent variable for each training instance.
    - To optimize :
        - 1.label each positive example: for each $(x,p)$ with $y(x,p) =1$ compute $h* = arg \quad max_h \quad f_\omega(x,p)$ with current model parameter $\omega$
        - 2.update the weights: do hard-negative mining over a set of negative instances until reaching a certain memory limit.
        use stochastic gradient descent
    - Latent SVM is guaranteed to converge to a local minimum only
    - So need to pick a good initialization for positive examples
    - Use overlap features $\varnothing_{overlap}$ as the indicator and set each dimension $h*$ as  $h = arg \quad max_h \quad \varnothing_{overlap}(p,h)$ 
    - This encourages the method to pick segments that best overlap with the candidate's box.

### Iterative Bounding Box Prediction
- Typical postprocessing step, object detection approaches perform bounding box prediction on their final candidate set.
    - Results in a few percent improvement in accuracy since the approach is able to make an informative re-localization based on complex features.
- Take this one step further, and do bounding box prediction and scoring the model in an iterative fashion
    - Because better localization can lead to improved predictions.
- First extract the CNN features and regress to a corrected set of boxes.
- Re-extract the features on the new boxes and score the model
    - But only the features on the boxes which have changed by more than 20%


### Conclusion
- MRF method that scores appearance as well as context for each detection
- Allows each candidate box to select a segment and score the agreement between them
- Proposed a sequential localization scheme, iterate between scoring and re-position the box
- Achieved boost over R-CNN, 4.1% on PASCAL VOC 2010 with 7-layer and 4.3% with 16 layers.
















