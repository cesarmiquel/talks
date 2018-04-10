# Machine Learning

## Andrew Ng Talk - Artificial Intelligence is the New Electricity ([video](https://www.youtube.com/watch?v=21EiKfQYZXc))


Problems:

Supervised Learning:

 - email => is it spam?
 - english text -> output french transalation
 - give text -> read out the text aloud
 - ad + user -> will he click on an ad?
 - Anything a human can do with < 1 sec of thought can be now or soon automate AI (many counter-examples:
   false positives and false negatives)

Why did AI take off in the last 5 yeas?

 - In traditional ML the more data you had performance wouldn't improve
 - If you feed more data to a NN it perfomed better
 - The bigger the NN (large NN) the performance improved 

You need 2 things:

  - A lot of data
  - Large computers / fast (HPC High Performance Computers / GPUs)

How to build a defensible business?

 - Not the algorithms. They are mostly published free by the most important centers.
 - Data, data, data.
   - example: face recognition system. Academic: working in 1-15 Million images. Best of the best: 200 Million samples!!
   - some products are created solely to 'collect' data where revenue is not important!!
 - Other scarce resource: talented engineers.

Virtuous circle of AI:

 - Product -> gets users -> provide data -> improve product -> get more users -> collect more data -> etc.
 - Example: recognize good / bad lettuces started taking pictures (http://www.bluerivertechnology.com/
   - Computer vision sees every plant and determines appropriate treatment for each
   - Robotic nozzles automatically target unwanted weeds as the machine passes
   - See & Spray assesses the applied herbicide, makes adjustments, and learns as it goes
   - Drone-based phenotyping as part of a program with the Deptartment of Energy
   - https://www.youtube.com/watch?v=-YCa8RntsRE (What it does)
   - https://www.youtube.com/watch?v=1I6mSQK4aPI (Some prototype testing of our next gen See & Spray platform in cotton and lettuce. December 2016)

What will take off soon?

 - Speech recognition
 - Face recognition: very important in China. Mobile first/only society. Can do a lot of stuff with
   cellphone. Even give you finance. Important to know its you. Baidu has improved a lot face recognition.
 - Baidu HQ: face recognition to recognize employees.
 - A little more in the future: Radiology recognition.

## TensorFlow DevSummit 2018 (March 30, 2018) [website](https://www.tensorflow.org/dev-summit/)

[Livestream (YouTube)](https://www.youtube.com/watch?list=PLQY2H8rRoyvxjVx3zfw4vA4cvlKogyLNN)

Some examples:

- Finding new planets 'Kepler 90 System' 'Kepler 90i'
- Assess risk to cardiovascular risk looking at reting scan
- Air traffic controllers, ML predict trajectories to help reduce risk of collisions
- Monitor rain forests 

Tensor flow:

- Initial release ~ 1/1/2016
- tf.keras
- Distribution execution: Estimator
- https://cloud.google.com/tpu/ | https://github.com/tensorflow/tpu

### tf.data

- [kaggle](https://www.kaggle.com/datasets) to retrieve datasets in CSV/TSV format and now tf.data has support for csv.

### TF Eager execution

- TensorFlow is a graoh execution engine for ML.
- Why graphs?: 
  - automatic differntiation
  - deployment to a python-free architecture (phones, Raspi, etc)
  - Automatic distritbution to 100s of machines.

### Tensorflow.js

- [Neural Network Playground](https://playground.tensorflow.org) / [Github](https://github.com/tensorflow/playground)
- We can use sensors


### Training Performance: A user’s guide to converge faster

- fp32 (IEEE floating point / 32 bits)
- By using fp16 (smaller floating point representation, 16 bits) we can improve performance by reducing memory bandwidth (less data being transfer around). Additionally you can have more layers. The weights can be stored in fp32 but the activations are stored in fp16.


## Lecture 1 | Machine Learning (Stanford)

Video: https://www.youtube.com/watch?v=UzxYlbK2c7E&list=PLA89DCFA6ADACE599

- Problema de separar dos voces
- Unsupervised Learning / ICA (Independent Component Analysis)
- `[W, s, v] = svd((repmat(sum(x.*x, 1), size(x,1), 1). *x) *x');`
- Source: Sam Rowels, Yair Wiss & Fero Simoncelli
- https://stackoverflow.com/questions/20414667/cocktail-party-algorithm-svd-implementation-in-one-line-of-code

# Nice review

### [The Great A.I. Awakening](https://www.nytimes.com/2016/12/14/magazine/the-great-ai-awakening.html)
How Google used artificial intelligence to transform Google Translate, one of its more popular services — 
and how machine learning is poised to reinvent computing itself.


## Interesting work (Papers)

### Building high-level features using large scale unsupervised learning ([PDF](https://arxiv.org/pdf/1112.6209))


[Abstract](https://arxiv.org/abs/1112.6209)
**a.k.a "The Cat paper" - 2012**


Le, Ranzato, Monga, Devin, Chen, Cirradi, Dean, Ng - 29/12/2011 - 12/7/2012 

We consider the problem of building high-level, class-specific feature detectors from only unlabeled data. For
example, is it possible to learn a face detector using only unlabeled images? To answer this, we train a 9-layered
locally connected sparse autoencoder with pooling and local contrast normalization on a large dataset of images
(the model has 1 billion connections, the dataset has 10 million 200x200 pixel images downloaded from the Internet).
We train this network using model parallelism and asynchronous SGD on a cluster with 1,000 machines (16,000 cores)
for three days. Contrary to what appears to be a widely-held intuition, our experimental results reveal that it is
possible to train a face detector without having to label images as containing a face or not. Control experiments
show that this feature detector is robust not only to translation but also to scaling and out-of-plane rotation. We
also find that the same network is sensitive to other high-level concepts such as cat faces and human bodies.
Starting with these learned features, we trained our network to obtain 15.8% accuracy in recognizing 20,000 object
categories from ImageNet, a leap of 70% relative improvement over the previous state-of-the-art.

**Notes**

- In previous literature there were success in *low-level* features such as "edge" or "blob" detectors
- Training deep learning algorithms is time / computational intensive
- Normally researchers reduce dataset sizes and this undermines the learning of high-level features.
- They use 200x200 images (random YouTUbe frames) much larger than typical 32x32 images
- Model: depp autoencoder with pooling and local contrast normalization
- To support paralelism: 'local receptive fields'. Reduces communication costs between nodes. Parameters are
  distributed across machines.
- Asynchronous SGD is employed to support data parallelism. The model was trained in a distributed fashion
  on a cluster with 1,000 machines (16,000 cores) for three days.
- The work is influenced by (Hinton et al., 2006; Bengio et al., 2007; Ranzato et al., 2007; Lee et al., 2007).
  It is strongly influenced by the work of (Olshausen & Field, 1996) on sparse coding.
- Used **DistBelief** software framework to manage the communication between machines within a model replica (the
  precursor of TensorFlow)

In numbers:
 - 1000 machines (16 CPU cores each)
 - 1 billion traiable parameters
 - 3 days of training

### Large Scale Distributed Deep Networks ([PDF](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/40565.pdf))

Seems to be a follow up on 'cat paper' going into de details of *DistBelief* (precursor to TensorFlow)


### Large-scale Deep Unsupervised Learning using Graphics Processors ([PDF](http://robotics.stanford.edu/~ang/papers/icml09-LargeScaleUnsupervisedDeepLearningGPU.pdf))


### ImageNet Classification with Deep Convolutional Neural Networks ([PDF](http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf))

(2012)

We trained a large, deep convolutional neural network to classify the 1.2 million
high-resolution images in the ImageNet LSVRC-2010 contest into the 1000 different
classes. On the test data, we achieved top-1 and top-5 error rates of 37.5%
and 17.0% which is considerably better than the previous state-of-the-art. The
neural network, which has 60 million parameters and 650,000 neurons, consists
of five convolutional layers, some of which are followed by max-pooling layers,
and three fully-connected layers with a final 1000-way softmax. To make training
faster, we used non-saturating neurons and a very efficient GPU implementation
of the convolution operation. To reduce overfitting in the fully-connected
layers we employed a recently-developed regularization method called “dropout”
that proved to be very effective. We also entered a variant of this model in the
ILSVRC-2012 competition and achieved a winning top-5 test error rate of 15.3%,
compared to 26.2% achieved by the second-best entry.

### End to End Learning for Self-Driving Cars ([PDF](https://images.nvidia.com/content/tegra/automotive/images/2016/solutions/pdf/end-to-end-dl-using-px.pdf))

2016 (25/4)

We trained a convolutional neural network (CNN) to map raw pixels from a single
front-facing camera directly to steering commands. This end-to-end approach
proved surprisingly powerful. With minimum training data from humans the system
learns to drive in traffic on local roads with or without lane markings and on
highways. It also operates in areas with unclear visual guidance such as in parking
lots and on unpaved roads.

- While CNNs with learned features have been in commercial use for over twenty years [3], their
adoption has exploded in the last few years because of two recent developments. First, large, labeled
data sets such as the Large Scale Visual Recognition Challenge (ILSVRC) [4] have become available
for training and validation. Second, CNN learning algorithms have been implemented on the
massively parallel graphics processing units (GPUs) which tremendously accelerate learning and
inference.

## Referencias

### Machine Consciousness

- [Joscha Bach - From Computation to Consciousness](https://www.youtube.com/watch?v=da-9zPgxWBY) | http://cognitive-ai.com/
- [Hartmut Neven - Google's Quantum AI Lab](https://www.youtube.com/watch?v=6aqMhbdxbAM) | Quantum Neural Networks
- [Sir Roger Penrose - How can Consciousness Arise Within the Law of Physics?](https://www.youtube.com/watch?v=h_VeDKVG7e0)

#### Discussion
- [Machine Consciousness Discussion - Penrose, Bach & Neven (YouTube)](https://www.youtube.com/watch?v=LJLxDVigHQA)

## [Research @Google](https://research.google.com)


## To Read

- [How to easily Detect Objects with Deep Learning on Raspberry Pi](https://medium.com/nanonets/how-to-easily-detect-objects-with-deep-learning-on-raspberrypi-225f29635c74)
- [Cocktail party algorithm SVD implementation … in one line of code?](https://stackoverflow.com/questions/20414667/cocktail-party-algorithm-svd-implementation-in-one-line-of-code)
- [End to End Learning for Self-Driving Cars](https://images.nvidia.com/content/tegra/automotive/images/2016/solutions/pdf/end-to-end-dl-using-px.pdf)


## Resources

- [Tensorflow](https://www.tensorflow.org/)
- [Jupyter](http://jupyter.org/)
- [Tensorflow + Junyper + Docker](http://containertutorials.com/docker-ml/tensorflow_jupyter.html)
- [CS229:  Machine Learning Andrew Ng.](http://cs229.stanford.edu/)
- [Machine Learning Crash Course - @developers.google.com](https://developers.google.com/machine-learning/crash-course/)
- [Deep Reinforcement Learning @Berkely Fall 2017](http://rll.berkeley.edu/deeprlcourse/) [Videos](https://www.youtube.com/playlist?list=PLkFD6_40KJIznC9CDbVTjAF2oyt8_VAe3)
- [Deep Learning Frameworks (powered by NVidia)](https://developer.nvidia.com/deep-learning-frameworks)
- [NVidia GPU Cloud](https://www.nvidia.com/en-us/gpu-cloud/)
- [Keras: The Python Deep Learning library](https://keras.io/)

## Links

- [Machine Learning-Driven Bundling. The Future of JavaScript Tooling. · Minko Gechev's blog](http://blog.mgechev.com/2018/03/18/machine-learning-data-driven-bundling-webpack-javascript-markov-chain-angular-react/)
- [awesome-deeplearning-resources/dl.md at master · endymecy/awesome-deeplearning-resources](https://github.com/endymecy/awesome-deeplearning-resources/blob/master/papers/2018/dl.md)
- [https://developers.google.com/machine-learning/crash-course/](https://developers.google.com/machine-learning/crash-course/)
- [https://hub.docker.com/r/jupyter/tensorflow-notebook/](https://hub.docker.com/r/jupyter/tensorflow-notebook/)
- [The Great A.I. Awakening - The New York Times](https://www.nytimes.com/2016/12/14/magazine/the-great-ai-awakening.html)
- [Jeffrey Dean - Research at Google](https://research.google.com/pubs/jeff.html)
- [RNNLM Toolkit](http://www.fit.vutbr.cz/~imikolov/rnnlm/)
- [A Gentle Introduction to Neural Machine Translation - Machine Learning Mastery](https://machinelearningmastery.com/introduction-neural-machine-translation/)
- [[1609.08144] Google's Neural Machine Translation System: Bridging the Gap between Human and Machine Translation](https://arxiv.org/abs/1609.08144)
- [Tensorflow Jupyter notebook on Docker &mdash; Container Tutorials](http://containertutorials.com/docker-ml/tensorflow_jupyter.html)
- [[1409.3215] Sequence to Sequence Learning with Neural Networks](https://arxiv.org/abs/1409.3215)
- [Understanding Deep Learning through Neuron Deletion | DeepMind](https://deepmind.com/blog/understanding-deep-learning-through-neuron-deletion/)
- [https://arxiv.org/pdf/1609.08144.pdf](https://arxiv.org/pdf/1609.08144.pdf)
- [Jupyter + Tensorflow + Nvidia GPU + Docker + Google Compute Engine](https://medium.com/google-cloud/jupyter-tensorflow-nvidia-gpu-docker-google-compute-engine-4a146f085f17)
- [Research Blog: Semantic Image Segmentation with DeepLab in TensorFlow](https://research.googleblog.com/2018/03/semantic-image-segmentation-with.html)
- [models/research/deeplab at master · tensorflow/models](https://github.com/tensorflow/models/tree/master/research/deeplab)
- [models/research at master · tensorflow/models](https://github.com/tensorflow/models/tree/master/research)
- [Examples - Cityscapes Dataset](https://www.cityscapes-dataset.com/examples/)
