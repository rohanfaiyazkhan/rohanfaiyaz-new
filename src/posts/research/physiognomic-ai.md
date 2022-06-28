---
title: 'Written on your Face: The Dangerous Assertions of Physiognomic AI Research'
author: 'Rohan Faiyaz Khan'
date: 2022-03-29
tags:
  - 'AI'
  - 'Fairness'
excerpt: This essay analyzes the ethics of AI that uses facial analysis to perform social classification, and criticizes defences researchers of such AI have used to jusitfy their work.
---

_Physiognomy_ is the study of inferring temperament and character from facial features. There are many links between physiognomy and the study of phrenology, which is the infamous study of deriving character or mental faculties from the bumps on one's skull. Historically both physiognomy and phrenology have been criticized and debunked as being scientifically unsound [^stark2021]. Critics have also pointed out how phrenology and physiognomy exhibit racist, classist and sexist prejudices legitimized through "science", which were used to naturalize the oppression of disenfranchised populations and serve political arguments in favor of colonization and slavery [^thompson2021].

The advent of AI has made it possible for physiognomy to make a resuregence under the guise of AI powered computer vision applications and research. Automated hiring such as HireVue uses facial recognition to analyze the performance of intervewees, recidivism risk for those awaiting bail is decided by algorithms, and crowd surveillance uses automated emotion detection to find suspicious individuals. Top conferences and journals have also published research claiming facial analysis can be used for social categorization such as predicting sexual orientation [^kosinski2017], political orientation [^kosinski2021], criminality [^criminality2016] among others.

It should not be surprising then that both commercial technology using physiognomic AI and academic research justifying its use have come under criticism [^stinson2020] [^stark2021]. Physiognomic AI make assumptions relating the human face and body to characteristics originating from the harmful pseudosciences of phrenology and physiognomy and provides mechanisms for normalizing such assumptions. Methodological and empirical challenges have also been made [^rhue] [^pseudoscience2022], this essay will attempt to focus on the ethical justifications for use of such AI, and defences physiognomic AI researchers have used to jusitfy their work.

## A Historic Retrospective on the Origins of Phrenology and Physiognomy

Physiognomy and phrenology can be traced as early as Victorian era Europe where scholars such as Johann Kaspar Lavater and Franz Josef Gall developed and popularized methods of collecting data on faces and skulls in an attempt to form a system of judgement from facial and cranial features [^pearl]. Such theories became very culturally popular in societies such as eighteenth and early nineteenth century Britain where denizens of growing industrial cities could adopt a framework of individual judgement that aligned with many of their held predispositions [^pearl]. As such, the tapestry of Western culture became intertwined with beliefs validating physiognomy, and this practice also meant people had to actively engage in self presentations that were considered favorable by physiognomic beliefs. Adoption of physiognomy into "an important vehicle of liberal ideaology" would also be favorable to establishing power structures of colonialism and slavery [^giustino]. It was convenient to be able to justify subjugation of certain populations if they possessed facial and bodily characteristics that could be interpreted as naturally inferior, and focus on the supposed "social good" of educating these populations on good societal mental faculties [^giustino].

A particularly significant scholar in physiognomy would be British statistician Francis Galton (who was also quite a strong proponent of eugenics). Galton in a rather famously unsuccessful study, attempted to create composite portaits of accused criminals in an attempt to discover facial features that would be indicative of criminality. Despite being unsuccessful in his endeavor, Galton would be extremely influencial in physiognomic research going forward, with researchers now attempting to look for facial characteristics that signified broader social character.

Of course given the luxury of looking back from today, it goes without saying the phrenologists such as Laveter, Gall and Galton did not succeed in establishing any strong evidence linking facial features and social characterization [^schlag]. And after World War II, eugenics, phrenology and physiognomy became rather unpopular. However, this was not due to lack of effort on phrenologists' part. When late phrenology was under attack, researchers had to justify their use of phrenologic research in the face of a large body of evidence discrediting it as a valid field of science. One particular strategy was to focus on proposed social good, choosing to avoid the question of whether phrenology was real but rather direct the question to what benefits phrenology could provide to society (particulary, colonial and settler-colonial Western society) [^schlag]. Even when serious academics were moving away from these pseudoscience, enterpreneurs picked up on the act which would be rather indicative of the future.

