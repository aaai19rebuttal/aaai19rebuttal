# Paper Title: User-Oriented Paraphrase Generation with Keywords Controlled Network


Paper ID 5654


This paper proposes a user-oriented paraphrase generation framework, mainly depending on additional keywords inputs. I think this idea is novel, useful and influential. However, as it is different from most existing models, there are a lot of things to be done related to keywords extraction. Thus, it is difficult to include all aspects of keywords extraction in 7-page conference paper. 

There are a lot of way to extract keywords, and it is better to jointly extract keywords and to generate paraphrase. Considering a lot of possibilities to deal with it, we simply use the simplest way and only focus on showing the strength of KCN.

I do think this paper will be influential, because it is the first one to focus on the requirement of users and propose an efficient way to fulfill it. Many questions related to keywords extraction are also reviewers’ “Why not ……? I can do it better!”. But are they really better? There will be a lot of future works! 


## Reviewer 1
1.	But in the subpart of encoder，the authors employ Bi-GRU to encode separated keywords which need a few explanation.

Because the order of keywords sequence is important, which controls the outputs, so we use Bi-GRU encoder to capture this order information. Figure 3 can be clearer, we will update it in the final version.

2.	how to restrict the number of keywords or the influence of different number of keywords should be described concretely.

The purpose of keywords extraction subpart in the training stage is to simulate the real-world scenario, keywords extraction by users. The simulation contains two parts: PositionRank and RandomSampling. PositionRank is the existing model which cannot be restrict by us, while RandomSampling is added aiming to learn the difference between source and target, as we mentioned in page 3. 

We find that

1)	Without PositionRank, the model is easy to generate wrong sentence.

2)	Without RandomSampling, or with few RandomSampling, the model is easy to meet “mode collapse” problem like in GAN area.

In the test stage, especially in assistant experiment Table 4. The less the keywords contains, the less information the outputs possess, and it would be error-prone.


## Reviewer 2

1.	The definition of the task should be elaborated. It is not clear whether all of the user given keywords should be included in the target sentence and whether their order should be preserved.

The order of the keywords is important. Different orders may generate different outputs. The order information is learnt by Bi-GRU encoder, which is illustrated in Methodology and Experiments.

No guarantee to copy all the keywords. If the keywords are irrelevant to source sentence, KCN might output non-sensical results. This is similar to SCPN who postprocesses to remove non-sensical results. Moreover, KCN aims to help users, not to be fooled by users. Both users and KCN should know which keywords are appropriate. In assistant experiment, we can see that KCN performs well to copy required keywords.

The coverage of keywords in the outputs is important. We will add more analysis about it. 

1.	The comparison is unfair, KCN should be compared with SCPN?

Comparing KCN with SCPN in automatic evaluation is not meaningful, because the key points of both lie in the controlled outputs, which is hard to evaluate by human nor machine. In addition, their inputs are different. Actually, they are two different genres in controlled paraphrasing automation area (syntax template and keywords). Moreover, SCPN uses tricky postprocessing to filter non-sensical outputs, which is hard to design a fair comparison.


2.	Autoencoder mode is necessary?

Yes, it is necessary.

Please see page 5 experimental setup part, the last paragraph *threshold*.

We think the motivation of autoencoder mode is intuitive. Without it, KCN cannot generate diverse outputs. The pages are limited, so we don’t want to add ablation study for it.

To testify whether autoencoder mode works for real world, we show some results of it in assistant experiment. Table 4.

3.	Why not use Quora Dataset?

As we mentioned in 5, the evaluation of paraphrase generation is very difficult. Test KCN in Quora may not reveal more advantages and disadvantages. Moreover, most of the sentences in Quora are questions, whose syntax are monotonous like those in MSCOCO. PARANMT50M are more like diverse real world sentences.

Test KCN in more dataset is good, but this is a conference paper and we only have 7 pages.



1.	In P.2, it mentions “in Fig. 2, the order of keywords refrigerator microwave determines the outputs order” . How will the order of keywords determine the output order by current model?

In the training stage, different keywords correspond to different target sentence, i.e. autoencoder mode and paraphrase mode. We hope that KCN can learn the dependency to keywords and generalize to more diverse keywords (test in assistive experiment).

2.	In P.3, it mentions “KCN takes either <s^s, k^s, s^t> or <s^s, k^t, s^t> randomly”. The former triple should be “<s^s, k^s, s^s>” for autoencoder, right?

