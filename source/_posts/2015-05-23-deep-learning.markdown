---
layout: post
title: "Deep Learning"
date: 2015-05-23 12:25:57 -0400
date_formatted: May 23, 2015
comments: true
categories: [personal, blog, machine learning, deep learning]
---
<p>
As part of the final project for <a href="http://www.cs.columbia.edu/~jebara/4772/" targer="_blank">COMS W4772: Advance Machine Learning</a>, a course I took at Columbia University during Spring 2015, my friend Rhea and I were exploring various adaptive learning techniques.  The idea was to study and compare various adaptive learning methods on different datasets and evaluate the pros and cons of each.
</p>

<p>
We kept the Stochastic Gradient Descent (SGD) as the baseline and compared it with Momentum, Nestorov's Accelerated Gradient (NAG), AdaGrad, and AdaDelta on the Digits (digit image data for OCR task), 20newsgroup (document collection) and Labeled Faces  in the Wild (LFW) (face image data) datasets.
</p>

<p>You can read the final report <a href="assets/AML_Project_Report.pdf" target="_blank">here</a>! 
</p>

<p>Also, as part of another research project, I was studying the effects of the different hyper-parameters on learning in a deep neural network. Using synthetic data, I was evaluating the role of learning rate, batch size, activation functions, hidden units, parameter initialization, learning methods in the working of a neural network. I then ran experiments on 20newsgroup data set to compare the final results as compared to SVM and Naive Bayes.</p>

<p>A TL;DR version is as follows: 
	<ul>
		<li> Too high learning rate causes divergence; too low causes to learn very slowly. Try various values to see which gives you faster convergence. A better approach is to use adaptive learning (explained later).
		<li> Too high batch size (behaves like standard gradient descent) is computationally slow as it processes many training examples for each update. Too low batch size (behaves like online learning) learns much faster but affected by noise. A moderate value (10%-30% of data size) works well. 
		<li> Use Rectified Linear Units (ReLUs) or tanh functions.
		<li> Increasing the hidden units let's you learn complex functions. Increasing too much may cause over-fitting. Choose a value using a validation set. 
		<li> Simplest approach is to initiallize randomly using a Gaussian distribution with mean 0 and standard deviation 1. 
		<li> Use adaptive learning techniques like AdaGrad, AdaDelta, etc. to let learning rate vary per dimension. This is very important for deep neural nets since the magnitude of gradients differ a lot between different layers. Eliminates the need of a global learning rate.  
	</ul>
</br>

The longer version can be found <a href="http://www.deepmusings.com/assets/DeepLearning_Project.pdf" target="_blank">here!</a>
</p>
