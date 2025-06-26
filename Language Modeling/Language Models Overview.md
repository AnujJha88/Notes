 So all the chatbots we see. 

### What matter when training LLMs?
- Architecture: LLMs are neural nets at their core, and so architecture plays a huge role
- Training Loss and Algorithm
- Data
- Evaluation: How do we know we are actually making progress 
- System: How do we actually make these run on modern hardware

LLMs are all based on transformers, or some version of them.
Most of Academia focuses on the first two.

The rest are more important in practice and what industry focuses on.

#### Pretraining
Classical language modeling paradigm where you train your model to essentially model all of internet.
e.g. GPT3
#### Post-training
More recent. Take the LLMs and make them into AI assistants.
e.g. ChatGPT

## Language Modeling
A language model is a probability distribution over sequences of tokens/words $p(x_{1},\dots,x_{L})$

So if you have a sentence like say "The mouse ate the cheese" the model gives you the probability of this sentence being found online or uttered by a human.
![[Pasted image 20250625125054.png]]

Model should have some syntactic knowledge to catch grammar errors and output a lower probability of appearing.

Also it should have some semantic knowledge and be able to tell that cheese does not usually eat the mouse.

Generative models: can generate some data(sentences, images).

LMs are generative as once you have a model of a distribution you can simply sample and generate data. Thus you can generate sentences using a LM.


Currently we use AutoRegressive(AR) Language Models. What these do is they use the chain rule to split the probability into a bunch of factors.
![[Pasted image 20250625125407.png]]

$P(next word| past)$ i.e. predicting next word using context of what happened in the past.


It has a drawback that is you want to sample a sentence from this model it first generates first word then goes back and generates second conditioned on the first and so on and so it will take quite a lot of time. 

### Autoregressive LM
**Task:** Predict the next word
Steps involved:
1. **Tokenize:** You tokenize these words or subwords and then give a reference ID to each token.
2. **Forward:** Pass this token sequence through the model to get an output that is the probability distribution over the next word/token.
3. **Sample**: Sample a token from this distribution
4. **Detokenize**: You detokenize the new ID you obtained in the previous step and you're done.
This is how you sample from a  language model.

Last two steps outlined above are inference time steps.

In training you predict the most likely token and then compare with the ground truth and then change your weights accordingly.

Output has to be same dimensionality as your number of tokens and you can add more tokens, there are methods to do it but no one does it in practice.

### AR Neural Language Models
Taken every token and then create an embedding of them, pass them through the neural net, in our case a transformer, and then you get a representation of all the words in the context, so basically a representation of all the words in a sentence.

![[Pasted image 20250625213440.png]]
Pass this representation to a linear layer to change its size from length of sentence to the vocabulary size. Pass it through a SoftMax to get your probability distribution over next token given the context. 

## Loss
It is a classification of the next tokens' index and so we use a cross entropy loss.
![[Pasted image 20250625213422.png]]

Target is usually a one-hot vector.

This is just the same as maximizing the text's log likelihood.

![[Pasted image 20250625213609.png]]

# Tokenizer

Why do we even need this? Tokens are more general than words.

Say you want to consider each word as a token of its own but then if you make a typo then there is no token associated with this "typo" and so how do you reference it and pass into the LLM?

In Thai there is no simple way to tokenize by spaces as there are no spaces between the words. So you do need something more general.

You cannot also just declare each character as its own token as then the length of your sequence is much longer and the complexity grows quadratically with length so we want to avoid this.

So the idea is to give common subsequences a certain token. A token is on average 3-4 letters.

An example of tokenizing algorithm is Byte Pair Encoding.
Steps to train a tokenizer:
1. Start with a large corpus of text
2. Start with one token per character
3. ![[Pasted image 20250626110011.png]]
4. Mere common pairs of tokens into one token.![[Pasted image 20250626110027.png]]
5. Repeat until you get your desired vocabulary size or all are merged.![[Pasted image 20250626110119.png]]


![[Pasted image 20250626110140.png]]

GPT-3 Tokenizer output for same text.
**Q. How to deal with spaces or periods etc?**
In theory there is no reason to deal with spaces or punctuation separately and say every space and punctuation gets its own token and then do all the merging.

But its a efficiency question. So in pre-tokenizer stage we say we would not look at the words coming before the space and then the words coming after i.e. we are not merging between spaces. But this is just a computational optimization. In theory you can just do the same as any other character.

You also usually keep the smaller tokens that you made as if there is say some typos then you want to be able to represent the words by character.

