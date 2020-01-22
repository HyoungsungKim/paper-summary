# Generative Adversarial Nets

## Abstract

We simultaneously train two models:

- A generative model G(생성자) that captures the data distribution
  - The training procedure for G is to maximize the probability of D making a mistake.
- A discriminative model D(판별자) that estimates the probability that a sample came from the training data rather than G. 

## Related work

Note that in many interesting generative models with several layers of latent variables (such as DBN(Deep Brief Network)s and DBM(Deep Boltzmann Machine)s), ***it is not even possible to derive a tractable unnormalized probability density.***

- 많은 생성 모델은 확률 변수를 알아내는 것이 불가능 함

## Adversarial nets

***The adversarial modeling framework is most straightforward to apply when the models are both multilayer perceptrons.*** 

To learn the generator’s distribution $$p_g$$ over data `x`, we define a prior on input noise variables $$p_z(z)$$,  then represent a  mapping to data space as $$G(z;\theta_g)$$,  where $$G$$ is  a differentiable function represented by a multilayer perceptron with parameters $$\theta_g$$

- Generator

$$
p_z(z) \rightarrow G(z;\theta_g) \rightarrow p_g
$$

We also define a second multilayer perceptron $$D(x;\theta_d)$$ that ***outputs a single scalar.*** $$D(x)$$ represents the probability that `x` came from the data rather than $$p_g$$.  We train D to maximize the probability of assigning the correct label to both training examples and samples from G. 

- Discriminator

We train D to maximize the probability of assigning the correct label to both training examples and samples from G.
$$
\min_G\max_D V(D,G) = \mathbb{E}_{x \sim p_{data}(x)}[\log D(x)] + \mathbb{E}_{z \sim p_z(z)}[log(1 - D(G(z)))]
$$

- 앞 : 판별자 -> D(x)를 최대로 가는건 판별을 완벽하게 해낸 것을 의미
- 뒤 : 생성자 -> G(z)를 최소로 만든 값을 D에 넣었을때 작게 나오는건 생성을 잘 한것임

