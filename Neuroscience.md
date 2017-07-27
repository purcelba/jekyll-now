---
layout: page
title: Neuroscience research
permalink: /Neuroscience/
---
I study how the brain combines diverse types of information and expectations to make decisions.  To do this, I combine computational modeling, behavioral studies in humans and nonhuman primates, and neurophysiological recording and manipulation.  I use model-based approaches and machine-learning algorithms to predict choices based on experience and neural activity that allow me to test hypotheses about underlying network mechanisms.

___

## Large-scale recordings of neural populations

I use state-of-the-art electrophysiology techniques to simultaneously record activity from large neural populations from multiple areas of the primate brain.  Activity from hudreds of neurons in the frontal and parietal cortex are recorded in monkeys making decisions.  I use dimensionality reduction methods to visualize the evolution of neural populations throughout decision formation. I use maching- learning algorithms to decode the information represented in different brain region and to make accurate predictions about the animal's upcoming choices. I study the correlation structure of signals within and across areas to to understand neural interactions within and across brain areas.  In addition, I inject small amounts of electrical current into different brain areas to understand how perturbation of the neural population state influences decisions at the behavioral and neural level. My results from this project will be presented at the [Society for Neuroscience Annual Meeting](https://www.sfn.org/annual-meeting/neuroscience-2017) this fall.

**Collaborator:** Roozbeh Kiani (New York University)

![Figure 1]({{ site.baseurl }}/images/gpfa_example.png "GPFA example."){: .center-image }
<p align="center">
<font size="2"><b>Figure 1.</b> (Left) I used Gaussian-Process Factor Analysis [Yu et al., 2009](https://www.ncbi.nlm.nih.gov/pubmed/19357332) to reduce the dimensionality of neural population responses over time.  Projecting the population responses onto two dimensions shows that the population evolves to signal the upcoming choice, but the magnitude of discrimination depends on the difficulty of the decision.  (Right) A linear classifier trained on 20 dimensions (~80% variance explained) can decode the upcoming choices with high accuracy as the decision evolves.</font>
</p>

___

## Neural models of decision making

I developed neural computational models of decision making.  My models used actual neurophysiological activity recorded from macaque monkeys making visual decisions as input to simple artificial neural networks.  The models predicted the monkeys's choices and response times and replicated the dynamics of actual neural populations.  I showed how new model architectures can improve prediction accuracy and predict model dynamics consistent with observed neural responses.   You can find an overview the modeling framework and sample code in the [gated_accumulator_model](https://github.com/purcelba/gated_accumulator_model) repository or read more about the details in the papers listed below.

**Collaborators:** Thomas Palmeri, Jeffrey Schall, Gordon Logan, Richard Heitz, Jeremiah Cohen (Vanderbilt University)

![Figure 2]({{ site.baseurl }}/images/gated_accumulator.png "Gated accumulator model."){: .center-image }
<p align="center">
<font size="2"><b>Figure 2.</b> Using actual neural data to generate input to decision-making networks.  Check out the <a href = "https://github.com/purcelba/gated_accumulator_model">gated_accumulator_model</a> repository or <a href = "https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellHeitzCohenSchallLoganPalmeri2010.pdf">Purcell et al. 2010</a> for more details.</font>
</p>

### Papers
- Purcell, B. A., Heitz, R. P., Cohen, J. Y., Schall, J. D., Logan, G. D., & Palmeri, T. J. (2010). Neurally constrained modeling of perceptual decision making. Psychological review, 117(4), 1113.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellHeitzCohenSchallLoganPalmeri2010.pdf)
- Purcell, B. A., Schall, J. D., Logan, G. D., & Palmeri, T. J. (2012). From salience to saccades: multiple-alternative gated stochastic accumulator model of visual search. Journal of Neuroscience, 32(10), 3433-3446.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellSchallLoganPalmeri2012.pdf)
- Purcell, B. A., & Palmeri, T. J. (2017). Relating accumulator model parameters and neural dynamics. Journal of Mathematical Psychology, 76, 156-171.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellPalmeri2017.pdf)
- Schall, J. D., Purcell, B. A., Heitz, R. P., Logan, G. D., & Palmeri, T. J. (2011). Neural mechanisms of saccade target selection: gated accumulator model of the visual–motor cascade. European Journal of Neuroscience, 33(11), 1991-2002.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/SchallPurcellHeitzLoganPalmeri2011.pdf)
- Zandbelt, B., Purcell, B. A., Palmeri, T. J., Logan, G. D., & Schall, J. D. (2014). Response times from ensembles of accumulators. Proceedings of the National Academy of Sciences, 111(7), 2848-2853.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/ZandbeltPurcellPalmeriLoganSchall2014.pdf)