**Q. Are the tokens unique? Is there only one occurrence or do you need to have multiple occurrences to make them take on a different meaning or something?** 
A. Every token has its own unique ID. If we have the word "bank" it could have a couple of different meaning depending on context. But it will have the same token and the model will learn based on the context whether to use the representation for one meaning or for a different one. The transformer is the one doing it not the tokenizer.

When you try to use the tokenizer you always choose the longest token that you can apply to it. So you do not use the smaller ones that you stored earlier unless needed.

All of this is computational optimization.

**Q. Why would people want to move away from tokenizers?**
One example is math. Numbers are not currently tokenized so a number might have its own token and so models do not see the numbers the same way we do. This is quite annoying as the reason for math being so general is because we can deal with each letter separately and then do composition. But models cannot do this so they have to do some special tokenization.


GPT-4 changed the way they tokenized code. Before that there was a strange way to deal with indents so it was harder to understand the code for the model.

Tokenizers are quite important.

## LLM Evaluation: Perplexity
It's similar to your validation loss. We just use something more interpretable, the average per token loss(independent of length), and then you exponentiate it because you want to make sure the quantities are independent of the base of the log(in units of the vocabulary size $|V|$).

![[Pasted image 20250626111811.png]]


$$1\leq PPL\leq|V|$$

1 is the best  and corresponds to perfect prediction. If you  have no idea and so you do a random prediction with probability $\frac{1}{|V|}$ and then math gives you that $PPL=|V|$

Intuitively it is the number of tokens that your model is hesitating between.

![[Pasted image 20250626112111.png]]


Dropped form 70 to 10 in 2017-23.

Perplexity is not used academically as it is very dependent on the tokenizer and the data that you use to evaluate and some other design choices but it is still quite important in developing them.

Imagine that ChatGPT uses tokenizer with 10,000 vocab size and Gemini has 100,000. Then the Gemini model would have the upper bound of the perplexity worse than GPT. 
## LLM Eval: Aggregating standard NLP benchmarks

More common in academia.

Collect many automatically evaluable benchmarks and the evaluate across all of them.

![[Pasted image 20250626112421.png]]

E.g. HELM

Mix of many things that can be easily evaluated like QA(Question Answering).
Usually you know the correct answer and so you can evaluate these models based on the likelihood of the models to predict that versus the other options.

e.g. MMLU(most common academic benchmark)
![[Pasted image 20250626112904.png]]


Collection of questions and answers in a variety of topics. MCQs. You can either look at the likelihood of the model generating the answers or you can ask what the most likely answer is. This is not unconstrained generation, as it can only answer among  the given choices.

So when you ask the most likely you prompt the model to output among the four and you would also constrain to only look at these four tokens in the output. In the first case you do not even need to generate anything as its a LM it will give a distribution over sentences and you look at the likelihood of generating all of the words you want.


And then you look at if the most likely sentence is actually the answer. No sampling, just use $p(x_{1},\dots,x_{L})$.

## Challenges in Evaluation.

1. For a long time even for this classical benchmark different companies and different orgs were using different ways to evaluate MMLU and hence you get completely different result. Prompting is a whole separate issue.
2. Train-Test Contamination: quite important in academia but not so much for companies as they know what they trained on. So for us its a real problem.
	
 There are a few ways to see if the test set was in the training set. One way is that given that most of the datasets online are not randomized and LMs just predict the next word given the context you can just look at the entire test set and if you generate all examples in order versus in a different order and if there is a disparity that means the train set was contaminated.


# Data

You basically train LLMs on all of (clean)internet. Internet is quite dirty and not representative of what we want in practice. If you download a random website now it would contain a lot of useful stuff. 
Steps:
1. Download all of internet using web crawlers like Common Crawl(adds in all the new websites found every month. It is an established crawler)(~250 billion pages $\approx$ 1PB of data)
	![[Pasted image 20250626120209.png]]
	This is the raw data you crawl form the internet and so it is pretty much illegible.
	Not useful to train LLM to generate this.
2. Extract text from this html by parsing the correct tags.
3. Challenges here are math and boilerplates(e.g. forms have all the same headers and footers but will have widely varying content).
4. Filter undesirable content(e.g. NSFW, harmful, PII etc.)(companies maintain a blacklist of websites and do not train on any data from them)(or train a small model to classify data into good or bad).
5. Deduplication e.g. headers, footers, menus, URLs that go to same website, Paragraphs that are duplicated a ton of time across sources.
6. Heuristic filtering to remove low quality documents based on rules that you define e.g. outlier tokens(unusual token distribution), length of words ,number of words etc. 

	Why not actively penalize the model for getting undesirable content? This is what is done in post-training. Pre-training you just try to model how humans speak and remove headers, footers and menus
	
7. Model based filtering: