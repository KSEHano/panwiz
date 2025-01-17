---
title: "Comparison of different Machine Translation Engines with BLEU"
author:
  name: Kaja Seraphina Elisa Hano
  email: khano@uos.de
date: 21st of August 2021
city: Osnabrück
degree: Bachelor of Science
area: Cognitive Science
supervisors:
- name: Dr. phil. Tobias Thelen
  email: tobias.thelen@uni-osnabrueck.de
  title: First supervisor
- name: Dr. Elia Bruni
  email: elia.bruni@uni-osnabrueck.de
  title: Second supervisor
description: |
  Many Machine translation (MT) systems exist but professional translators do not necessarily know how accurate the different systems translate. In this work, several MT systems, SDL Trados, Google, Bing, and DeepL are compared to each other. The quality of the translations from English to German and Czech are used for that. They are compared with the metric BLEU and professional translators. Google and DeepL achieve the highest scores followed by SDL Trados, Bing had the lowest scores. It is surprising that in the study with the professional translators the human translations do not get the best results. On these grounds, only a tentative recommendation for Google, DeepL, and SDL Trados can be given but only as additional help to a human translation.
acknowledgements: |
  Throughout the process of the thesis, I received great support. I thank Wietzke Consulting for providing the data and recruiting the participants.
 
  I thank my supervisor Dr. Tobias Thelen for regularly providing feedback and help during the duration of the Bachelor thesis.
  
  I also thank my family and friends for their help with the programming of the study and their insights during the editing process, especially Christopher Gundler for editing and providing me with panwiz to format this thesis.
---

# Introduction

Machine translation (MT) systems, i.e. computational systems that translate text, have been on the rise and seem to become more accurate. They are used privately and professionally to translate a variety of texts. Despite the increase in MT quality, professional human translations are still sought after. This begs the question, if MT systems can compete with professional translations particularly in specialized domains. Human and automatic quality evaluation of translations are common ways to judge individual translations and translation systems for their quality. As correct translations are often crucial, it is important to know how reliable MT systems are and if they can be used for such translations. This work will examine the quality of several MT systems compared to professional translators.

In everyday life, an MT is already often privately or professionally used to adapt to the sheer volume of content and language pairs to be translated [@book pp.1-6]. The quality of them is still not as good as a human translation but they seem to become more similar to them. When evaluating the quality of a translation human evaluation is still the standard. While it is more accurate it is time-consuming and expensive. For that reason, an automatic evaluation could augment human evaluation [@book pp. 24-27; @BetterTrans]. @BetterTrans showed that automatic evaluation can help improve MT when integrated into the development or learning process of an MT system. An automatic evaluation is very useful during the optimization process of an MT system. It is faster and less expensive than a human evaluation and can therefore be repeatedly used during the development of an MT system. Additionally, an automatic evaluation seems to be more objective than human evaluation [@book pp. 24-27]. Automatic evaluation has many advantages and strives for a high correlation with human evaluation which can still be improved [@BLEU]. Automated evaluation is an important tool for the improvement and quality assessment of an MT system.

One of the groups most interested in the development of MT is professional translators. As MT systems improve they can become part of the translation workflow. In specialized fields, MT systems could have the added difficulty to translate the right technical terms. Some of the often translated texts are manuals for machines, here correct translations are very important for the correct use of the machine. These manual texts are a specialized field with their own phrases that need to be translated according to the domain. Several MT systems can be tested to assess their quality. The two language pairs for the comparison were English-German and English-Czech. English-German is a common translation pair that is often translated and MT for this language pair could be very similar to a human translation. Czech, while still a European language with a Latin alphabet, is translated less often and can show if there is a difference between frequently and less frequently translated languages. The thesis at hand used four chapters of the manual on the use of the software of a printer as a source to compare several MT systems to each other and a professional translation.

In the following, I will present related work, including the BLEU metric as well as other metrics and an alternative to quality evaluation. After that, the two-part experiment will be presented with the computational calculation of the scores and the evaluation of translations in a study with professional translators. The results of this study will be presented and discussed as well as possible further work and limits to the presented results, followed by a conclusion.

# Related Work

Automatic translation evaluation is a useful tool that is used to judge the quality of translations in a fast and cheap way. BLEU is well established but has some challenges that other metrics try to solve. Additional to evaluation metrics there is another approach to test the quality of a translation, quality estimation. In the following, I will present BLEU, some of the challenges that BLEU does not address, some other metrics and their function, and an alternative to quality evaluation.

## BLEU

The BLEU metric is one of the well-established metrics and is often used to compare translations [@BLEU; @book pp. 24-27; @Clarity]. It is an easy to calculate and interpret score and similar enough to human evaluation to be of relevance. Although many more accurate tools have been developed since it is still a standard metric used [@book pp. 24-27; @Clarity]. Some of the reasons are that BLEU has a low computation time, it is easy to run, and it can be integrated into the code for training an MT system [@METEOR]. 

The BLEU score does not give an indication of an objectively good translation but a similarity measure between a hypothesis translation and a reference translation done by a professional translator. It works as follows, the computer is given at least one reference and compares this reference to a hypothesis, an MT of the same source segment as the reference [@BLEU]. BLEU prefers shorter sentences to avoid that problem, a brevity penalty is introduced if the hypothesis sentence is shorter than the closest reference in terms of length [@BLEU]. However, the brevity penalty can be too low, and shorter sentences achieve disproportionately higher scores even if it is implemented [@METEOR]. The other parameters are the weights of the n-grams that control the influence the n-grams have on the final score [@BLEU]. Originally BLEU could boast a relatively high positive correlation with human judgment of the translation but since the publication of the BLEU paper, a host of different metrics have emerged that give a result closer to human judgment [@METEOR; @TERp]. Further, there is the problem that BLEU scores do not always accurately reflect differences in translation quality [@Re-evaluating].

Mathematical BLEU is mainly based on the so-called precision $p = \frac{m}{t}$, $m$ being the number of matches and $t$ being the number of n-grams in the hypothesis [@recall]. The use of unigrams allows for an assessment of adequacy, that is that the words are correct. Higher number n-grams account for fluency, that is if the translation is readable as a coherent sentence. So the basic BLEU score is based on

 $p_{all} = \displaystyle\sum_{n =1}^{N} w_n \frac{m_n}{t_n}$

where $N$ is equal to 4, the biggest n-grams considered and $w_n$ are the weights for the n-grams and $w_n = \frac{1}{N}$ [@BLEU]. Using $p_{all}$ means that words can be counted several times, i.e a sentence consisting of a word with high frequency like "the" could lead to an inflated score. To avoid the overuse of high-frequency, words the maximum number of occurrences of a word in one reference is counted and the n-gram count is clipped, this is the modified precision [@BLEU]. On a corpus or several sentences, the modified precision is then the sum of the clipped n-gram matches of all sentences divided by the number of candidate n-grams. The n-gram counts are modified and all matches are added together to give the modified n-gram count:

$p_n = \frac{\displaystyle\sum_{C \in Candidates} \displaystyle\sum_{n-gram \in C} Count_{clip} (n-gram)}{\displaystyle\sum_{C' \in Candidates} \displaystyle\sum_{n-gram' \in C'} Count(n-gram')}$ [@BLEU]

The brevity penalty is done so that shorter sentences do not get an inflated score. If the length of the candidate $c$ is longer than the length of the reference $r$ that is closest to the candidate the brevity penalty is $1$ in other cases it is $e^{(1-r/c)}$ [@BLEU].

$$BP = \begin{cases} 1 & \text{if } c>r \\ e^{(1-r/c)} & \text{if } c\leq r \end{cases}$$

The final definition of BLEU is then

$BLEU = BP * \exp(\displaystyle\sum_{n=1}^{N}w_n \log{p_n})$

Because all the weights are identical this is the same as using a geometric mean [@BLEU]. This leads to a BLEU score of zero if one of the n-grams is not present.

The BLEU score calculates the precision of a translation. With the use of the modified precision and the brevity penalty, it tends to be similar to human evaluation. One of the main advantages is that BLEU needs only one, better several, reference translation(s) and the hypothesis to work and thus does not need many resources. Regardless of the existence of metrics with higher positive correlations to human judgment the few resources needed and simplicity of the calculation are reasons for the use of BLEU.

BLEU is used in different ways, as a baseline metric and also to evaluate the quality of a translation. BLEU is frequently used when introducing other metrics. Another use of BLEU is in studies that test the quality of MT systems. Here BLEU is used because it does not require many resources and is language independent. @Post used BLEU to compare a customized statistical MT (SMT) system, Microsoft Translator Hub, with a neural MT (NMT) system, DeepL. They evaluated the results with BLEU and after that compared that to human evaluation. They found out that the NMT is better than the SMT but while BLEU judges the NMT to be better it also underestimates it.

Similarly, another study examined the quality of Google Translate [@legal]. They translated legal text from English to Croatian and evaluated the quality with BLEU with up to three references. Like the study above they confirm their results with human evaluation. They conclude that BLEU has a stronger positive correlation with human judgment if it has more references. Human evaluation while more time-consuming is still valued as more accurate because it has a good inter-judge correlation, despite the high subjectivity.

 The easy use of BLEU gives us an orientation for the quality of a translation and can be applied with even little resources. One reference can be enough but when scoring a whole corpus the reference translation should be done by more than one translator to give an accurate result. Because BLEU works with references for each sentence, it is language-independent, there is only the need for a reference translation [@BLEU]. All this makes BLEU a relatively easy tool.

BLEU is used very frequently a feat considering it was introduced in 2002. Sadly the reporting of BLEU scores seems to be inconsistent as @Clarity criticizes. They present the problem that papers which use BLEU do not report any changes to BLEU. This is problematic as it does not allow for a comparison between different papers, the same score may not mean the same in different studies. They suggest improving reports of any changes and use BLEU only to compare similar systems. This brings me to the conclusion that BLEU is still a very well-used tool despite its difficulties.

## Challenges for BLEU

BLEU is an old metric, especially in the fast-changing field of computer science. It is still relevant because it is fast and easy to use. This ease of use and speed however comes at the price of its quality. BLEU has several flaws that lower the accuracy of the score. Other publications try to address those in their metrics. The problems that appear in BLEU are inaccurate scores for single sentences or segments, a reliance on exact word matches, and no recall.

Firstly BLEU has inaccurate scores for single segments [@METEOR]. The BLEU score is calculated via a geometric mean that leads to the problem that often scores are 0 if one of the n-grams is 0. This problem can be avoided by changing the weights. The problem is then that the weights can not be freely manipulated to have a strong correlation with human judgment. The score should not be so strongly influenced by the absence of an n-gram. A more accurate score for single segments would be useful in the fine-tuning of MT systems [@METEOR]. Inaccurate scores also lead to the problem that translations are scored the same despite being qualitatively very different [@Re-evaluating]. Therefore automatic translation evaluation could profit from a more accurate sentence score. Solving the following problems can also help with the accuracy of the score for single sentences.

Secondly, BLEU needs exact matches. The BLEU metric does not consider synonyms and words with the same stem as a match. That is a problem as adequate alternative translations in terms of synonyms are not always considered to be good translations [@METEOR]. There are infinitely many correct translations. Not recognizing alternative translations leads to the fact that many correct translations are not recognized as such [@TERp p.118]. To alleviate this problem BLEU can use several references but even with those only a small subset of all possible good translations can be considered by BLEU. @recall even suggest that stemming reference and hypothesis already improves the accuracy of a metric. A metric could profit by matching synonyms and different word forms even if that has a higher computational cost for matching words and additional resources are needed in that process.

Finally, BLEU does not use recall. *Recall* is the number of matches divided by the number of words or n-grams in the *reference* [@recall]. The main calculation in BLEU is the so-called *precision* which is the number of matched n-grams divided by the total number of words in the *hypothesis* [@BLEU]. The reason BLEU uses precision instead of recall is that recall has to work with the reference which matches the hypothesis the best. BLEU can combine the matches of several references and combine those to calculate the scores. BLEU includes a brevity penalty to avoid that shorter translations are always preferred. However, the results in @recall suggest that the penalty is not enough to get results with a high correlation with human judgment. Considering that study, recall seems to be a powerful tool to achieve correlation with human judgment. It should be a part of a metric and heavily weighted. Recall could be a tool to improve the correlation but it must be considered how to include it if one wants to pick matches from several references to further improve the accuracy of the scores.

The factor of missing recall and the need for exact matches lead to the problem of a score that has no accurate single sentence scores. An improvement in these areas can ultimately lead to a score that is closer to human evaluation.

## Alternatives to BLEU

Considering the challenges of BLEU, several alternative metrics have emerged to tackle them. Here I include three other well-established metrics that are very similar to BLEU and often used and compared to it [@book, p. 26; @QE, p. 44]. NIST [@NIST], METEOR [@METEOR], and TER [@TER] including the two variants of TER HTER and TER-Plus [@TERp] are further explained here. NIST is most similar to BLEU, METEOR includes several measures to improve on BLEU and TER is the most distinct of the three.

@NIST has developed the evaluation metric that resembles BLEU the most, it is called NIST. Like all the automatic evaluation methods NIST requires at least one reference translation. Further, it compares the different n-grams of the reference or references to the hypothesis and gives the whole segment a score. NIST improves on BLEU by including information weights, i.e. that n-grams with a lower chance of co-occurrence are weighted more heavily than those with a high chance of co-occurrence [@NIST]. With those improvements, NIST has a slightly higher positive correlation with human judgment in terms of recognizing adequacy but not fluency [@NIST]. NIST is very similar to BLEU and improves slightly on it in terms of positive correlation with human judgment.

METEOR relies on a human-made reference translation to score the MT like BLEU and NIST. One of the big problems in BLEU is that the words have to be *exactly* the same to be considered a match, as mentioned above. METEOR solves this problem, it can match synonyms as well as word stems with additional resources [@METEOR]. This helps to give a more accurate score as it better considers alternative translations as adequate. Another change in comparison to BLEU is that METEOR supports recall and has three tuneable parameters [@METEOR]. The three parameters can be tuned to better correlate with human judgment depending on the MT system given. The first parameter controls the weights for precision and recall, the second the shape of the fragmentation penalty function, a penalty for different word order, and the third the weights of the penalty [@METEOR]. These parameters help to tune the metric to the system it is evaluating and give a score that better correlates with human judgment. METEOR considers synonyms and word stems, including recall, and has three tunable parameters to get a stronger positive correlation with human judgment.

Other than the previous two the TER score does not measure how different a translation is from a reference translation, instead, it measures how many edits have to be performed to change an MT into the closest human reference [@TER]. Hence the name *translation edit rate* or TER. Edits include deletions, insertions, substitutions, and shifts. To reduce the processing time to find the minimal number of edits for a given segment the algorithm first calculates the shift, which reduces the number of edits the most with a greedy search algorithm and then the other edits [@TER]. This is the basic algorithm but there are two versions of it.

The first variant is the *human-targeted translation edit rate* or HTER. In addition to the references of human translations, there is a human-targeted reference. This means that human annotators, who are fluent in the target language, are given the MT and the references. The annotators edit an MT in such a way that it is an adequate translation [@TER]. The edited MT translation is then used as another reference to calculate the score. This can be used when working on an MT system as the translation does not change that much with every change in the MT system. If they do change, the annotators can work on the segments that changed again. The most pressing problem here is that the annotators have to work on each sentence. This is time and cost-intensive as they need three to seven minutes per sentence [@TER]. So while HTER is an improvement on TER in terms of positive correlation between human and automatic evaluation it is quite work-intensive and expensive.

The second variation of TER is TER-Plus or TERp. It works like TER with at least one reference and then calculates the optimal edits. Like METEOR it considers recall, synonyms, and word stems in the calculation of the editing rate. Other than the basic TER algorithm all edits have different costs, i.e. shifts, substitutions, deletions, and insertions do not have all the same cost of 1 as they have in TER [@TERp]. These parameters for cost are optimized to positively correlate with human judgment on a specific system with the help of a hill-climbing algorithm. The cost can be even further specified to certain word groups so that TERp can get an even higher positive correlation with human judgment [@TERp].

TER is a little bit different from the other metrics in that it calculates the edit rate and not the similarities or matches between hypothesis and reference. The variations of TER, HTER, and TERp, try to improve on TER to give a more accurate editing cost. They are both more costly than TER because they rely on more resources. HTER is especially costly as it requires human labor. TER and its variants are closer to human evaluation but a little more costly than BLEU.

Several aspects of BLEU have room for improvement to get a score closer to human evaluations and the metrics above tackle those. This leads to a higher positive correlation with human evaluation. I mentioned NIST, METEOR, and TER as some of the well-known alternatives. NIST is not much of an improvement compared to BLEU, and METEOR, as well as TER, need more resources than BLEU. In the end, BLEU is still a metric that is often used to compare to other metrics as a baseline, or to evaluate MT systems.

| **Metric<br />** | **Human Reference<br />**   | **Synonyms <br />and Stems** | **Tunability/ <br />Preciseness** | **Recall<br />** |
| ---------------- | --------------------------- | ---------------------------- | --------------------------------- | ---------------- |
| BLEU             | Yes                         | No                           | n-gram weights                    | No               |
| NIST             | Yes                         | No                           | information gain                  | No               |
| METEOR           | Yes                         | Yes                          | three tuning parameters           | Yes              |
| TER              | Yes                         | No                           | -                                 | No               |
| HTER             | Yes (plus edited reference) | No                           | -                                 | No               |
| TERp             | Yes                         | Yes                          | different editing cost            | Yes              |

