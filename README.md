# Fake Review Detection using GAN BERT


In the current world, we habitually turn to the wisdom of our peers, and often complete strangers, for advice, instead of merely taking the word of an advertiser or business owner. A 2015 study by marketing research company Mintel found nearly 70 percent of Americans seek out others’ opinions online before making a purchase.

Consumer reviews sites has become target of deceptive reviews spam. Heavy reliance on review information by users has dramatic effects on business owners. It has shown that even half star rating on Amazon helps business owners to sell out 19 percentage points more frequently. Even the fictitious reviews are written to sound authentic. Hence, to counter this issue we designed and implemented a model to detect fake or deceptive reviews using Generative Adversarial Networks. 

# Previous Work: 

People have used RNN for generator which better captures contextual information and realising semantics of long texts.Most of the research have worked on detecting fake reviews using supervised learning algorithms. But the problem with RNN is that it is not suitable for task like review detection(it is biased model that gives more influence to later words than the earlier words). They used CNN model, which has shown to be effective for text classification but they need to be careful with the window size. 

Also the problem with classification methods is the lack of substantial ground truth values for most of supervised learning techniques. Hence, we implement semi-supervised learning algorithm i.e. the Generative Adversarial Networks.

# GAN-BERT:

GAN-BERT is an extension of BERT which uses a Generative Adversarial setting to implement an effective semi-supervised learning schema. It allows training BERT with datasets composed of a limited amount of labeled examples and larger subsets of unlabeled material. In particular, the Semi-Supervised GAN is used to make the BERT fine-tuning robust in such training scenarios where obtaining annotated material is problematic. In fact, when fine-tuned with very few labeled examples the BERT model is not able to provide sufficient performances.

With GAN-BERT we extend the fine-tuning stage by introducing a Discriminator-Generator setting, where:
•	the Generator G is devoted to producing "fake" vector representations of sentences;
•	the Discriminator D is a BERT-based classifier over k+1 categories.