## Facial recognition and the power of physiognomic black box

The power of computer vision algorithms has exploded since AlexNet showed in 2012 what could be achieved by leveraging tremendous GPU power and deep neural networks for categorization tasks [^alexnet]. At the same time, the culture surrounding deep learning research would come under criticism for chasing benchmark scores in a practice where the original task and the justification behind would soon become lost as long as the accuracy figures are impressive [^benchmarking]. To confound matters even further, deep neural networks in particular are opaque black boxes that cannot inherently explain their predictions [^blackbox].

The combination of all these factors would be the perfect storm necessary for the return of physiognomy. Facial recognition would become a rather important field in AI, and it would become commonplace to use models pretrained on massive datasets for feature extraction. For instance, the model proposing prediction of political orientation uses a commerical implementation (from Face++) of a ResNet-50 model trained on VGGFace2 dataset [^kosinski2021]. These extracted features could be used to perform any manner of classification, including social classification. Of course, this assumes that the features extracted from the images are perfectly representative of the images, something which even the authors of such models dispute. The authors of the mentioned VGGFace2 dataset are very explicit that their dataset has cultural and social biases and should not be considered universally representational [^vggface2], and therefore one can reason that features extracted from an image by a model trained on VGGFace2 would also not be a perfect representation of the image. And speaking of biases, the labels used to train the subsequent classification step is also likely to suffer from biases regarding where the data originated from. Using our political orientation example, the data was collected using self reported political orientation labels associated with profile pictures. It is highly likely that there was some kind of cultural performativity associated with how people presented in these images, and it is certain that these presentational differences are likely to influence the model [^pseudoscience2022].

Now these methodological questions don't matter because a neural network is an opaque black box that can hide away questionable choices in methodology, as well as blanket that the task being performed under the hood is quite similar to the debunked practices of physiognomy and phrenology. The criticisms do not matter to the researchers as long as they can frame the accuracy as surprisingly high. This is the approach used to justify both the political orientation predictor as well as the sexual orientation predictor. The dubious findings of both models do not need to be explained because deep neural networks cannot fully be explained.

AI physiognomic researchers have attempted to defend their use of such AI by acknowledging the harms of past phrenology and physiognomy, only to later make the same assumptions that give credence to the discredited pseudosciences. Starks and Hutson in their review of Physiognomic AI criticize the authors of the sexual orientation predictor for both disavowing phrenology while at the same time devoting two pages to discussing the links between facial features and "character" and using that framework to base their study [^stark2021]. Later in the political orientation predictor, Kosinski states predicting political orientation from facial analysis “does not necessarily imply that liberals and conservatives have innately different faces” and that more work should be done to analyze the extent to which “transient features could fully account for the predictability of political orientation” [^kosinski2021]. However, he also states “algorithms would likely quickly learn how to extract relevant information from other features" [^kosinski2021], implying that perhaps innate facial features are the primary driving force behind such predictive power.

Like phrenologists of the past, social good is often used to justify the research of physiognomic AI. For example, Kosinski cites privacy concerns as the primary reason to raise awareness on the possibility of social classification [^kosinski2021]. However, in the process of doing so physiognomic AI make statements and assumptions that validate dangerous physiognomic and phrenological claims. This leads to the conclusion that preserving privacy is all that is needed to "fix" physiognomic AI. Given the immense harms that are being perpretrated by such AI in the wild in commercial applications, there is a strong need to reject all notions of social good justifying technology that reinforce harmful pseudosciences. Furthermore, this absolutely does not excuse physiognomic AI researchers from the responsibility of engaging with criticisms of phrenology and physiognomy.

