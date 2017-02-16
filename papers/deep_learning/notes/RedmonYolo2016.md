# You Only Look Once object recognition

- Detects over 9000 object categories
- Using PASCAL VOC and COCO
- 67 FPS
- Outperforms Faster R-CNN with ResNet and SSD
- Method to jointly train object detection and classification
- Harness the large amount of classification data to expand the scope of current detection
- Heirarchical view of object classification that allows combining distinct datasets
- Joint training alg that allows training detectors on both detetion and classification
##  Batch Normalization
- Improves converence while eliminating the need
for other forms or regularization
### High Resolution Classifier
- Typical state of the art detection method use classifier pre-trained on ImageNet
Starting with AlexNet most classifiers operate on input images smaller than 256 x 256
### Convolution with Anchor Boxes
- Predicts the coordinates of bounding boxes directly using fully connected layers on top
of the convolutional feature extractor
### Dimension Clusters
- Start with k-means to cluster the training set to generate prior
### Direct location prediction
- Instability coming from predicting x,y locations
- Instead of predicting offsets, predict location coordinates relative to the location of the
grid cell
- This bouds the ground truth to fall between 0 and 1
- Use a logistic activation to constrain the network's prediction
### Fine Grained Features
- Uses 13 x 13 feature map
- Faster R-CNN and SSD both run their proposal networks at various
feature maps in the network to get a range of resolutions
- The passthrough layer concatenates the higher resolution features
with low resolution features by stacking adjacent features into diff channels
instead of spatial locations similar to the identity mappings in ResNet
### Multi-scale Training
- Uses resolution 416 x 416
- Since the model only uses convolutional and pooling layers it can be 
resized on the fly
- Instead of fixing the input image size, change the network every few iterations.
- Every 10 batches the network randomly chooses a new image dimension size
- This helps the net to learn to predict well across a variety of input dimensions
### Speed
- Most detection frameworks rely on VGG-16 as the base feature extractor
- VGG-16 is a powerful accurate classification network but complex
- Yolo uses a custom network based on the Googlenet architecture
- This is faster than VGG-16 only using 8.52 billion operations for a forward pass
### Darknet-19
- Use mostly 3 x 3 filters and doubling number of channels after every pooling step
- Following the work on Network in Network (NIN) they use global average pooling to make
predictions as well as 1 x 1 filters to compress the feature representation between 3 x 3 convolutions
- Batch normalization used to stabilize training, speed up convergence and regularize the model
- Final model has 19 convolutional layers and 5 maxpooling layers
- Requires 5.58 billion operations to process an image and achieves 72.9% top-1 accuracy
### Training for classification
- Network is trained on the standard ImageNet 1000 class classification dataset for 160 epochs
using stohastic gradient descent with a starting $\alpha = 0.1$, polynomical rate decay and weight decay of $0.0005$ and
momentum using the Darknet neural network.
- Normal tricks of image augmentation tricks also used.
- After initial training on images at 224 x 224 then it is fine tuned at a larger size, 448.
- Then it is trained for only 10 epochs
### Training for detection
- Modify the network for detection by removing the last convolutional layer and instead adding on three 3 x 3
convolutional layers with 1024 filters each followed by a final 1 x 1 convolutional layer with the number of
outputs we need for detection.
- For VOC it predicts 5 boxes with 5 coordinates each and 20 classes per box so 125 filters.
- A passthrough layer from the final 3 x 3 x 512 layer to the second to last convolutional layer is added
so that the model can use fine grain features
- Network is trained for 160 epochs.
### Joint training classification and detection
- This method uses images labelled for detection to learn detection-specific info like bounding box coords
and *objectness* as well as how to classify common objects.
- It uses images with only class labels to expand the number of categories it can detect.
- During training images from both detection and classification datasets are mixed.
- So when the network sees an image labelled for detection we can backpropagate based on the full
YOLOv2 loss function.
- When it sees a classification image we only backpropagate loss from the classification-specific
parts of the architecture.
- This approach presents challenges, detection datasets have only common objects and general labels
like "dog" or "boat". Classification datasets have a much wider and deeper range of labels.
- ImageNet has more than a hundred breeds of dogs. To train on both datasets one needs a coherent way
to merge these labels.
- Most approaches to classification use a softmax layer across all the possible categories to compute the final probability
distribution. Using a softmax assumes the classes are mutually exclusive.
- But you wouldn't want to combine ImageNet and COCO using this model because the classes of say dog breed and dog
are not mutually exclusive.
- So instead use a multi-label model to combine the datasets which does not assume mutual exclusion.

### Heirarchical classification
- ImageNet labels are pulled from WordNet, a language database that structures concepts and how they relate
- Build a heirarchical tree from the concepts in ImageNet
- To build the tree examine the visual nouns in ImageNet and look at their paths through the WordNet graph
to the root node in this case "physical object".
- The final result is WordTree a heirarchical model of visual concepts
- To perform classification with WordTree we predict conditional probabilities at every node for the probability
of each hyponym of that synset given that synset.
- Instead of assuming every image has an object, YOLOv2's objectness predictor is used to give value of
Pr(physical object). The detector predicts a bounding box and the tree of probabilities.
### Dataset combination with Wordtree
- Can use WordTree to combine multiple datasets together in a sensible fashion.
- Simply map the categories in the datasets to synsets in the tree.


