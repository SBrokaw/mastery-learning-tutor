# The Math Academy Way - Technical Deep Dives

*Extracted from "The Math Academy Way" by Justin Skycak*

# V. TECHNICAL DEEP DIVES {#v.-technical-deep-dives}

## Chapter 26\. Technical Deep Dive on Spaced Repetition {#chapter-26.-technical-deep-dive-on-spaced-repetition}

***Note:** this chapter elaborates on concepts introduced in [chapter 18](#chapter-18.-spaced-repetition-\(distributed-practice\)).*

***Summary:** Math Academy employs Fractional Implicit Repetition (FIRe), a novel spaced repetition algorithm, to calculate student learning profiles. FIRe generalizes spaced repetition to hierarchical knowledge, allowing repetitions on advanced topics to implicitly trickle down to simpler topics. The algorithm handles partial encompassings and extends repetition flows through fractional encompassings, optimizing credit distribution. The speed of the spaced repetition process is calibrated to each individual student on each individual topic, where student ability and topic difficulty are competing factors.*

### Fractional Implicit Repetition (FIRe) {#fractional-implicit-repetition-(fire)}

To calculate student spaced repetition profiles, Math Academy uses a novel spaced repetition algorithm called **Fractional Implicit Repetition (FIRe).** FIRe generalizes spaced repetition to hierarchical bodies of knowledge where

1. repetitions on advanced topics “trickle down” implicitly to simpler topics through encompassing relationships, and

2. simpler topics receiving lots of implicit repetitions discount the repetitions appropriately (since they are often too early to count for full credit towards the next repetition).

#### | Concrete Example {#|-concrete-example}

As a concrete example, recall that *Multiplying a Two-Digit Number by a One-Digit Number* encompasses *Multiplying One-Digit Numbers* and *Adding a One-Digit Number to a Two-Digit Number*.

If you pass a review on *Multiplying a Two-Digit Number by a One-Digit Number*, then the repetition will also flow backward to reward *Multiplying One-Digit Numbers* and *Adding a One-Digit Number to a Two-Digit Number* because you’ve just shown evidence that you still know how to perform these skills.

| ![][image16] | →  | ![][image17] |
| :---: | :---: | :---: |

On the other hand, if you fail a repetition on *Adding a One-Digit Number to a Two-Digit Number*, then the failed repetition will also flow forward to penalize *Multiplying a Two-Digit Number by a One-Digit Number*. If you can’t add a one-digit number to a two-digit number, then there’s no way you’re able to multiply a two-digit number by a one-digit number. The same thing happens if you fail a repetition on *Multiplying One-Digit Numbers*.

| ![][image60] | →  | ![][image61] |
| :---: | :---: | :---: |

#### | Visualizing Repetition Flow {#|-visualizing-repetition-flow}

Note that repetition flows can extend many layers deep – not just to directly encompassed topics, but also to “second-order” topics that are encompassed *by the encompassed topics*, and then to third-order topics that are encompassed by second-order topics, and so on.

Visually, credit travels downwards through the knowledge graph like lightning bolts.

| ![][image62] | →  | ![][image63] | → | ![][image64] |
| :---: | :---: | :---: | :---: | :---: |

Penalties travel upwards through the knowledge graph like growing trees.

| ![][image65] | →  | ![][image66] | → | ![][image67] |
| :---: | :---: | :---: | :---: | :---: |

#### | Partial Encompassings {#|-partial-encompassings}

FIRe also naturally handles cases of **partial encompassings**, in which only some part of a simpler topic is practiced implicitly in an advanced topic. This occurs more frequently in higher-level math.

For instance, in calculus, advanced integration techniques like integration by parts require you to calculate integrals of a variety of mathematical functions such as polynomials, exponentials, and trigonometric functions. But some of those functions might only appear in a portion of the integration by parts problems. So, if you complete a repetition on integration by parts, you should only receive a fraction of a repetition towards each partially-encompassed topic.

In the diagram below, we label encompassings with numerical **weights** that represent what fraction of each simpler topic is practiced during the more advanced topic. You can loosely interpret each weight as representing the probability that a random problem from the advanced topic encompasses a random problem from the simpler topic.

![][image68]

FIRe extends repetition flows many layers deep through fractional encompassings as well. The end result is that repetitions

1. travel unhindered along a “trunk” of full encompassings, and

2. fade off along partial encompassings branching outwards from the trunk.

![][image69]

### Setting Encompassing Weights Manually {#setting-encompassing-weights-manually}

#### | Direct and Key Prerequisites are Sufficient {#|-direct-and-key-prerequisites-are-sufficient}

Because encompassing weights are set manually, it is not feasible to set an explicit weight between every pair of topics in the graph. We have thousands of topics, so the full pairwise weight matrix would contain tens of millions of entries. How do we set all those weights?

It turns out that it is not actually necessary to explicitly set every weight in the matrix. It suffices to set only the weights for topic pairs where

1. the weight has a nontrivial value,

2. the weight cannot otherwise be inferred using repetition flow, and

3. the distance between the topics in the prerequisite graph is low,

and assume that all other weights not computed implicitly during repetition flow are zero. The reasoning behind these conditions is as follows:

1. The magnitude of the weight represents the magnitude of the implicit repetition credit. In order for an implicit repetition to make an impact on staving off explicit reviews, it has to be associated with a nontrivial amount of credit.

2. If repetition flow can infer a weight, then nothing will change if the weight is set manually (unless the manually-set weight is being used to correct a value that would otherwise be inferred by repetition flow).

3. If two topics are far apart in the prerequisite graph, then their weight will not make much of an impact on staving off reviews, even if it is a full encompassing. In that case, by the time the student reaches the more advanced topic, they will already have done most of their explicit reviews on the simpler topic.

Conveniently, the weights that satisfy the above conditions tend to be those along direct and key prerequisite edges, the number of which scales linearly with the number of topics. This makes it feasible to set encompassing weights manually: one weight for each direct or key prerequisite

Note that it is not unusual to find direct and key prerequisite edges with a weight as low as zero. This can happen when a topic requires some amount of conceptual familiarity with the prerequisite, but does not require the student to actually have mastered the prerequisite to the point of being able to solve problems in the prerequisite topic.

#### | Non-Ancestor Encompassings and Mastery Floors {#|-non-ancestor-encompassings-and-mastery-floors}

To comply with course standards, it is sometimes necessary to have **equivalent topics** spread out across multiple courses, with the equivalent topics in higher courses covering a more advanced treatment of the same skills taught in lower courses. However, the simpler equivalent topics are usually not required as prerequisites for the more advanced equivalent topics.

For instance, courses on algebra-based statistics and calculus-based statistics would have many equivalent topics that cover the same skills. Although the calculus-based statistics course would provide more advanced treatments of these skills, the corresponding equivalent topics in the algebra-based statistics course would not be prerequisites.

Even though simple equivalent topics would not be ancestors of advanced equivalent topics via direct or key prerequisite paths, we can still set full-encompassing edge weights between them so that a student who completes an advanced topic will implicitly receive credit for any simpler equivalent topics as well. These are called **non-ancestor encompassings**.

Non-ancestor encompassings, along with course-based **mastery floors** (lower-course topics that are automatically considered mastered by any student taking the course), can also be useful for assigning credit to leaf topics in lower-level courses. The mastery floor of a course consists of the lower-course topics that

* are “far back” enough that it is safe to automatically consider them mastered, or

* lie “below” the simplest topics that could be assessed on the course’s diagnostic.

Intuitively, the top of the mastery floor marks the dividing line regarding whether it is at all feasible for a student to take a course.

### Student-Topic Learning Speeds {#student-topic-learning-speeds}

#### | Ratio of Student Ability and Topic Difficulty {#|-ratio-of-student-ability-and-topic-difficulty}

Student ability and topic difficulty are competing factors – high student ability speeds up the overall student-topic learning speed, while high topic difficulty slows it down. So, to compute a student-topic learning speed, we compute

1. the speedup due to student ability,

2. the slowdown due to topic difficulty, and then

3. their ratio.

[![][image70]](https://www.codecogs.com/eqnedit.php?latex=%5Ctextrm%7Bstudent-topic%20learning%20speed%7D%20%3D%20%5Cdfrac%7B%5Ctextrm%7Bspeedup%20due%20to%20student%20ability%7D%7D%7B%5Ctextrm%7Bslowdown%20due%20to%20topic%20difficulty%7D%7D#0)

| Student-Topic Learning Speed vs Student Ability and Topic Difficulty  |  | *Student Ability* |  |  |
| :---: | :---: | :---: | :---: | :---: |
|  |  | *Strong* | *Moderate* | *Weak* |
| *Topic Difficulty* | *Easy* | fastest | faster | baseline |
|  | *Moderate* | faster | baseline | slower |
|  | *Hard* | baseline | slower | slowest |

#### | Measuring Student Ability at the Level of Individual Topics {#|-measuring-student-ability-at-the-level-of-individual-topics}

**Student ability** is measured at the granular level of individual topics – we keep track of accuracy across answers, giving more weight to recent answers, and also propagating

* correct answers down to simpler encompassed topics and

* incorrect answers up to more advanced topics that encompass the answered topic.

To choose the initial starting value for a topic’s accuracy, we make a prediction based on the accuracy values of the topic’s **local neighborhood** consisting of its direct prerequisites, key prerequisites, encompassings, and same-module topics.

![][image71]

#### | Measuring Topic Difficulty {#|-measuring-topic-difficulty}

**Topic difficulty** is measured by computing the topic’s accuracy across all instances when one of its questions was answered by a serious student on an assessment.

In theory, if student abilities could be measured on each topic with perfect fidelity, then topic difficulties would no longer be needed and student-topic learning speeds could be based entirely on student abilities. But in practice, there are two reasons why it is helpful to rely on topic difficulty as well:

1. *It improves the initial prediction.* Although we already have information about the particular student’s learning speed on other topics, the topic difficulty provides information about the particular topic’s learning speed for other students. This is an independent, information-rich signal. 

2. *It naturally acts as a correction factor.* When topic difficulty is high, it decreases the learning speed – which is desirable given that high topic difficulty is caused by low assessment performance, which is in turn (largely) caused by students not getting enough review. Similarly, when topic difficulty is low, it increases the learning speed – which is desirable given that low topic difficulty is caused by extremely high assessment performance, which indicates that students might not need as much review as they are receiving.

### High-Level Structure {#high-level-structure}

At a high level, the structure of Math Academy’s spaced repetition model can be summarized as follows:

[![][image72]](https://www.codecogs.com/eqnedit.php?latex=%5Cbegin%7Balign*%7D%20repNum%20%26%5Cto%20%5Cmax%20%5Cleft\(%200%2C%20%5Cphantom%7B%2C%7D%20repNum%20%2B%20speed%20%5Ccdot%20decay%5E%7B%5C%2C%20failed%7D%20%5Ccdot%20rawDelta%20%5Cright\)%20%5C%5C%5C%5C%20memory%20%26%5Cto%20%5Cmax%20%5Cleft\(%200%2C%20%5Cphantom%7B%2C%7D%20memory%20%2B%20rawDelta%20%5Cright\)%20\(0.5\)%5E%7B%20%5C%2C%20days%20%5C%2C%20%2F%20%5C%2C%20interval%7D%20%5Cend%7Balign*%7D#0)

* *repNum* \= how many successful rounds of spaced repetition the student has accumulated on a particular topic.

  In following definitions, a "repetition" is a successful review at the appropriate time.

* *days* \= how many days it's been since the previous repetition.

* *interval* \= the ideal number of days to be spaced between repetition *repNum* and repetition *repNum+1*.

* *memory* \= how well the student is expected to remember now that it's been some time since the previous repetition. Memory decays over time and the next repetition becomes due when the memory becomes sufficiently low.

* *speed* \= the learning speed for the student on this particular topic, based on how well the student is performing. Governs how quickly the student moves forwards or backwards through the spaced repetition process.

* *failed* \= 1 if repetition was failed and 0 if it was passed.

* *rawDelta* \= how much raw spaced repetition credit the student earned during the repetition, ignoring speed and decay. *rawDelta* is positive if the repetition was passed and negative if failed.

  The higher the quality of work in a passed repetition, or the worse the quality of work in a failed repetition, the larger the magnitude of *rawDelta*.

  The magnitude of *rawDelta* is also discounted if the repetition was completed early relative to the desired *interval*, i.e., if *memory* is sufficiently high.

  Note that successful work (positive credit) on an advanced topic is also counted towards any simpler topics that are implicitly practiced as component skills, and unsuccessful work (negative credit) on a simpler topic is also counted towards any more advanced topics of which that simpler topic is a component skill.

* *decay* \= the speed at which the student moves backwards in the spaced repetition process, relative to their forwards speed, if they fail a repetition.

  *decay* is a positive quantity that starts at 1 and grows larger as the repetition becomes more overdue relative to its ideal *interval*, i.e., as *memory* becomes severely low.

  *decay* was introduced to model severe knowledge decay like the notorious "summer slide," where topics learned shortly before the end of the previous school year may be forgotten so severely over summer vacation that they need to be reviewed more frequently or even completely re-taught at the start of the following year.

![][image73]

## Chapter 27\. Technical Deep Dive on Diagnostic Exams {#chapter-27.-technical-deep-dive-on-diagnostic-exams}

***Note:** this chapter elaborates on concepts introduced in [chapter 4](#bookmark=id.pinaf5gi9iat) and [chapter 20](#bookmark=id.9piu58tpshrd).*

***Summary:** Math Academy uses adaptive diagnostics to infer each incoming student’s knowledge profile. The novel diagnostic assessment algorithm leverages causal relationships and correlation-based inference in the knowledge graph to efficiently gauge a student’s knowledge frontier to a sufficient level of detail while minimizing the number of questions on the diagnostic. It measures knowledge confidence and handles conflicting information, adapting its diagnosis to nuanced scenarios like prerequisite-postrequisite and accuracy-time conflicts. Conditional completion is employed for areas where knowledge was inferred with low confidence, allowing the system to continue fine-tuning a student’s placement as it collects more data about their performance.*

### Minimizing the Number of Questions {#minimizing-the-number-of-questions}

Without any clever algorithms, it would take a massive number of diagnostic questions to infer a student’s knowledge frontier. Courses often contain up to several hundred topics, plus twice as many foundational topics – which means that if we started at the bottom and asked you a diagnostic question for every topic up until the point that you could no longer answer them correctly, we’d end up asking you 500+ questions in total.

However, Math Academy is able to cut down this number of diagnostic questions by an order of magnitude using a novel diagnostic question selection algorithm. Our diagnostics generally take only 20-40 questions for lower-grade courses (like Prealgebra) and 40-60 questions for higher-grade courses (like Calculus).

We’re able to achieve this level of diagnostic efficiency for two reasons:

1. In addition to leveraging “causal” relationships, i.e. encompassings, we also leverage looser forms of correlation-based inference.

2. We compress the knowledge graph beforehand into the smallest number of topics that fully "covers" a course and its foundations at a desired level of granularity.

As a result, our diagnostic assessment algorithm is highly leveraged on both accuracy and precision. While a perfect-accuracy, perfect-precision diagnostic could require up to a thousand questions, we are able to reduce this by an order of magnitude by giving up a negligible amount of accuracy and precision.

(Of course, highly leveraged algorithms have a higher risk of being thrown off by false input – but we mitigate this risk by detecting and re-assessing questions that we suspect may have been incorrectly assessed.)

### Knowledge Confidence and Conditional Completion {#knowledge-confidence-and-conditional-completion}

#### | Theory {#|-theory}

During diagnostics, Math Academy also measures **knowledge confidence**, i.e., our confidence in our classification of whether the student knows or does not know a topic. Most diagnostics complete with fairly high confidence across the student’s entire knowledge profile, but occasionally, there can be areas of low confidence. These do not arise from a lack of diagnostic coverage, but rather, from conflicting evidence in a student’s responses.

There are two main types of conflicts:

1. *Prerequisite-Postrequisite Conflict:* a student answers a more advanced topic correctly but a simpler question incorrectly, which may indicate a gap in the student’s knowledge.

2. *Accuracy-Time Conflict:* a student submits a correct answer but takes an excessively long time to solve the problem, which may indicate that they have not yet mastered the corresponding topic.

To handle these conflicts, we carefully weight positive and negative evidence against each other to form a highly nuanced diagnosis of student knowledge that adapts appropriately to future observations, just like a tutor would.

In particular, if the evidence balances out to “just barely” place a student out of some topics, the system will consider those topics **conditionally completed**: the student will initially be given tasks under the assumption that they know those topics, but if the student struggles, then the system will immediately begin “falling backwards” along the appropriate learning paths.

#### | Example {#|-example}

As an example, suppose

* student A answers many questions correctly on their diagnostic but takes excessively long on most questions, while

* student B answers fewer questions correctly but supplies correct answers quickly and confidently.

Then

* student A will have a higher amount of overall knowledge, a significant portion of which is low-confidence (and may be quickly pruned back if they struggle), while

* student B will have less overall knowledge but may have more high-confidence knowledge.

![][image74]

#### | Implementation {#|-implementation}

To achieve this behavior, we weight diagnostic evidence using a **plus-minus balance** at each topic.

* The **sign** (positive or non-positive) represents the prediction of whether the student knows the topic.

* The **magnitude** of the balance represents the degree of confidence in the prediction.

Each answer is associated with a **weight** that represents its contribution to the plus-minus balance of the corresponding topic. By default, an answer’s weight is equal to one. However, if a student submits a correct answer but takes an excessively long time relative to the expected time for a student who has mastered the topic, the answer weight is diminished. The answer still gives the student positive credit, but the slower the student is to solve the problem (beyond a reasonable time threshold), the smaller the amount of credit.

Once the weight of an answer is determined, it propagates throughout the knowledge graph updating plus-minus balances of topics as appropriate:

* A *correct* answer *increases* the plus-minus balances of the answered topic and its *prerequisites* (and their prerequisites, and so on), while

* an *incorrect* answer *decreases* the plus-minus balances of the answered topic and its *post-requisites* (and their post-requisites, and so on).

After all diagnostic questions have been processed, any topics with positive plus-minus weights are credited with a number of repetitions equal to their plus-minus balance.

### Conservative vs. Aggressive Edge of Mastery {#conservative-vs.-aggressive-edge-of-mastery}

There have been times when a student completed a course, took a placement diagnostic for the same course, and was surprised that the placement told them their knowledge frontier was lower in the course. But this is actually an expected result, especially for students who are weaker.

Just because a student successfully completes all the homework for a course, doesn't mean that they're going to ace a comprehensive final exam over the course. This is especially true for the placement exam, which is even harder than a normal exam: unlike a normal exam,

* a placement exam has to cover every topic (including all of the hardest topics), and

* it has to cover the most advanced question type from each of those topics (we can't place a student out of a topic if they only know how to do the simpler cases).

Additionally, although we often talk about a student's "edge of mastery" as though it were a single line across the student's knowledge profile, a student really has a "zone of mastery" that is bounded below by a conservative edge of mastery and above by an aggressive edge of mastery. A placement exam measures the conservative edge of mastery, while mastery-based learning with layering operates on the aggressive edge of mastery.

When a student successfully completes a lesson, they've mastered the topic well enough to continue layering on top of what they've learned, but they probably haven't reached the point of automaticity with the topic yet. By way of analogy, whenever a gymnast learns a new skill at practice, it takes some more time and practice before they are ready to showcase that skill in competition, but that doesn't stop them from continuing to work on more advanced skills during practice in the meantime.

### Supplemental Diagnostics {#supplemental-diagnostics}

As topics are added and connectivity is revised in the knowledge graph, the knowledge profile inferred from a student’s initial placement diagnostic can get a little out of date. When this happens, we assign tiny diagnostics called **supplemental diagnostics** to bring the student’s knowledge profile back up to date.

The topics on a supplemental diagnostic are those that were not directly assessed on the original diagnostic and have plus-minus balances of zero when considering all the assessment answers since (and including) the original diagnostic. The same form of highly efficient inference is used on supplemental diagnostics, which means supplemental diagnostics are generally quite small, consisting of at most a handful of questions.

### Selecting Good Diagnostic Questions {#selecting-good-diagnostic-questions}

There’s a lot of nuance that goes into selecting a good diagnostic question for a given topic.

* On one hand, diagnostic questions can’t be too easy. Each diagnostic question should be difficult enough that if a student were to answer it correctly, then an expert tutor or teacher would infer that they have fully mastered the corresponding topic.

* Additionally, a diagnostic question should exercise all of the prerequisites of the corresponding topic. Otherwise, if there’s a prerequisite that the question doesn’t exercise, then a student who doesn’t know the prerequisite could still answer the question correctly and erroneously receive credit for the prerequisite.

* That said, a diagnostic question should generally not be the most difficult question in its corresponding topic. The more complicated a question, the higher the likelihood that a student might make a silly mistake.

So, each diagnostic question should be chosen as the simplest question that

1. would convince an expert tutor or teacher that the student has mastered the corresponding topic, and

2. exercises all of the topic’s prerequisites.

In practice, due to the level of nuance required, we select diagnostic questions manually.

## Chapter 28\. Technical Deep Dive on Learning Efficiency {#chapter-28.-technical-deep-dive-on-learning-efficiency}

***Note:** this chapter elaborates on concepts introduced in [chapter 17](#bookmark=id.7fkp59khmotz) and [chapter 21](#bookmark=id.8xtr7duh3gfd).*

***Summary:** We introduce the concept of theoretical maximum learning efficiency, analogous to the physical phenomenon that nothing can travel faster than the speed of light. The maximum learning efficiency for a given knowledge graph depends on its encompassing density – however, a knowledge graph does not have to be fully encompassed, or even nearly fully encompassed, for its maximum learning efficiency to approach the theoretical limit. Math Academy’s mathematical knowledge graph contains enough encompassings that its maximum learning efficiency is close to the theoretical limit. In practice, the actual learning efficiency attained by an individual student depends primarily on the student’s quality of performance, and to a lesser extent on their pace (the average amount of work completed each day).*

### What is Learning Efficiency? {#what-is-learning-efficiency?}

#### | Theoretical Maximum Learning Efficiency {#|-theoretical-maximum-learning-efficiency}

In physics, nothing can travel faster than the speed of light. It is the theoretical maximum speed that any physical object can attain. A universal constant.

In the context of spaced repetition, there is an analogous concept: **theoretical maximum learning efficiency.** In theory, given a sufficiently encompassed body of knowledge, it is possible to complete all your spaced repetitions without ever having to explicitly review previously-learned material.

As a simple demonstration, consider a sequence of topics where the first topic is fully encompassed by the second, which is fully encompassed by the third, which is fully encompassed by the fourth, and so on.

![][image75]

Each time you learn the next topic, all the topics below receive full implicit repetitions. Assuming that you never run out of new topics to learn, the only reason you would ever need to do an explicit repetition is if you get stuck repeatedly attempting and failing to learn the next topic.

It’s important to realize that *a graph does not have to be fully encompassed, or even nearly fully encompassed, for its maximum learning efficiency to approach the theoretical limit.* Even if most relationships between topics are non-encompassing, a considerable minority of encompassings goes a long way.

For instance, Math Academy’s mathematical knowledge graph contains enough encompassings that its maximum learning efficiency is close to the theoretical limit. We have empirically observed that, in practice, most mathematical courses can be learned with roughly only one explicit review per topic on average. In theory, a perfect student who aced every single learning task would need even fewer explicit reviews.

#### | Theoretical Minimum Learning Efficiency {#|-theoretical-minimum-learning-efficiency}

By contrast, there is also a concept of **theoretical minimum learning efficiency.** This is precisely the setting of independent flashcards – or equivalently, a set of topics without any encompassings.

![][image76]

In this setting, no topic can receive implicit repetitions from any other topic. Every single review must be done explicitly.

![][image77]

It’s worth emphasizing that, unlike Math Academy, other spaced repetition systems do *not* leverage the power of encompassings and therefore implement theoretical *minimum* learning efficiency.

### Factors that Impact Learning Efficiency {#factors-that-impact-learning-efficiency}

Remember that to achieve maximum learning efficiency, Math Academy uses a process that we call **repetition compression**. We gather all topics that have due repetitions, and then compress this set into a much smaller set of tasks that

1. covers all of the due repetitions, and

2. will lead to the greatest overall gain in spaced repetitions across your entire knowledge profile.

You can think of Math Academy as a turbo-boosted educational engine, where repetition compression is our combustion mechanism.

But remember that an engine can’t actually move a car unless it is supplied with gas and oil. The gas is needed to produce the energy that moves the car, and the oil is needed to prevent friction from locking up the engine.

The same applies to Math Academy. In order to experience the turbo-boosting,

1. you have to put in a sufficient **amount** of work that can be converted into educational progress, and

2. the **quality** of your work has to be high enough to avoid excessive friction during the learning process.

#### | Performance {#|-performance}

By looking at your **performance** (pass rate and accuracy) across various types of learning tasks, we are able to calculate a **learning efficiency percentage** that estimates how close you are to the maximum possible efficiency for your course.

If you maintain a high learning efficiency, then you can make a lot of progress in your course by doing a relatively small amount of work. But if you have a low learning efficiency, then you will have to do significantly more work to make the same amount of progress.

| Learning Efficiency | How Much Work to Complete a Course |
| :---: | :---: |
| 1.00 | 1x |
| 0.80 | 1.25x |
| 0.67 | 1.5x |
| 0.50 | 2x |
| 0.25 | 4x |

#### | Pace {#|-pace}

In Math Academy, work is measured in **eXperience Points (XP).** One XP represents one minute of fully-focused, fully-productive work for an average serious (but imperfect) student. The amount of XP that you complete per weekday (on average) is called your **pace.** 

##### \> Learning Efficiency vs Pace {#>-learning-efficiency-vs-pace}

Although the quality of your work is the single greatest factor that affects your learning efficiency, your pace can affect your learning efficiency as well. The faster you push your knowledge frontier forward, the further your knowledge frontier is ahead of your due reviews, and the more likely it is that we can find good topics to “knock out” a large number of your due reviews.

We empirically determined the following relationship:

[![][image78]](https://www.codecogs.com/eqnedit.php?latex=%5Ctextrm%7Blearning%20efficiency%7D%20%5Cpropto%20%5Ctextrm%7Bpace%7D%5E%7B0.1%7D#0)

This means that if you double your pace, your learning efficiency increases by about 20.1 \= 7%. Likewise, if you cut your pace in half, your learning efficiency decreases by about 7%.

##### \> Pace vs Time to Completion {#>-pace-vs-time-to-completion}

In a normal class period during a school day, it’s reasonable to expect at least 40 minutes of fully-focused, fully-productive work. This corresponds to a baseline pace of 40 XP per weekday. 

When we benchmark the amount of XP in our courses, we simulate an average student who is serious but imperfect and works at a pace of 40 XP per weekday. On average, courses contain about 3000 XP assuming that a student knows all the necessary prerequisites (though this can vary a lot depending on the amount of material that must be covered, e.g. prealgebra is \~2000 XP but precalculus is \~4000 XP).

Below is a table that shows how long it would take you to complete a 3000 XP course, depending on your pace. Note that learning efficiencies are computed relative to the baseline pace of 40 XP/weekday (so a pace of 40 XP/weekday corresponds to an efficiency of 1, and higher paces correspond to efficiencies greater than 1).

| Pace  *(XP/weekday)* | Efficiency Multiplier  *(pace/40)0.1* | How Long To Complete a Course Benchmarked at 3000 XP  *weekdays \= 3000/(pace\*multiplier)* |
| :---: | :---: | :---: |
| 160 | 1.15 | 3 weeks |
| 80 | 1.07 | 7 weeks |
| 40 | 1.00 | 15 weeks |
| 20 | 0.93 | 32 weeks |
| 10 | 0.87 | 69 weeks (\~1.3 years) |
| 5 | 0.81 | 148 weeks (\~3 years) |

To put this in perspective: in a traditional classroom, each weekday involves 50 minutes of class plus the same duration of homework after school. On this schedule, it takes students a full school year (36 weeks) to complete a course.

![][image79]

But if you spent the same amount of time working on Math Academy (100 XP/weekday), you could finish in just 5-6 weeks\! That’s more than a 6x speedup – and you don’t have to be a genius to achieve it. Remember, we’re talking about an average student who is serious but imperfect.

Even for a massive course like AP Calculus BC (which is benchmarked at about 6000 XP, twice as big as an average course), the speedup is still over 3x. And if you factor in all the extra time you’d spend studying for quizzes, midterms, finals, and the AP test itself in a traditional class, which is already included in Math Academy’s 6000 XP benchmark, it’s a 4x speedup.

On the flipside, if you tried to use Math Academy like a phone game and only did a couple of minutes per day, it could take you nearly a decade to learn a traditional school year’s worth of math.

For this reason, we highly recommend that you maintain a pace of *at least* 15 XP per weekday if you want to experience the benefits of Math Academy. But really, the higher your pace, the better.

## Chapter 29\. Technical Deep Dive on Prioritizing Core Topics {#chapter-29.-technical-deep-dive-on-prioritizing-core-topics}

***Note:** this chapter elaborates on concepts introduced in [chapter 4](#bookmark=id.pinaf5gi9iat)*.

***Summary:** Math Academy prioritizes core topics that are most relevant in the “big picture” of mathematics. No topics are skipped, but because all topics are continually reviewed, covering core topics first allows students to get more practice and therefore develop a greater degree of automaticity on the core topics by the end of the course. This is advantageous because the core topics are the ones that appear more frequently as prerequisites of other topics in mathematics. We also observed that roughly a third of 4th grade through AP Calculus BC topics, while necessary to meet standards, were not actually prerequisites for university math, so we created a streamlined Mathematical Foundations (MF) course sequence that cuts out those topics and consists of mostly core topics. The MF sequence is geared towards adult learners who want to pursue advanced university courses as soon as possible but lack the necessary foundational knowledge.*

### Core and Supplemental Topics {#core-and-supplemental-topics}

When a student progresses through a course, Math Academy prioritizes **core topics** first, that is, the topics that are most relevant in the “big picture” of mathematics. For instance, in calculus, the product rule would be a core topic, while the intermediate value theorem would be a **supplemental topic**.

Of course, a student taking the calculus course will of course cover both core and supplemental topics. No topics are skipped; it’s just a matter of the order in which they are covered. Because students cover core topics first and continue practicing them throughout the course, they get more practice and therefore develop a greater degree of automaticity on the core topics by the end of the course. This is advantageous because the core topics are the ones that appear more frequently as prerequisites of other topics in mathematics.

Math Academy employs a proprietary intelligent algorithm to automatically identify core topics in its knowledge graph. At a high level, the idea is to satisfy two competing conditions:

1. If a topic is core, then all of its ancestor topics (i.e. its prerequisites, their prerequisites, and so on) must also be core.

2. However, in each standard course in the knowledge graph, there must be some balance between core and supplemental topics – for instance, we should not label all topics as core, or all topics as supplemental, even though both of those cases would technically satisfy the preceding condition. The specific balance may vary depending on the connectivity of topics in the course and their relationship to topics in other courses.

### The Mathematical Foundations Sequence {#the-mathematical-foundations-sequence}

After developing a comprehensive curriculum that covers all the standards for 4th grade through AP Calculus BC, as well as plenty of advanced university courses, we found that roughly a third of 4th grade through AP Calculus BC topics were not actually prerequisites for university math. So, we created a streamlined **Mathematical Foundations (MF)** course sequence that cuts out those topics and consists of mostly core topics.

The MF sequence is geared towards adult learners who want to pursue advanced university courses as soon as possible but lack the necessary foundational knowledge. Whether an adult is starting off again with the basics or just needs to brush up on calculus, our Mathematical Foundations sequence is the fastest and most efficient way to get up to speed with the mathematical concepts and tools that are necessary to excel in university-level mathematics.

![][image80]