You are right, that’s a mistake.

3.	Equation (6) shows k^s is related to target sentence k_r^t, but it is used for autoendoer and should not be include any information from target sentence, right?

You are right, that’s a mistake.

In my code, k^s = {k_p^s, k_r^s} k_r^s = RandomSample(set(s_s) - set(s_t)) (It does not include information from target) for the sake of the balance of keywords number. It can be k^s = {k_p^s} as well, but it will make the training unstable.

4.	In equation (7), how is k_r^s defined?

k_r^s = RandomSample(set(s_s) - set(s_t)), like Eq. 5

5.	In P.4, is o_i the concatenation of o_i^s and o_i^k?

Yes

6.	In P.4, does the decoder have the ability to copy from the source sentence or the user keywords or both? 

That’s an interesting question. The current model does not have this ability.

Now we use mode-gate to switch between copy-mode and generative mode. But why cannot it be a 3 classes mode-gate (copy-source, copy-keywords, generative)?

We think the 3 classes mode-gate is imbalanced. Because of the property of paraphrase generation, most of the words will be copied from source and keywords while only few words will be generated from a softmax over the entire vocabulary. That will hinder the learning of generative-mode. However, generative-mode is very important, because we hope KCN can help users with more flexibilities.

This is an interesting variant of KCN, we will add this to experiments when we have more pages (e.g. Journal)

## Reviewer 3
1.	In table 4 in which you present a few outputs of your system, I must admit that a simplistic rule-based system could have obtained similar results.

I select the simplest examples to illustrate KCN, maybe I should add more to appendixes.

I don’t think it is necessary. A rule-based system can deal with paraphrase, but it is hard to do it well. You can Google “paraphrase” and try some of the systems. I guess most of them are rule-based systems. Their outputs are unnatural and error-prone. 

Paraphrase generation is a hard task. You can begin from 
Vila, M.; Mart´ı, M. A.; and Rodr´ıguez, H. 2014. Is this a paraphrase? what kind? paraphrase boundaries and typology. Open Journal of Modern Linguistics 4(01):205.

Fujita, A. 2005. Automatic Generation of Syntactically Well-formed and Semantically Appropriate Paraphrases. Ph.D. Dissertation, Ph. D. thesis, Nara Institute of Science and Technology.

## Reviewer 4
1.	Do you think it is necessary to introduce an extra loss to force the keywords to appear in the output sentence?

In the paraphrase-mode, the keywords of target sentence are forced to appear in the output sentence. Other keywords are optimal to do like that.

We’d better analysis the coverage of keywords in the outputs, then decide if we should force keywords to appear in the outputs. There are many ways to do so and it will be an interesting new model which is different from KCN. 

2.	I think the quality of the keywords extraction part should be evaluated.

The quality of keywords is important in the Keywords Controlled Network framework, but I think it is not important in this paper.

Different strategies of keywords extraction would exert big influence on the training. However, even with the state-of-art extraction algorithms, we cannot say they are in the identical distribution as users’. Moreover, many other extraction algorithms are time-consuming and would be a bottleneck in generating training data. PositionRank & RandomSampling are the fastest one to test the KCN prototype.

In future works, I want to extract keywords and generate paraphrase jointly. I hope that end-to-end model would be better than other extraction algorithms in paraphrase generation area. I am really excited to do that, but it will be another paper which is hard to compress in this 7-page conference paper. The purpose of this paper is to show the strength of the keywords-controlled paraphrase generation.

3.	RandomSample is used for diversity enhancement. Do you filter the functional words? or any word can be included? Do you do dedup on the extracted keywords?

No, the only important trick in keywords extraction is to let them follow the order either in target sentence or in source sentence. The tricks you mentioned can do a big favor in product, but not very important in this paper, like I said in prior answer.

4.	Since the copy network is used to copy keywords to the output sentence, I am wondering, what is the coverage of the keywords copied. I think the author should conduct analysis about this.

Yes, you are right. I will do that.

5.	Why do you use the dataset paranmt50M instead of the popularly used one, such as the quora data?

The most popular one is MSCOCO. With MSCOCO, it is easy to compare KCN with others.
PARANMT50M includes a lot of natural sentences while Quora is full of questions. I want to conduct human evaluation in broader natural sentences rather than monotonous question sentences. Do more experiments is good, but we only have 7 pages. 