![ganbert](https://user-images.githubusercontent.com/79450753/146297838-71c0ebc5-9a06-4569-a29e-e1caddf64d61.jpg)

D- Distinguishes between Truthful reviews, deceptive reviews and Fake reviews from Generator.
G- Produces samples resembling data distribution.

Here labelled data is used to train the discriminate well, Unlabelled data as well as generated data is used to improve inner representations. We enrich the BERT fine-tuning with this approach.
Reviews are classified into 3 Classes.
Class 1- Truthful
Class 2- Deceptive
Class 3- Fake generated data.

# Discriminator Loss function

![Capture1](https://user-images.githubusercontent.com/79450753/146299025-ec5d03d9-4176-40f9-9de7-6e27269bc36e.JPG)

This is supervised loss.

![Capture2](https://user-images.githubusercontent.com/79450753/146299224-f2b06d2c-3ff3-4ed7-93cd-ce26e29d6940.JPG)

This is semi-supervised and generative loss.

![Capture3](https://user-images.githubusercontent.com/79450753/146299469-d787f1ab-0c4e-4de8-921b-b172bdf218a8.JPG)

This is the total loss from Discriminator.

D has the role of classifying an example with respect to the k categories of the task of interest, and it should recognize the examples that are generated by G (the k+1 category). G, instead, must produce representations as much similar as possible to the ones produced by the model for the "real" examples. G is penalized when D correctly classifies an example as fake.

In this context, the model is trained on both labeled and unlabeled examples. The labeled examples contribute to the computation of the loss function with respect to the task k categories. The unlabeled examples contribute to the computation of the loss functions as they should not be incorrectly classified as belonging to the k+1 category (i.e., the fake category).

# Generator Loss function 

![Capture4](https://user-images.githubusercontent.com/79450753/146300437-ba78d887-8ede-47e7-9bc3-57ad95d3d128.JPG)

The generator should produce examples  whose intermediate representations provided input to D are very similar to real ones.
f(x) is the activation on an intermediate layer of D


![Capture5](https://user-images.githubusercontent.com/79450753/146300544-d476ce70-af15-420f-95e6-ed0d9925c850.JPG)

This is loss from semi-supervised data. Thus we add both the losses coming from the generator.

# Dataset

https://osf.io/tyue9/

Our dataset has 40K reviews from Amazon reviews with equal number of truthful and deceptive reviews. Out of the 40K, 8K (20%) reviews were used for testing, which has equal number of truthful and deceptive reviews.

# Evaluation

Graphs for Batch Size 64, and Sequence Length 64

![Capture6](https://user-images.githubusercontent.com/79450753/146302484-e23e78b1-a1dd-40a0-b5fb-b34c47ab603e.JPG)![Capture7](https://user-images.githubusercontent.com/79450753/146302617-ec002092-0560-4aca-8e7d-0e28052fddca.JPG)

![Capture8](https://user-images.githubusercontent.com/79450753/146302720-c9cd522f-6313-492f-9a9e-780b3720f754.JPG)![Capture9](https://user-images.githubusercontent.com/79450753/146302722-984638c9-b07a-4b0f-82ac-3841b91afc50.JPG)

So these are the train loss, test loss and accuracy figures. We train the GAN-BERT model under batch size 64 and sequence length 64, and run the model for 10 epochs. As we can see, the generator train loss and discriminator train loss become lower and lower at each epoch, and the accuracy becomes higher.

![Capture6](https://user-images.githubusercontent.com/79450753/146302484-e23e78b1-a1dd-40a0-b5fb-b34c47ab603e.JPG)![Capture7](https://user-images.githubusercontent.com/79450753/146302617-ec002092-0560-4aca-8e7d-0e28052fddca.JPG)

![Capture8](https://user-images.githubusercontent.com/79450753/146302720-c9cd522f-6313-492f-9a9e-780b3720f754.JPG)![Capture9](https://user-images.githubusercontent.com/79450753/146302722-984638c9-b07a-4b0f-82ac-3841b91afc50.JPG)

These are figures that training the model under batch size 128 and sequence length 64. As we can see, when using a larger batch there is a significant degradation in the quality of the model, as measured by its ability to generalize. The lack of generalization ability is due to the fact that large-batch methods tend to converge to sharp minimizers of the training function. Having a very large epoch size will not necessarily improve the accuracy. Epoch sizes can increase the accuracy up to a certain limit beyond which you begin to overfit your model. Having a very low one will also result in underfitting.

# BGAN-BERT

![Capture11](https://user-images.githubusercontent.com/79450753/146304332-001d4e82-4e08-4276-96cf-e9964048d188.JPG)
![Capture12](https://user-images.githubusercontent.com/79450753/146304334-eb588730-15ed-4689-afe2-68a1e0757758.JPG)
![Capture13](https://user-images.githubusercontent.com/79450753/146304335-a037933f-77cd-44e0-988c-343da31a68c6.JPG)

More informative priors will have a greater influence on the posterior distribution for a given sample size. Larger sample sizes will give the likelihood function greater influence on posterior distribution for a given prior distribution. In practice, precise estimation of posterior distribution with smaller sample sizes with more informative sizes and vice versa.

# Structure

-Generator: MLP, generates textual data similar to that fed from the generator as tokens.

-Discriminator: MLP, classifies tokens in vectors as 0s(fake) or 1s(real).

-Activation Function: LeakyReLU


# BAYESIAN LEARNING

Need for Bayesian approach:

Probabilities are based on a degree of belief in an event. It gets Updated as new information is available. Without this, a better model may be left unexplored.


# MARKOV CHAIN MONTE CARLO

# What is it?:

It is a class of algorithms used to sample from probability distributions

Each Markov step samples the target distribution, more steps, more samples, more accurate.

For Bayesian GAN-BERT, Markov screening induces the belief that the next review (t+1) to be generated depends only on the previous review t.

Here, we learn an unknown distribution. 

We construct a set of samples and search over a range of possible outcomes.


# Working:

Define domain of possible inputs and generate random samples.

Aggregate and summarize and find the value of the hyperparameters that give us the best output. The best output is determined from the values of mean and variance in out implementation.


Since Monte Carlo search work best for large iterations and 100s of iterations for training dataset of 32000 samples are run, it requires massive computation power.
Hence, we have chosen a different route where, we have sampled hyperparameters within a selected range of 0.2 to 0.0000009.


# BGAN-BERT RESULTS

Number of epoch = 5; Batch size = 128; Hidden layers = 1 (both)

![Capture14](https://user-images.githubusercontent.com/79450753/146307362-a1f6eaf3-2df5-48a4-8a97-534dbcf2af16.JPG)

Accuracy: Mean vs Variance

Number of epoch = 5; Batch size = 128; Hidden layers = 1 (both)

![image](https://user-images.githubusercontent.com/79450753/146307673-b4b194ba-5fc8-4b45-9267-8bfee07ca81b.png)


# CONCLUSION

With lower batch sizes, better are the accuracy results and lower are the test losses, reason: better generalization.

The accuracy of BGAN-BERT is better than that of the GAN-BERT performs better. Reason: better learning parameters.


--***--