Table: **All metrics in comparison** {#tbl:Metrics}

## Challenges for Metrics

Metrics like BLEU and its alternatives can have systematic errors. @Re-examining used an algorithm based on MT metrics to detect paraphrases. They discover some common mistakes that lead to the misclassification of pairs of sentences as paraphrases. This is similar to evaluating if a translation is good compared to its reference.

The first source for mistakes is *misleading lexical overlap* as some sentences have a big overlap but some words are changed leading to a completely different meaning [@Re-examining]. This is problematic because, for example, BLEU would give two translations that differ only in one word from the reference translation the same score even if the word difference changes the meaning of the whole sentence. A different word choice can be scored the same as a wrong translation especially if there is no resource for stemming or synonyms like in BLEU, NIST, or TER, and synonyms and different word forms are not matched.

The next source of mistakes stems from a *lack of world knowledge* if not given the appropriate knowledge MT systems can not know if something is the same like a professional position and the name of the person filling it [@Re-examining]. This is not much of a problem for MT systems in general as they seem to translate very closely to the source. More of a problem are synonyms, this source of mistakes is reduced by TERp and METEOR, which consider synonyms. In those cases, it would be important to have a domain-specific resource, because not all synonyms are appropriate in every domain.

The mistakes above can lead to systematic errors and inappropriate scoring for different sentences. Qualitative different sentences can be scored the same, and sentences that have the same quality could be scored differently.

## Quality Estimation

Besides these different quality evaluation metrics, there is another field for translation quality control that is *quality estimation*. Quality estimation has the goal to give an estimation of any given sentence without a reference translation [@QE]. Other than the metrics it is supposed to have a more accurate estimation on a sentence level. It needs of course some reference point to make a judgment on the quality of a segment. Most quality estimation is done as a supervised learning task that needs to be trained on a high number of sentence pairs in the source and target language with a human evaluation of their quality [@QE]. One of the more well-known quality estimation tools is QuEst [@Quest]. Some of the important uses of QuEst are: giving an estimation of the editing time, selecting the best translation of some alternatives, deciding if a segment needs revising, and estimating if the segment can be used for the training of an MT system [@Quest]. QuEst needs a lot of different resources, other than the metrics that need only the reference translations and possibly a resource for stem and synonym matching. The most important required resources are a big training corpus with human annotation of the quality of the translation and language models for source and target language.

There are two parts of the QuEst algorithm: the feature extractor that finds relevant features for the quality estimation and the machine learning part. The feature extractor finds MT system independent "black-box" features that try to measure complexity, fluency, and adequacy. "Glass-box" models show the confidence of a certain MT system. The machine learning part of the algorithm then uses the extracted features to build the quality estimation by tuning hyper-parameters and selecting relevant features. The finished build is then specific to that language pair and domain of the training corpus. Quality estimation like QuEst can provide a stronger positive correlation to human judgment than the metrics [@QE]. The biggest disadvantage of quality estimation seems to be the need for many resources and a complicated code. Other than the metrics that provide results fast and with minimal resources QuEst and other quality estimation algorithms that have to extract the features and train a model are no easy solutions and seem only worth it if the domain and language pair are used frequently.

# Method

For the evaluation of the quality of several MT engines, I set up a two-part experiment. The first part of the experiment was fully computational. Source sentences in English were translated into the target language German or Czech with different MT engines. Then I calculated the BLEU score for those translations and compared the different translations to each other. The second part of the experiment brought in a human component. Several professional translators evaluated the quality of a selection of the translations. Both parts are then used to evaluate the quality of the translation with the automatic and manual quality evaluation. The data and the experiment are explained in this part.

## Data

The data consisted of the translations of four chapters from a printer manual. It was relatively new and the selected chapter concerned the software of the printer and fax machine. The original English and the professional translation into German. For Czech, a shorter version of the manual was used. To protect the customer's data all references to the company and their products were anonymized and formatting tags in the data were removed. The data was separated into segments with a minimum length of one word. The separation was an artefact from the previous translation work. In the end, the German version consisted of 3,767 segments including repetitions. It was separated into four parts of different lengths according to the original chapters with 1,606, 401, 378, and 1,382 segments respectively. There were fewer Czech segments as there was only a shortened translation available. In total the Czech translation contained 2,165 segments separated again into parts with 1,273, 127, 215, and 550 segments. The English original was then used as a source for the MT into each language and the human translation was used as a reference to compare the MT too.

Table @tbl:Dataexample_DE and table @tbl:Dataexample_CZ show two examples of the data used in the experiment. The first example in both is one of many segments that were only one or two words long. Here it is apparent that the MT systems translated the segment literally and not with the right technical term for this context, see human translation. It is questionable, if the context of a whole sentence would have sufficed to give a correct translation. The translations of whole sentences show that the MT systems translate very literally. In the second example are the translations of the same sentence into German and Czech. Here it is again apparent that the translations by the MT systems are similar or even identical to each other while the Human translation is quite different. While the English source and the more word-for-word translations are passive the human translations into German and Czech talk explicitly to the reader. This is possibly a convention for the writing of manuals that differs in different languages and possibly even localisations. MT systems have translations that are similar to each other but differ from human translation because they seem to translate more literally. This gives the translators two challenges, to use the appropriate terms and to translate in an appropriate style for the language, localization, and domain.



| **Translator** | **Example 1**   | **Example 2**                                                |
| -------------- | --------------- | ------------------------------------------------------------ |
| **Source**     | Slit Glass      | It is possible to check the status of the system and network, the status of consumables such as toner and paper and the situation of the options being used. |
| **Human**      | Belichtungsglas | So können Sie den Status des Systems und des Netzwerks, den Status der Verbrauchsmaterialien wie Toner und Papier sowie den Zustand der verwendeten Optionen überprüfen. |
| **SDL Trados** | Schlitzglas     | Es ist möglich, den Status des Systems und des Netzwerks, den Status von Verbrauchsmaterialien wie Toner und Papier sowie die Situation der verwendeten Optionen zu überprüfen. |
| **Google**     | Schlitzglas     | Es ist möglich, den Status des Systems und des Netzwerks, den Status von Verbrauchsmaterialien wie Toner und Papier sowie die Situation der verwendeten Optionen zu überprüfen. |
| **DeepL**      | Spaltglas       | Es ist möglich, den Status des Systems und des Netzwerks, den Status der Verbrauchsmaterialien wie Toner und Papier und die Situation der verwendeten Optionen zu überprüfen. |
| **Bing**       | Schlitzglas     | Es ist möglich, den Status des Systems und des Netzes, den Status von Verbrauchsmaterialien wie Toner und Papier und die Situation der verwendeten Optionen zu überprüfen. |

Table: **Example Segments German** {#tbl:Dataexample_DE}

| **Translator** | **Example 1**      | **Example 2**                                                |
| -------------- | ------------------ | ------------------------------------------------------------ |
| **Source**     | Slit Glass         | It is possible to check the status of the system and network, the status of consumables such as toner and paper and the situation of the options being used. |
| **Human**      | Skenovací štěrbina | Můžete zde zkontrolovat stav systému a sítě, stav spotřebního materiálu jako toner a papír a přehled o požívaném volitelném příslušenství. |
| **SDL Trados** | Proříznuté sklo    | Je možné zkontrolovat stav systému a sítě, stav spotřebního materiálu, jako je toner a papír, a situaci používaných možností. |
| **Google**     | Štěrbinové sklo    | Je možné zkontrolovat stav systému a sítě, stav spotřebního materiálu, jako je toner a papír, a stav použitých doplňků. |
| **DeepL**      | Štěrbina Sklo      | Je možné zkontrolovat stav systému a sítě, stav spotřebního materiálu, jako je toner a papír, a situaci používaných možností. |

Table: **Example Segments Czech** {#tbl:Dataexample_CZ}

## Machine Translators

The MT systems were all freely available for the study. Bing and Google are both freely accessible online translator that translate short texts, Google even translates whole documents [@Google; @Bing]. Because Google is freely available and supports a high number of languages it is often used in studies [@legal]. DeepL is a freely available translator that only covers a few languages, when the study was done Czech had not been available, which has changed ever since. It can translate short segments but also documents, there is a plug-in for the SDL Trados software is used [@DeepL]. SDL Trados is a computer-assisted translation (CAT) tool that helps to ease the translation workflow. It includes a machine translation tool the "SDL Language Cloud" [@SDL_NMT]. All of these translators are NMT systems, i.e. a neural network for translations, which is the new standard [@Post; @Google; @diffNMT; @SDL_NMT; @Bing_NMT].

For the translation into German the MT systems of SDL Trados, DeepL, Google document translation, and the Bing translator were used. The same was used for the Czech translation except for DeepL, as at the time of the experiment there was no possibility to translate into Czech with DeepL. The data then was entered in the translators and translated which took a few minutes at most.

The translators were accessed in different ways. SDL Trados is not only a translator but mainly a CAT tool. It allows the users to translate without worrying about formatting. SDL Trados has the option for an MT tool, the SDL Trados Language Cloud [@SDL_NMT]. It is also possible to use a plugin for DeepL, another MT system. Both are usually used to make translations that are later edited. Google translate and Bing translator are both online MT systems provided by Google and Microsoft respectively that both companies also use in their other products [@Google_NMT; @Bing_NMT]. Except for the Bing translator, all translations were done on the whole documents. The Microsoft translator for documents requires a self-programmed base that had still several problems that could not be solved for this thesis. For that reason, the sentences had to be translated in chunks of 5000 characters for German or 1000 Characters for Czech. All MT systems were freely accessible for the study. On a first look, the translations seemed very similar to each other. All the translators provided translations for the sources without problems and were easily handled.

## BLEU

BLEU uses the comparison between the new translation and a reference translation to calculate the score. It is a part of the nltk.translate package [@BLEU; @nltk]. Any change of capitalization can be the difference between a score of 0 and 1, 1 being an identical translation and 0 a translation with no similarity due to the need for exact matches. Therefore, the data had to be prepared before it was scored.

First I replaced every "-" with a space as that was not a significant translation difference. After that, I stripped all the punctuation and parentheses as BLEU does not work with these. Next, I converted everything to lower case. The problem of not matching words because of different but false capitalization was higher than wrongly identifying a word as matching if they had differences only in capitalization. One example was "auf Seite" and "Auf Seite": there the unigram score would be 0.5 instead of 1 simply because of the capitalization. With the preparation done the segments could be scored.

The main tuning parameters for BLEU are the weights. The weights influence the importance of the n-grams. Usually the n-grams from n = 1 to n = 4 are considered. Without tuning the n-grams are weighted equally and result in a geometric mean. This did not lead to any satisfactory results, shorter segments of one or two words were scored as 0 even if the translation was identical to the reference, this was especially problematic because of the high number of single or two-word translations. Using a geometric mean could lead to an inaccurate segment and overall score because short segments are not counted correctly. To get a more adequate result the weights were changed. A high weight for unigrams was most important but other n-grams had some weight so that they would be considered. In the end, the weights were as follows (0.7, 0.15, 0.075, 0.075) in order from unigram to 4-gram. The new weights ensure that it is not a geometric mean anymore and scores are calculated even if segments are shorter. The high weight for unigrams gives more importance to adequacy than to fluency, the use of the right words was weighted stronger than their order. This way the results had a value different from 0 more often, even for the segments with less than four words.

The BLEU score is calculated with different sets of references. Once with only the human translation as a reference. Twice with all translations as reference excluding the translation that was the reference. One version where the human translation was included as a hypothesis and one where it was exclusively used as a reference. There was a score calculated for every segment of every translation and one for each chapter. It was also calculated which translation ranked best on each segment and on average. The results show the BLEU score the different MT systems and the human translation achieve depending on the references.

## Human Evaluation

BLEU gave a first impression of the quality of the MT but it compares the translations to reference translations and I had only one verified reference translation, the human translation. It was important to see if translators rate the MT similarly to the BLEU score and test what kind of correlation the BLEU score has with human evaluation. In the following, I describe the study with the translators.

### Data

The number of sentences was very high, to keep the time to do the study to an acceptable length for the participants this number had to be reduced. First, it was necessary to filter out adequate sentences for the study. They had to be complete sentences, different for each MT system, and there should not be any source sentences more than once. The source sentences had to have a length between 6 and 15 words. The lower bound allowed to filter out most nonsentences. @SentenceLength showed that 17 to 18-year-olds could parse sentences with up to 15 words with ease, it also suggested that better readers could parse even longer sentences with relative ease but it seemed like a good cut of point to keep the length of the sentences manageable and time per sentence shorter. In the next step sentences that had identical translations in different translators were excluded. The sentence also had to have at least one score that differed from the others to guarantee diverse sentences. From that set of sentences, 60 sentences were randomly sampled. Sentences with missing words and repeated sentences were manually excluded. Finally, the first 50 sentences of that final set were used in the study. All these steps reduced the sentences to a diverse and adequate sample of all the sentences.

### Experiment

The experiment was done by the participants remotely and without supervision. Each participant received an e-mail with a link to the study. They were then given 50 source sentences with five translations for German and four for Czech this included all MT as well as the human translation. In each question, they were given the source sentence and all translations. They were then asked to rate the quality of the translation on a 5 point Likert scale from "strongly agree" to "strongly disagree", figure @fig:screenshot shows an example of this. At the beginning of the study, they were instructed to select "strongly agree" if the translation was perfect and "strongly disagree" if the sentence needed a complete new translation. These sentences were presented to the participants in a random order different for each participant the order of the translators was randomized for each question and participant. Following these questions where the general questions, about demographics and the personal use of MT in their normal translation process. Such as the frequency and extent of use of MTs. The expected time for answering the questions was about 40 Minutes in both languages. 

The general questions asked for the typical age and gender and specific to the study also native language and language pairs they translate. They were also questioned on their personal use of MT systems. They were asked what kind of systems they used. Generic, ones that translate text without any further information and do not change, adaptive, that can change with new input, customizable, that is changed to a specific language pair and domain, other or no MT system at all. The next question was when they use it in the translation process, in pre or post-editing, during the translation, or never. Another question was what kind of CAT tools were used, SDL Trados, memoQ, Wordfast, others, or none. The second to last question was how often they use MT systems to translate words, phrases, sentences, or documents on a scale from never to always. The final question asked how they would rate the quality of MT they normally encounter.

For the evaluation of the main part, the average was calculated overall candidates for each question and then the average for each translator was calculated. The BLEU score was newly calculated on the study questions with the human translation as a reference and all possible translations including the human translation as hypotheses. This was important to be able to calculate the Spearman correlation between the human and machine evaluation. The Spearman correlation coefficient was chosen as a measure because this correlation shows a monotonous correlation between two variables that are not necessarily bivariate nor have a normal distribution as the data were in this experiment [@Spearman]. 

![**Example Question**](images/screenshot.png){#fig:screenshot}

### Participants

The participants were all professional translators, most of them have worked for Wiezeke Consulting in the past. Wiezeke Consulting recruited them from all available translators they know for those language pairs. There were five translators per language pair. The translators were all women for German, plus one unidentified person, ages 51 to 60, and three women and two men for Czech ages 31 to 40 except for one person who was 18 to 30 years old. I had to exclude three datasets from the Czech study because they were only partly done. One German participant did not answer the demographic questions but was still included because they answered all the translation questions. All translators were native in the target languages and translated professionally from English to German or English to Czech.

### Usage of MT Systems

The questions in the study following the questions on translations were on the personal use of MT systems. For the participants of the Czech study, only one responded to not use any MT engine but later responded that they translate words, phrases, and sentences with MT systems. The others all use adaptable MT systems and two use also a customizable MT and one of them additionally uses a generic MT system. The MT is used during the translation process and/or during the post-editing. They all use some MT systems not listed but SDL Trados and memoQ are also used by four and three people respectively, at least four of them use more than one CAT tool. The MT systems were used to translate words either never, three participants, rarely, one, or sometimes, one. To translate phrases they were used never, two, rarely, one, and sometimes, two. Sentences were translated with an MT system rarely, two, sometimes, often, and always, each one. Whole documents were never translated with an MT system, except for one participant who used it always. The quality of MT systems was judged by three people to be good or better while one person judged it as neither good nor bad and one judged it as bad.

For the German version of the study, the results to these questions were only given by four people the participant was still included for the other questions as they answered all other 50 questions. MT systems were only used by one participant and she used a generic one. This time the answers agree with the other answers on what they translate with the MT system. MT systems are used by two people for post-editing and never by the other two. SDL Trados and at least one other tool are used by all participants, memoQ by two and Wordfast by one person. Only phrases and sentences are sometimes translated but only one person does that all others never translate anything with an MT system, no one translates words or whole documents with an MT system. The quality of MT is mostly judged as neither good nor bad and one person judges it as bad.

# Results

Bellow the BLEU scores, results of the study, and correlation are presented the results are all tested for significance with $\alpha = 0.1$. Significant values are marked with a "*" in the figures and bold in the tables. For all cases this means that they are significantly different to all other results, significant differences to not all other results are marked.

For the computational part, the different MT systems and human results were compared to each other with a pairwise two-tied t-test, comparing every result for the translation to every other translation. The data for the t-test were the results for each segment for the BLEU score or rank. The results presented for the computational part are the average ranks and the average of the chapter BLEU score.

For the study, the results were first averaged over the participants for each translator and question and a pairwise two-tied t-test was calculated on those results. The results presented are the average rank for each translator and the BLEU score calculated on the whole set of sentences. The Spearman correlation was calculated with all the average responses of all the participants to each translation and the BLEU score for each translation.

In this section, I will first present the results of the BLEU study, then the results of the Translator study, and finally, compare the results to each other.

## BLEU

For the BLEU score, a higher number is better while for the ranks a low number is. On a first look, the numbers are all close together in the same area but there are some significant differences between the results. The score presented for BLEU is the average of the corpus scores for all chapters.

For the Czech translations, all differences are significant as seen in figure @fig:CZ_BLEU. In each version, Google gives the best translation followed by SDL Trados and finally Bing. It is obvious to see that difference between SDL Trados and Bing is smaller than the one between SDL Trados and Google. In the comparison between all the MTs and the human translation, it is interesting to see that the score for the human translation is the lowest.

In the translations to German, figure @fig:DE_BLEU, it looks a little different Bing still has the worst results with a significant difference to the other results. When I only used a human reference DeepL was the best translator but had no significant difference to Google or SDL Trados. In the two one against all versions, it is Google that ranks the highest with a significant difference to SDL Trados and DeepL that have no significant difference to each other. Bing has always a significantly lower score than the other MT systems.

![**Czech BLEU scores**: "*" mark significance](images/CZ_BLEU.png){#fig:CZ_BLEU}



![**German BLEU scores**: "*" mark significance](images/DE_BLEU.png){#fig:DE_BLEU}



| **Translators** | **Bleu**  | **Rank**  |
| --------------- | --------- | --------- |
| google          | **0,509** | **1,436** |
| SDL Trados      | **0,45**  | **1,634** |
| Bing            | **0,444** | **1,693** |

: **Czech reference is human**: {#tbl:Cz_h} Average of the corpus BLEU calculated on each chapter and average rank, significant differences are bold

\newpage

| **Translators** | **Bleu**  | **Rank**  |
| --------------- | --------- | --------- |
| google          | **0,818** | **1,479** |
| SDL Trados      | **0,746** | **1,695** |
| Bing            | **0,751** | **1,698** |

: **Czech one against all**: {#tbl:Cz_1aa} Average of the corpus BLEU calculated on each chapter and average rank, significant differences are bold, the references are the human translation and all other translations

| **Translators** | **Bleu**  | **Rank**  |
| --------------- | --------- | --------- |
| google          | **0,818** | **1,61**  |
| SDL Trados      | **0,746** | **1,878** |
| Bing            | **0,751** | **1,913** |
| human           | **0,593** | **2,635** |

: **Czech one against all with human hypothesis**: {#tbl:Cz_1aa_h} Average of the corpus BLEU calculated on each chapter and average rank, significant differences are bold, the references are all other translations

| **Translators** | **Bleu** | **Rank**  |
| --------------- | -------- | --------- |
| DeepL           | 0,432    | 1,573     |
| google          | 0,41     | 1,649     |
| SDL Trados      | 0,401    | **1,747** |
| Bing            | **0,36** | **2,003** |

: **German human reference**: {#tbl:De_h} Average of the corpus BLEU calculated on each chapter and average rank, significant differences are bold

| **Translators** | **Bleu**  | **Rank**  |
| --------------- | --------- | --------- |
| google          | **0,875** | **1,56**  |
| SDL Trados      | 0,841     | 1,86      |
| DeepL           | 0,839     | 1,87      |
| Bing            | **0,776** | **2,025** |

: **German one against all**: {#tbl:DE_1aa} Average of the corpus BLEU calculated on each chapter and average rank, significant differences are bold, the references are all other translations including the human translation

| **Translators** | **Bleu**  | **Rank**  |
| --------------- | --------- | --------- |
| google          | **0,875** | **1,64**  |
| SDL Trados      | 0,841     | 1,957     |
| DeepL           | 0,839     | 1,948     |
| Bing            | **0,776** | **2,203** |
| human           | **0,494** | **3,79**  |

: **German one against all with human hypothesis**: {#tbl:De_1aa_h} Average of the corpus BLEU calculated on each chapter and average rank, significant differences are bold, the references are all other translations

## Evaluation Study

The human judgments, figure @fig:studyresults, show that the participants judged Google and human translations into Czech to be the best. However, there is no significant difference between any of the translators. They are significantly different in terms of ranks.

It looks a little different for the German study. The ranks again were all significantly different from each other. DeepL was judged to be the best without a significant difference to the second-best Google. The human and SDL Trados translations have significantly lower scores than DeepL. Bing has the lowest score with a significant difference to all other translators.

The scores overall are very mediocre on a 1 to 5 scale they are all below a four.

![**Study Results**: "*" mark significance, lines indicate if they are only significantly different to one other result](images/StudyResults.png){#fig:studyresults}



| **Translator** | **Results** | **Rank** |
| -------------- | ----------- | -------- |
| google         | 3,57        | **1,9**  |
| human          | 3,406       | **2,34** |
| SDL Trados     | 3,223       | **2,62** |
| Bing           | 3,202       | **2,64** |

: **Study results Czech**: {#tbl:Cz_study} significant differences are bold

| **Translator** | **Result** | **Rank** |
| -------------- | ---------- | -------- |
| DeepL          | 3,42       | **2,08** |
| google         | 3,164      | **2,6**  |
| human          | *2,936* *  | **2,96** |
| SDL Trados     | *2,944* *  | **2,98** |
| Bing           | **2,531**  | **3,42** |

: **Study results German**: {#tbl:De_study} significant differences are bold, * are only significantly different to first place

## Comparison between Computational and Human Evaluation

Table @tbl:Cz_comparison and @tbl:De_comparison show the results of the human scores as well as the BLEU scores done on the whole corpus of question in one with the human translation as reference. The BLEU score for the human translation is the highest with a big difference to the other translations.

The Czech results show that the order of human translation and Google, as well as Bing and SDL Trados, are switched. For the translations into German, the human translation jumps to the front with the highest BLEU score the order of the remaining translators stays the same.



| **Translator** | **Human** | **Corpus Bleu** |
| -------------- | --------- | --------------- |
| human          | 3,406     | 0,955           |
| google         | 3,570     | 0,489           |
| Bing           | 3,202     | 0,416           |
| SDL Trados     | 3,222     | 0,402           |

: **Study results Czech in comparison to BLEU scores on all study questions**: {#tbl:Cz_comparison}

| **translator** | **human** | **Corpus Bleu** |
| -------------- | --------- | --------------- |
| human          | 2,936     | 0,917           |
| DeepL          | 3,42      | 0,329           |
| google         | 3,164     | 0,317           |
| SDL Trados     | 2,944     | 0,303           |
| Bing           | 2,531     | 0,287           |

: **Study results German in comparison to BLEU scores on all study questions**: {#tbl:De_comparison}

### Correlation

The Spearman correlation between the human score and the BLEU score is rather small for Czech and German. Czech has a significant result for the correlation between the values of the study, table @tbl:Cz_corr and BLEU as well as of the rankings but the correlation is very low positive, negligible low for the values.

The correlation between the human judgment is even lower in the German study, table @tbl:De_corr. The correlation between the values is not significant. Between the ranks, the correlation is significant but negligible negative.

|           | **Correlation** |
| --------- | --------------- |
| **value** | **0,277**       |
| **rank**  | **0,379**       |

: **Correlation between study results and BLEU scores for Czech**: {#tbl:Cz_corr}

|           | **correlation** |
| --------- | --------------- |
| **value** | 0,063           |
| **rank**  | **-0,121**      |

: **Correlation between study results and BLEU scores for German**: {#tbl:De_corr}

# Discussion

For the small group of translators, it seems that the Czech translators use MT systems more often in their work and rate the quality as better compared to their German counterparts. This is surprising in so far as that German is the more commonly translated language by MT systems. The reason for that could be the frequency of use by the translators. The Czech participants used MT systems with a higher frequency than the German participants and could be more used to an MT. It could also be that the translation into Czech is better than into German, or the differences between the languages could influence the results. To find out if there is a causal relationship between translator use and quality or perceived quality, a completely different study would be needed. Despite the German translator's bad opinion, they did not judge the human translation to be the best and significantly lower than DeepL.

The results are not directly comparable to other studies because the weights were changed. That is a common problem in the reporting of the BLEU results. Sometimes it is not clear which changes have been made to the code which leads to the problem that there is no one single BLEU score and results can be misleading [@Clarity]. The results can only give a ranking of the translations, not a comparable result. Further, because of the changed weights, unigrams have more influence on the score. This leads to a heavier emphasis on adequacy but less on fluency which BLEU accounts for by using the longer n-grams. That means that the words might be translated correctly but not in the right order.

In the results for one against all with the human as a hypothesis, the score for the human translation is significantly lower than the other translations. The reason for that is not a lower quality of the human translation but its differences to the MT translations. As BLEU tells us how similar a hypothesis is to its references. This means that the MT are more similar to each other than to the human translation. This explains the low score for the human translation and the high score of the MT systems when the other MTs are used as additional references.

For the BLEU score of the study sentences, the translations were scored against the human translation as references, even the human translation had only the identical human translation as a reference. This leads to a very high BLEU score. Surprisingly, the score is not 1 which should be expected. When looking into the single sentence scores, the segments are mostly scored as 1 but some are a little lower than 1. A reason could be a mistake in the production of the references and hypotheses or it could be influenced by the weights.

The result of the study was surprising especially the sores for the human translations. Other than expected the human score was not the best and even significantly worse than the best result in the German study. It was expected that the human translation as the gold standard would be significantly better than the MTs. A reason for the surprisingly bad result for the human translation could be that it was too different from the source. While they could in context be correct translations and the way it should be translated, the translators could have judged the translation as too different from the source. Other than the MTs that are more word-for-word translations. The results of the one against all with a human hypothesis scores suggest that the human translation is rather different from the other translations.

 There is not much correlation, it is unclear why that is the case. There are not enough data for a definite result even if the results are significant a bigger sample set could give results that are more telling. After all human evaluation is still the defining factor when judging the quality of an MT system. Other papers suggest that BLEU positively correlates with human judgment [@BLEU]. The results here might be screwed by the use of the human translation as a reference and hypothesis in the BLEU score. @BLEU for example uses three MT systems and two human translations in their study with human participants. Although, the order of the MT ranking stays the same disregarding the human translation in the German translation for the study results and the BLEU score the correlation is negligible. The overall score for all translations is, with under 4 on a 1 to 5 scale, rather mediocre.

Disregarding the human translation, for the Czech translation, the ranking order of the two worst is swapped between the BLEU score and the study results, however, there is no significant difference between the two. For the German study the order from highest to worst score stays the same. Considering the BLEU score with only a human reference, table @tbl:Cz_h and table @tbl:De_h, the order of the MT systems is the same as in the study, for both languages. This lends some credibility to the BLEU scores even if the results of the study have less significant differences.

For the MT systems, Google is the best with a significant difference in nearly all cases. Only in German is Deep L ranked the best, in both the BLEU score with only a human reference and the study, but without a significant difference to Google. SDL Trados and DeepL are often close to each other with no significant difference between them except in the cases where DeepL is the best translator. This could indicate that those two translators are similar to each other. Bing is the MT system that is continuously judged to have the lowest score. Except for the BLEU score of the Czech study questions where SDL Trados has the lowest score.

Improvements to the experiment could be made. Having at least one other human translation as a reference or hypothesis could make the BLEU score more accurate and improve the correlation with human judgment. It is also possible that despite the wide use of the BLEU score it does not correlate well with the judgment of humans when confronted with NMT, as shown in a study BLEU tends to underestimate the quality of NMTs [@Post]. Another metric like TERp or METEOR could be better for the evaluation of the translations as they also consider synonyms and stems. It would also be interesting to compare these MT systems to customized MT systems that are specialized to the domain.

A further interesting question to consider is a change to the study. Firstly it could be interesting to see how translations are scored without a source sentence. Secondly, more context could be given to the participant, not asking them to rate single sentences but instead short paragraphs for example to give more context. The sample set for the different sentences could be also bigger either by giving participants longer questionnaires or giving them different sentences to evaluate.

Finally, quality estimation could be an alternative to metrics when testing the quality of an MT system. It could give results without the need for reference translations and possible ones that correlate better with human judgment. This would necessitate more time for the training of the tool and a big training set. The required data would be different from the data used in this work which is another hurdle for the use of quality estimation.

# Conclusion

In the field of translation evaluation, there are several different translation evaluation metrics. The well-established ones are BLEU, NIST, METEOR, and TER. They all compare a hypothesis, a translation, to at least one reference translation and give a score that shows how similar the hypothesis is to the reference. They all have different advantages and disadvantages. One important aspect is how they calculate the score and if the calculation is based on precision, matching n-grams per n-grams of the hypothesis, or recall, matching n-grams per n-grams in one of the references. Another important aspect is the recognition of synonyms, most metrics can not match synonyms but instead require exact matches. Only METEOR and TERp a variant of TER can recognize and match synonyms but require additional resources. In general, the metrics do not require many resources, mainly a reference translation, and are therefore fast and language independent. BLEU is very well-established and a very fast and easy-to-use metric.

The computational experiment showed that there are differences between most of the MT systems, sometimes the differences were not significant. The comparison between the MT systems and human translation showed that the BLEU score is a similarity measure and shows only how similar a translation is to another one. This is apparent in the low score for the human translation when the references are MT and the increase of the scores for the MT when other MTs were included in the references.

The human evaluation showed not many significant differences between the different translations. The sample size for the data was relatively small and no definite answer to which the best MT system is or how good the correlation between BLEU score and human evaluation can be given. Other than expected, the human translation is not significantly better than the other translations. The reasons for that are unclear. However, Google and DeepL reach repeatedly high scores followed by SDL Trados while Bing has with one exception consistently and significantly the lowest score.

It can tentatively be said that Google and DeepL are the best MT systems, although, SDL Trados is close behind. Bing seems to be a bad choice as a translator, although it should be said that it had the disadvantage to not be able to translate the whole document at once. However, despite the low ranking of the human translation, it seems that at the moment a translation has to be augmented by a professional translator. The BLEU scores of the one against all with human hypothesis show that the human translation is quite different from the MT and it could be that this was the reason for the low score in the study. In the examples above it is obvious that the translations by professionals are quite different. For one they use the appropriate terminology, for another they use another style of writing if a literal or a word-for-word translation is not the right choice. This is a reason why an MT has to be edited by a translator. It is unclear if a pre-translation by one of the better MT systems would be helpful. While the translations are judged to be reasonably good, the difference between MT and human translation means that they have to be changed. The editing cost to get from an MT to a finished good translation could be higher than the cost for translating everything by hand. It could be helpful in this case to have a quality estimation to give translators an overview if the sentences translated by an MT need no change, some editing, or a new translation.

MT systems are good but not good enough to compete with professional translators. They can augment their work but in the translation of important documents or more complicated and domain-specific texts, a professional translation is still needed. It remains to be seen how MT systems develop in the next few years and how the evaluation of them can improve. There has been continuous improvement in the last few years and there are always new metrics published. For the near future, the job of a translator does not seem to be in danger but might be subject to changes.

# References

::: {#refs}

:::