### In the news
- [Vanderbilt News](https://news.vanderbilt.edu/2010/10/08/neurons-cast-votes-to-guide-decision-making/)
- [Vanderbilt News](https://news.vanderbilt.edu/2014/02/03/number-of-neurons/)
- [Futurity Research News](http://www.futurity.org/lots-neurons-can-work-fast-just/)

___

## Models of adaptive decision making

I combined human psychophysical experiments, computational models of behavior, and neurophysiological recordings from behaving macaque monkeys to understand how we adapt our decision strategies following mistakes. I fit models of choice and reaction time to human and monkey behavior and optimized model parameters to make inferences about the underlying neural mechanisms. To test the model predictions, I analyzed neural dynamics recorded from the parietal cortex of monkeys performing a perceptual decision-making task. The neural data matched the model predictions and showed that although our visual sensitivity drops following errors, our brain can compensate by adaptively slowing down the subsequent choice. You can read more about it in the following paper or check out some of the popular press on the article.  

**Collaborator:** Roozbeh Kiani (New York University)

![Figure 3]({{ site.baseurl }}/images/pes.png "Post-error slowing."){: .center-image }
<p align="center">
<font size="2"><b>Figure 3.</b> Decision errors slow us down (Left).  We found that neural activity in parietal cortex also changes (Middle).  Our computational modeling explains why this happens (Right).  Check out <a href = "https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellKiani2016a.pdf">Purcell & Kiani (2016)</a> for more details.</font>
</p>

### Paper
- Purcell, B. A., & Kiani, R. (2016). Neural mechanisms of post-error adjustments of decision policy in parietal cortex. Neuron, 89(3), 658-671.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellKiani2016a.pdf)

### In the news
- [The Atlantic](https://www.theatlantic.com/science/archive/2016/02/why-mistakes-are-often-repeated/470778/)
- [ScienceLine](http://scienceline.org/2016/08/i-fd-up/)
- [Medical Daily](http://www.medicaldaily.com/errors-decision-making-just-slow-us-down-why-we-dont-always-learn-our-mistakes-370600)
- [Science Daily](https://www.sciencedaily.com/releases/2016/01/160121130011.htm)
- [The Sun](https://www.thesun.co.uk/archives/news/195779/the-reason-why-we-dont-learn-from-our-mistakes-is-revealed/)

___

I developed a new computational model to understand how the brain flexibly changes the decision strategy in a dynamic environment.  When our choices produce errors, we need to decide whether the mistake was the result of poor information or a bad strategy - i.e, am I making the wrong moves or am I just being dealt bad hands?  To optimally decide when the strategy must change, we should treat negative outcomes as evidence that for a bad strategy and weight it by our choice confidence.  I developed a model that showed how the brain could use simple computations to implement this approach.  I tested the models behavioral predictions with a  human psychophysics experiment. I show that the model accurately predicts human behavior and quantitatively outperforms plausible alternatives. I am currently conducting experiments in non-human primates to test the models predictions about neural mechanisms.  

![Figure 4]({{ site.baseurl }}/images/cngenv.png "Changing environment."){: .center-image }
<p align="center">
<font size="2"><b>Figure 4.</b> A computational model explains decisions based on a visual stimulus and strategy about how to report the decision. (Left) Stronger visual information leads to greater accuracy.  (Middle) Subjects are more likely to change strategies when choices associated with greater evidence (i.e., higher confidence) cause negative feedback. (Right) A model explains how accumulation of visual evidence maps to confidence, which in turn provides evidence for changes of strategy.  See <a href = "https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellKiani2016b.pdf">Purcell & Kiani (2016b)</a> for more details.</font>
</p>

### Paper:
- Purcell, B. A., & Kiani, R. (2016). Hierarchical decision processes that operate over distinct timescales underlie choice and changes in strategy. Proceedings of the National Academy of Sciences, 113(31), E4531-E4540.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellKiani2016b.pdf)

### In the news
- [Neuroscience News](http://neurosciencenews.com/confidence-neuroscience-error-4705/)
- [Eureka Alert](https://www.eurekalert.org/pub_releases/2016-07/nyu-iga071416.php)
- [Science Daily](https://www.sciencedaily.com/releases/2016/07/160718161503.htm)

___
## Neurophysiological representations of visual salience

In two studies, I recorded activity from neurons in medial and lateral frontal cortex of monkeys trained to perform a visual search task in which they located a target item among distractors (e.g., red circle among green circles).  I used auROC curves and other analyses to quantify the information about object identity carried by individual neurons.  I found that lateral frontal cortex robustly represents the location of the target, but medial frontal corext did not.  Instead, medial frontal cortex seems to play a distinct role in signaling negative feedback.

**Collaborators:** Polly Weigand, Jeffrey Schall, Richard Heitz, Jeremiah Cohen (Vanderbilt University)

![Figure 5]({{ site.baseurl }}/images/sef2.png "SEF and FEF."){: .center-image }
<p align="center">
<font size="2"><b>Figure 5.</b> (Left) Example neurons from lateral and medial frontal cortex.  Only the lateral neuron signals the target location (red shading).  (Right) An auROC analysis indicates that lateral frontal neurons signal significantly more information about target location.  See <a href = "https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellWeigandSchall2012.pdf">Purcell et al. (2012)</a> for more details.</font>
</p>

### Papers
- Purcell, B. A., Weigand, P. K., & Schall, J. D. (2012). Supplementary eye field during visual search: salience, cognitive control, and performance monitoring. Journal of Neuroscience, 32(30), 10273-10285.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellWeigandSchall2012.pdf)
- Purcell, B. A., Heitz, R. P., Cohen, J. Y., & Schall, J. D. (2012). Response variability of frontal eye field neurons modulates with sensory input and saccade preparation but not visual search salience. Journal of neurophysiology, 108(10), 2737-2750.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellHeitzCohenSchall2012.pdf)

___
## Homologous neural mechanisms of visual attention and working memory in humans and non-human primates

I collaborated with Goeff Woodman to conduct two studies of electroencephalogram (EEG) signals - brain activity recorded from the surface of the head in humans that can be used to index cognitive states.  My work helped to establish a non-human primate model of EEG components associated with visual attention and working memory.  By pairing EEG from outside the skull with recordings of neural activity from inside the brain, we could study how neural populations give rise to observed EEG.  My work showed how frontal cortex indirectly contributes to the generation of attention and memory related EEG signals.  You can read more about this work in the papers below or check out [Geoff's website](http://www.psy.vanderbilt.edu/faculty/woodman/) for more information about his awesome work.

**Collaborators:** Geoff Woodman, Rob Reihardt, Jeffrey Schall, Richard Heitz, Polly Weigand (Vanderbilt University)

![Figure 6]({{ site.baseurl }}/images/eeg.png "FEF and EEG."){: .center-image }
<p align="center">
<font size="2"><b>Figure 6.</b> Example neuron (top, blue), local field potential (middle, green), and EEG (bottom, red) recorded from a macaque monkey performing a visual search task.  All three signals indicate the location of the search target. See <a href = "https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellSchallWoodman2013.pdf">Purcell et al. (2013)</a> for more details.</font>
</p>

### Papers
- Purcell, B. A., Schall, J. D., & Woodman, G. F. (2013). On the origin of event-related potentials indexing covert attentional selection during visual search: timing of selection by macaque frontal eye field and event-related potentials during pop-out search. Journal of neurophysiology, 109(2), 557-569.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/PurcellSchallWoodman2013.pdf)
- Reinhart, R. M., Heitz, R. P., Purcell, B. A., Weigand, P. K., Schall, J. D., & Woodman, G. F. (2012). Homologous mechanisms of visuospatial working memory maintenance in macaque and human: properties and sources. Journal of Neuroscience, 32(22), 7711-7722.[pdf](https://github.com/purcelba/purcelba.github.io/blob/master/docs/ReinhartHeitzPurcellWeigandSchallWoodman2012.pdf)

___

## Evaluating methods for measuring and visualizing human similarity judgments

As an undergraduate, I evaluated methods for visualizing the structure of human categorization of colors and personality traits.   I collected human similarity judgments of color and personality trait stimuli using a "sort-and-merge" procedure in which subjects grouped stimuli into clusters based on their perceived similarity and subsequently merged the clusters based their overall group similarity.  I visualized the resulting similarity structure using an multidimensional scaling (MDS) procedrue.  I assessed the internal validity of the procedure using percent variance explained and the external validity of the procedure by comparing the shape of the solution to a "ground truth" dataset obtained using pairwise similarity judgments.   I found that the sort-and-merge procedure could capture the structure of judgments quite well, establishing it as a cheaper and less time-consuming alternative to pairwise judgments for measuring human categorization.


**Collaborator:** Robin Thomas (Miami University)

![Figure 7]({{ site.baseurl }}/images/mds.png "MDS."){: .center-image }
<p align="center">
<font size="2"><b>Figure 7.</b> First two dimensions of the MDS solution for personality trait data collected with sort-and-merge procedure compared to pairwise similarity judgments. Both methods generally agree and reveal the structure of human categorization.
</p>

