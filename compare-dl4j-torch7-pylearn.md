---
title: 
layout: default
---

# DL4J vs. Torch vs. Theano vs. Caffe

Deeplearning4j is not the first open-source deep-learning project, but it is distinguished from its predecessors in both programming language and intent. DL4J is a JVM-based, industry-focused, commercially supported, **distributed deep-learning framework** intended to solve problems involving massive amounts of data in a reasonable amount of time. It integrates with Hadoop and Spark using an arbitrary number of GPUs or CPUs, and it has [a number you can call](http://www.skymind.io/contact/) if anything breaks. 

### Theano/PyLearn2

Most academic researchers in the field of deep learning rely on [**Pylearn2**](http://deeplearning.net/software/pylearn2/) and [Theano](http://deeplearning.net/software/theano/), which are written in Python. Pylearn2 is a machine-learning library, while Theano is a library that handles multidimensional arrays, like Numpy. Both are powerful tools widely used for research purposes and serving the large Python community. They are well suited to data exploration and explicitly state that they are intended for research. 

Pylearn2 is a normal (non-distributed) framework that includes everything necessary to conduct experiments with multilayer Perceptrons, [restricted Boltzmann machines](../restrictedboltzmannmachine.html), Stacked Denoising Autoencoders and [Convolutional nets](convolutionalnets.html). We recommend it for precisely those use cases. In contrast, Deeplearning4j intends to be the equivalent of Scikit-learn in the deep-learning space. It aims to automate as many knobs as possible in a scalable fashion on parallel GPUs or CPUs, integrating as needed with Hadoop and Spark. 

### Torch

[**Torch**](http://torch.ch/) is a computational framework written in Lua that supports machine-learning algorithms. Some version of it is used by large tech companies such as Google and Facebook, which devote in-house teams to customizing their deep learning platforms. Lua is a multi-paradigm scripting language that was developed in Brazil in the early 1990s. 

Torch7, while powerful, [was not designed to be widely accessible](https://news.ycombinator.com/item?id=7929216) to the Python-based academic community, nor to corporate software engineers, whose lingua franca is Java. Deeplearning4j was written in Java to reflect our focus on industry and ease of use. We believe usability is the limiting parameter that inhibits more widespread deep-learning implementations. We believe scalability ought to be automated with open-source distributed run-times like Hadoop and Spark. And we believe that a commercially supported open-source framework is the appropriate solution to ensure working tools and building a community. 

### Caffe

[Caffe](http://caffe.berkeleyvision.org/) is a well-known and widely used machine-vision library that ported Matlab's implementation of fast convolutional nets to C and C++ ([see Steve Yegge's rant about porting C++ from chip to chip if you want to consider the tradeoffs between speed and this particular form of technical debt](https://sites.google.com/site/steveyegge2/google-at-delphi)). Caffe is not intended for other deep-learning applications such as text, sound or time series data. Both Deeplearning4j and Caffe perform image classification with convolutional nets, which represent the state of the art. In contrast to Caffe, Deeplearning4j offers parallel GPU *support* for an arbitrary number of chips, as well as many, seemingly trivial, features that make deep learning run more smoothly on multiple GPU clusters in parallel. While it is widely cited in papers, Caffe is chiefly used as a source of pre-trained models hosted on its Model Zoo site. Deeplearning4j is [actively building a parser](https://github.com/deeplearning4j/deeplearning4j/pull/480) to import Caffe models to Spark.

### Licensing

Licensing is another distinction among these open-source projects: Theano, Torch and Caffe employ a BSD License, which does not address patents or patent disputes. Deeplearning4j and ND4J are distributed under an **[Apache 2.0 License](http://en.swpat.org/wiki/Patent_clauses_in_software_licences#Apache_License_2.0)**, which contains both a patent grant and a litigation retaliation clause. That is, anyone is free to make and patent derivative works based on Apache 2.0-licensed code, but if they sue someone else over patent claims regarding the original code (DL4J in this case), they immediately lose all patent claim to it. (In other words, you are given resources to defend yourself in litigation, and discouraged from attacking others.) BSD doesn't address the issue. 

### Speed

Deeplearning4j's underlying linear algebra computations, performed with ND4J, have been shown to run [at least twice as fast](http://nd4j.org/benchmarking) as Numpy on very large matrix multiplies. That's one reasons why we've been adopted by teams at NASA's Jet Propulsion Laboratory. Moreover, Deeplearning4j has been optimized to run on various chips including x86 and GPUs with CUDA C.

While both Torch7 and DL4J employ parallelism, DL4J's **parallelism is automatic**. That is, we automate the setting up of worker nodes and connections, allowing users to bypass libs while creating a massively parallel network on [Spark](https://github.com/deeplearning4j/deeplearning4j/tree/master/deeplearning4j-scaleout/spark), [Hadoop](https://github.com/deeplearning4j/deeplearning4j/tree/master/deeplearning4j-scaleout/hadoop-yarn), or with [Akka and AWS](http://deeplearning4j.org/scaleout.html). Deeplearning4j is best suited for solving specific problems, and doing so quickly. 

For a full list of Deeplearning4j's features, please see our [features page](../features.html).

### Why Java?

We're often asked why we chose to implement an open-source deep-learning project in Java, when so much of the deep-learning community is focused on Python. After all, Python has great syntactic elements that allow you to add matrices together without creating explicit classes, as Java requires you to do. Likewise, Python has an extensive scientific computing environment with native extensions like Theano and Numpy.

Yet Java has several advantages. First of all, as a language it is inherently faster than Python. Anything written in Python by itself, disregarding its reliance on Cython, will be slower. Admittedly, most computationally expensive operations are written in C or C++. (When we talk about operations, we also consider things like strings and other operations involved with higher-level machine learning processes.) Most deep-learning projects that are initially written in Python will have to be rewritten if they are to be put in production. Not so with Java.

Secondly, most major companies worldwide use Java or a Java-based system. It remains the most widely used language in the world. That is, many programmers solving real-world problems could benefit from deep learning, but they are separated from it by a language barrier. We want to make deep learning more usable to a large new audience that can put it to immediate use. 

Thirdly, Java's lack of robust scientific computing libraries can be solve by writing them, which we've done with [ND4J](http://nd4j.org), which runs on distributed GPUs or GPUs, and can be interfaced via a Java or Scala API.

Finally, Java is a secure, network language that inherently works cross-platform on Linux servers, Windows and OSX desktops, Android phones and in the low-memory sensors of the Internet of Things via embedded Java. While Torch and Pylearn2 optimize via C++, which presents difficulties for those who try to optimize and maintain it, Java is a "write once, run anywhere" language suitable for companies who need to use deep learning on many platforms. 

###Ecosystem

Java's popularity is only strengthened by its ecosystem. [Hadoop](https://hadoop.apache.org/) is implemented in Java; [Spark](https://spark.apache.org/) runs within Hadoop's Yarn run-time; libraries like [Akka](https://www.typesafe.com/community/core-projects/akka) made building distributed systems for Deeplearning4j feasible. In sum, Java boasts a highly tested infrastructure for pretty much any application, and deep-learning nets written in Java can live close to the data, which makes programmers' lives easier. Deeplearning4j can be run and provisioned as a YARN app.

Java can also be used natively from other popular languages like Scala, Clojure, Python and Ruby. By choosing Java, we excluded the fewest major programming communities possible. 

While Java is not as fast as C or C++, it is much faster than many believe, and we've built a distributed system that can accelerate with the addition of more nodes, whether they are GPUs or CPUs. That is, if you want speed, just throw more boxes at it. 

Finally, we are building the basic applications of Numpy, including ND-Array, in Java for DL4J. We believe that many of Java's shortcomings can be solved quickly, and many of its advantages will continue for some time. 

### Scala

We have paid special attention to [Scala](http://deeplearning4j.org/scala.html) in building Deeplearning4j and ND4J, because we believe Scala has the potential to become the dominant language in data science. Writing numerical computing, vectorization and deep-learning libraries for the JVM with a [Scala API](http://nd4j.org/scala.html) moves the community toward that goal. 

To really understand the differences between DL4J and other frameworks, you may just have to [try us out](http://deeplearning4j.org/quickstart.html).