[^stark2021]: L. Stark and J. Hutson, “Physiognomic Artificial Intelligence,” Social Science Research Network, Rochester, NY, SSRN Scholarly Paper ID 3927300, Sep. 2021. doi: 10.2139/ssrn.3927300.
[^kosinski2017]: Y. Wang and M. Kosinski, “Deep neural networks are more accurate than humans at detecting sexual orientation from facial images.,” American Psychological Association, vol. 114, no. 2, pp. 460–468, 2017.
[^kosinski2021]: M. Kosinski, “Facial recognition technology can expose political orientation from naturalistic facial images,” Scientific Reports, vol. 11, no. 1, Art. no. 1, Jan. 2021, doi: 10.1038/s41598-020-79310-1.
[^criminality2016]: X. Wu and X. Zhang, “Automated inference on criminality using face images” arXiv:1611.04135, 4038-4052.
[^thompson2021]: C. E. Thompson, “Phrenology Is Here To Stay,” Arc Digital, Feb. 11, 2021. https://medium.com/arc-digital/phrenology-is-here-to-stay-c835b5ce5032 (accessed Mar. 31, 2022).
[^stinson2020]: C. Stinson, “The Dark Past of Algorithms That Associate Appearance and Criminality,” American Scientist, Dec. 02, 2020. https://www.americanscientist.org/article/the-dark-past-of-algorithms-that-associate-appearance-and-criminality (accessed Mar. 31, 2022).
[^pseudoscience2022]: R. F. Khan, S. Baranek, G. Reeds, C. Stinson, "Empirical Challenge to Social Classification using Facial Recognition" _unpublished_
[^rhue]: L. Rhue, “Racial Influence on Automated Perceptions of Emotions,” Social Science Research Network, Rochester, NY, SSRN Scholarly Paper ID 3281765, Nov. 2018. doi: 10.2139/ssrn.3281765.
[^giustino]: D. de Giustino, “Reforming the Commonwealth of Thieves British Phrenologists and Australia,” Victorian Studies, Indiana University Press, vol. 15, no. 4, pp. 439–461, 1972.
[^pearl]: S. Pearl, About Faces: Physiognomy in Nineteenth-Century Britain. Harvard University Press, 2010. [Online]. Available: https://books.google.ca/books?id=V87xpO3o3F0C
[^schlag]: P. Schlag, “Law and Phrenology Commentary,” Harv. L. Rev, vol. 110, no. 4, pp. 877–921, 1997.
[^alexnet]: A. Krizhevsky, I. Sutskever, and G. E. Hinton, “ImageNet Classification with Deep Convolutional Neural Networks,” in Advances in Neural Information Processing Systems, 2012, vol. 25. Accessed: Apr. 01, 2022. [Online]. Available: https://papers.nips.cc/paper/2012/hash/c399862d3b9d6b76c8436e924a68c45b-Abstract.html
[^benchmarking]: I. D. Raji, E. M. Bender, A. Paullada, E. Denton, and A. Hanna, “AI and the Everything in the Whole Wide World Benchmark,” arXiv:2111.15366 [cs], Nov. 2021, Accessed: Feb. 03, 2022. [Online]. Available: http://arxiv.org/abs/2111.15366
[^blackbox]: C. Rudin, “Stop Explaining Black Box Machine Learning Models for High Stakes Decisions and Use Interpretable Models Instead,” arXiv:1811.10154 [cs, stat], Sep. 2019, Accessed: Mar. 31, 2022. [Online]. Available: http://arxiv.org/abs/1811.10154
[^vggface2]: Q. Cao, L. Shen, W. Xie, O. M. Parkhi, and A. Zisserman, “VGGFace2: A dataset for recognising faces across pose and age,” arXiv:1710.08092 [cs], May 2018, Accessed: Nov. 20, 2021. [Online]. Available: http://arxiv.org/abs/1710.08092