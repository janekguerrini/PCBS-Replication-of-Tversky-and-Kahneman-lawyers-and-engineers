PCBS Project: Replication of Tversky and Kahneman's "Lawyers and Engineers"
============================================================================================

<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

  - [Introduction](#Introduction)
  - [Hypothesis](#Hypothesis)
  - [Description of the stimuli](#description-of-the-stimuli)
  - [Setting up the materials](#setting-up-the-materials)
  - [Instructions](#instructions)
  - [Experiment](#experiment)
  - [Demographics](#demographics)
  - [Experience and Class Review](#experience-and-class-review)
  - [Reference](#reference)

<!-- markdown-toc end -->


## Introduction ##
Kahneman & Tversky (1973) provided some important insight into how people deal with probability tasks in which they have to integrate prior probabilities with specific evidence. One of the most famous experiments of that study, "Lawyers and Engineers" (henceforth L&E) was constructed as follows. People were given a description corresponding to an invidivual which was randomly chosen from a sample of 100 people. There were two experimental conditions:
+ One group of subjects was told that the sample consisted of 70 engineers and 30 lawyers.
+ The other group of subjects was told that the sample consisted of 30 engineers and 70 lawyers.
The description were like the following:
> «Jack is a 45-year-old man. He is married and has four children. He is generally conservative, careful, and ambitious. He shows no interest in political and social issues and spends most of his free time on his many hobbies which include home carpentry, sailing, and mathematical puzzles.»
Then, subjects were asked to write down the probability that the subject was one of the 30 (or 70, respectively) engineers.

The results was that subjects produced essentially the same probability judgments. From this, Tversky and Kahneman concluded that subjects evaluated the likelihood that a specific description belonged to an engineer rather than to a lawyer by the degree to which this description was representative of the two stereotypes, with little or no regard for the prior probabilities of the categories.

There is extensive evidence that the result is partly determined by the triggering of pragmatic interpretive processes.Schwarz, Strack, Hilton & Naderer (1991), for instance, showed that the so-called representativeness effect is smaller when participants are told that the description is generated and randomly picked by a computer.

We hypothesise that along the human origin of the description, also the long, full-fledged psychological description triggers interpretive processes. People pragmatically interpret the description as being a hint by the experimenter. To get rid of this confound, we replicate the study with one-line descriptions that are not said to be psychologically determined.

The experiment will be run on Mechanical Turk and programmed in JavaScript language with the jspsych library. [Jspsych](https://www.jspsych.org) is a JavaScript library for running behavioral experiments in a web browser created by Josh de Leeuw. The library provides a flexible framework for building a wide range of laboratory-like experiments that can be run online.

## Description of the stimuli ##

The stimuli are constructed as follows: we have six pairs of real-world professions and a description associated with each pair.

**Table 1**


| Name of the scenario  | hypothesis 1 | hypothesis 2 | description/evidence |
| ------------- | ------------- | ------------- | ------------- |
| `airplane`  | flight attendant | commercial airline pilot | is over 35 years old |
| `medicine`  | nurse  | doctor  |  has taken advanced biology courses |
| `soccer`  | elite soccer player  | elite soccer coach  | spends a lot of time studying soccer tactics  |
|  `university` | student  | professor  | 	spends a lot of time at the university  |
| `church`  | congregant  | pastor | spends one afternoon a week reading the bible  |
| `politics`  | voter  | politician | is well informed about topical issues and policy making  |
| `lawyers&engineers`  | lawyer  | engineer | 	loves solving Rubik’s cubes  |

Participants see sentences of the form:
> Consider an individual drawn from a group of `first prior` `hypothesis 1` and `second prior` `hypothesis 2`.
Now you learn that this individual `evidence`.
What is the probability that he is a `hypothesis 1`?

We have six conditions, corresponding to different priors feeded to the participants:

**Table 2**


| | `first prior`| no given prior | 10 | 30 | 50 | 70 | 90 |
|---|---|---|---|---|---|---|---|
|`second prior`| |||||||
|no given prior||Group 1||||||
|90|||Group 2|||||
|70||||Group 3||||
|50|||||Group 4|||
|30||||||Group 5||
|10|||||||Group 6|

Summing up, every participant sees:
+ all the profession pairs with the corresponding evidence.
+ only one combination of priors corresponding to her/his group.

## Setting up the materials ##

We set up the battery of materials that we will feed the script.   

``` javascript
//Add materials to this array. The materials here are
//inflected in all the ways we need them to inflect in
//order to piece together the questions in the script.

var materials = [
  { h1_plural: "students",
    h1_predicate: "a student",
    h2_plural: "professors",
    h2_predicate: "a professor",
    e: "spends a lot of time at the university",
    id: "university"
  },
  {h1_plural: "voters",
    h1_predicate: "a voter",
    h2_plural: "politicians",
    h2_predicate: "a politician",
    e: "is well-informed about topical issues and policy-making",

    id: "politics"
  },
  {h1_plural: "nurses",
    h1_predicate: "a nurse",
    h2_plural: "doctors",
    h2_predicate: "a doctor",
    e: "has taken advanced biology courses",

    id: "medicine"
  },
  {h1_plural: "elite soccer players",
 h1_predicate: "an elite soccer player",
 h2_plural: "elite soccer coaches",
 h2_predicate: "an elite soccer coach",
 e: "spends a lot of time studying soccer tactics",

 id: "soccer"
},
 {h1_plural: "congregants",
   h1_predicate: "a congregant",
   h2_plural: "pastors",
   h2_predicate: "a pastor",
   e: "spends one afternoon a week reading the bible",

   id: "church"
 },
 {h1_plural: "lawyers",
   h1_predicate: "a lawyer",
   h2_plural: "engineers",
   h2_predicate: "an engineer",
   e: "loves solving Rubik's cubes",

   id: "T&K"
 },
 {h1_plural: "flight attendants",
   h1_predicate: "a flight attendant",
   h2_plural: "commercial airline pilots",
   h2_predicate: "a commercial airline pilot",
   e: "is over 35 years old",

   id: "airplane"
 }

];
```

## Instructions ##


```javascript
// Instructions
var instructions_html = ["<h1>Welcome!</h1><p>In this experiment, you will read one or more sentences and then be asked some questions about them. For example:</p><p class=\"example\">Consider an individual drawn from a group of 100 butchers and fishmongers.</p><p class=\"example target\">Now you learn that this individual weigh products before selling them.</p><p> What is the probability that this individual is a nutcher? </p><p>Click <i>Next</i> for another example.</p>"];

instructions_html.push("<p>Here's another example of the kind of question we may ask you.</p><p class=\"example\"> Consider an individual drawn from a group of 100 firefighters and police officers.</p><p class=\"example target\"></p><p class=\"example target\"> Now you learn that this individual likes order and tidiness. </p><p> What is the probability that this individual is a police officer? You will be given a small textbox to type in your guess. <p><p> That's all the examples we have for you, click <i>Next</i> to begin the experiment.</p>");

var instructions = {
  type: 'instructions',
  show_clickable_nav: true,
  pages: instructions_html,
  data: { questionId: "instructions" }
};
```

## Experiment (main script) ##
```javascript
var theSubject = jsPsych.randomization.randomID(10);
jsPsych.data.addProperties({ subjectId: theSubject,
                             condition: 1 });

// Read materials and shuffle them
materials = jsPsych.randomization.shuffle(materials);

// Set up variables we'll need for loops, and empty blocks to be filled
// in with questions
var i;
var first_prior;
var second_prior;
var question_probability = new Array;

//  pairs of priors of the different conditions are taken from the different
// URLs of the dispatcher. The getURLVariable function returns the value of a
// variable passed in through the query string.See documentation at
// https://www.jspsych.org/core_library/jspsych-data/#jspsychdatageturlvariable
var first_prior = jsPsych.data.getURLVariable('first_prior')
var second_prior = jsPsych.data.getURLVariable('second_prior')

// Stimulus sentence: gets the priors from the URL of the main html file, loads
//the content from the materials battery.
for (i = 0; i < materials.length; i++) {
  question_probability.push(
    { type: "survey-text",
      preamble: "<p class=\"preamble\">Consider an individual randomly drawn from a group of "+ first_prior +" " + materials[i].h1_plural + " and "+ second_prior +" " + materials[i].h2_plural + ".</p>" + "Now you learn that this individual " + materials[i].e + ".</p>",
      questions: [
        { prompt: "<p class=\"question\">What's the probability that this person is a " + materials[i].h1_predicate + "?</p>",
          columns: 5,
          rows: 1,
          value: ""}],
        data: { questionId: "question_probability" + materials[i].id }
  });
};


var theTimeline = new Array;
theTimeline.push(instructions);
theTimeline = theTimeline.concat(question_probability);
theTimeline.push(demographics_original);
theTimeline.push(demographics_page1);
theTimeline.push(demographics_page2);


jsPsych.init({
  timeline: theTimeline,
  show_progress_bar: true,
  on_finish: function(data){  jsPsych.data.displayData('csv');

  }
});

```


## Demographics ##
```javascript
// Demographic questionnaire
var demographics_original = {
  type: "survey-multi-choice",
  preamble: "<h1>Demographic questionnaire <br>(page 1 of 3)</h1><p>Please answer the following demographic questions.  Your payment will not be affected by your answers.</p>",
  questions: [
    { prompt: "What is your college-level background in philosophy, linguistics, and logic?",
      options: [ "----",
                 "None",
                 "1 undergraduate-level course",
                 "2 or more undergraduate-level courses",
                 "1 or more graduate-level courses" ],
      labels: [ "NA", "none", "1ug", "2+ug", "1+g" ] },
    { prompt: "What is your college-level background in mathematics and statistics?",
      options: [ "----",
                 "None",
                 "1 undergraduate-level course",
                 "2 or more undergraduate-level courses",
                 "1 or more graduate-level courses" ],
      labels: [ "NA", "none", "1ug", "2+ug", "1+g" ] },
    { prompt: "What is your gender?",
      options: ["----", "Male", "Female", "Other" ],
      labels: [ "NA", "male", "female", "other"] },
    { prompt: "How old are you?",
      options: [ "----",
                 "Less than 18",
                 "18 to 24",
                 "25 to 34",
                 "35 to 44",
                 "45 to 54",
                 "55 to 64",
                 "65 to 74",
                 "75 or older" ],
      labels: [ "NA", "-18", "18-24", "25-34", "35-44", "45-54", "55-64", "65-74", "75+" ]
    } ],
  data: { questionId: "demo" }
};

var demographics_page1 = {
      preamble: "<h1>Demographic questionnaire(page 2 of 3)</h1><p>Please answer the following demographic questions.  Your payment will not be affected by your answers.</p>",
      type: "survey-multi-select",
      questions: [
        { prompt: "<p>Which of the following categories, if any, are you a member of?</p>",
          options: [ "university student",
                     "university professor",
                     "voter",
                     "politician",
                     "nurse",
                     "doctor",
                     "elite soccer player",
                     "elite soccer coach",
                     "regular churchgoer",
                     "pastor",
                     "lawyer",
                     "engineer",
                     "flight attendant",
                     "commercial airline pilot"
                   ]
        },
        { prompt: "<p>This what a test about general knowledge of professional categories. It is important for us to know whether you have any technical knowledge or informed opinion about these categories. This could mean for example having a close friend/family member who works in the field or in a closely related field, or your regularly reading specific news articles concerning the field.</p>",
          options: [ "university student",
                     "university professor",
                     "voter",
                     "politician",
                     "nurse",
                     "doctor",
                     "elite soccer player",
                     "elite soccer coach",
                     "regular churchgoer",
                     "pastor",
                     "lawyer",
                     "engineer",
                     "flight attendant",
                     "commercial airline pilot"
                     ]
        },
        { prompt: "<p>From the answers you gave above, pick the one you feel most strongly about. How strongly do you feel about that?</p>",
          options: [ "weak/superficial",
                     "average",
                     "strong/specific",
                     "very strong/very specific"
                     ]
        }],
          data: { questionId: "demo1" }
    };

var demographics_page2 = {
  type: "survey-multi-select",
  preamble: "<h1>Demographic questionnaire<br>(page 3 of 3)</h1>",
  questions: [
    { prompt: "What is the highest degree or level of school you have completed?",
        options: [ "----",
                   "Less than a high school degree",
                   "High school degree or equivalent",
                   "Some college, no degree",
                   "Associate degree",
                   "Bachelor's degree",
                   "Master's degree",
                   "Professional degree",
                   "Doctorate"],
        labels: [ "NA", "notHS", "HS", "CnotD", "AD", "Bsc", "Msc", "PD", "PhD" ]},
        { prompt: "What is the highest degree or level of school your parents have completed?",
      options: [ "----",
                 "Less than a high school degree",
                 "High school degree or equivalent",
                 "Some college, no degree",
                 "Associate degree",
                 "Bachelor's degree",
                 "Master's degree",
                 "Professional degree",
                 "Doctorate"],
      labels: [ "NA", "notHS", "HS", "CnotD", "AD", "Bsc", "Msc", "PD", "PhD" ]}
            ],
  data: { questionId: "demo2" }
};
```
## Main html file and dispatcher ##

We set up a central html file that loads the jspsych plugins we need for our code and loads the different parts of the experiment. The jspsych plugins are publicly available. I uploaded them to my public html folder in order to not take up space in the GitHub folder.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta author="Janek Guerrini">
    <title>Experiment on reasoning</title>
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
    <script src="https://ajax.googleapis.com/ajax/libs/jqueryui/1.10.3/jquery-ui.min.js"></script>
    <link href="https://ajax.googleapis.com/ajax/libs/jqueryui/1.10.3/themes/black-tie/jquery-ui.min.css"
          rel="stylesheet" type="text/css">


    <script src="http://web-risc.ens.fr/~jguerrini/experiments"></script>

    <script src="http://web-risc.ens.fr/~jguerrini/experiments/jsPsych-6.1.0/jspsych.js"></script>
    <script src="http://web-risc.ens.fr/~jguerrini/experiments/jsPsych-6.1.0/plugins/jspsych-survey-text.js"></script>
        <script src="http://web-risc.ens.fr/~jguerrini/experiments/jsPsych-6.1.0/plugins/jspsych-survey-multi-choice.js"></script>
    <script src="http://web-risc.ens.fr/~jguerrini/experiments/jsPsych-6.1.0/plugins/jspsych-instructions.js"></script>
    <script src="http://web-risc.ens.fr/~jguerrini/experiments/jsPsych-6.1.0/plugins/jspsych-survey-multi-select.js"></script>


    <link href="http://web-risc.ens.fr/~jguerrini/experiments/jsPsych-6.1.0/css/jspsych.css" rel="stylesheet">

  // restyling?

  </head>
  <body>
    <div id="jspsych-target"></div>
  </body>

  <script src="materials.js"></script>
  <script src="other_questions.js"></script>
  <script src="script.js"></script>

</html>

```

We set up the dispatcher with the URL that calls the html file, by passing in through the query string the variables that determine the participant condition and which we need in our main script.

 ```php
<?php
// Get a global counter from server and update counter
$counter_file = "/tmp/dispatch-counter-eco-1";
$counter_val = file_get_contents($counter_file);
file_put_contents($counter_file, $counter_val + 1);

// Possible redirections
$redirects = array("?first_prior=90&second_prior=10"
                   "?first_prior=70&second_prior=30"
                   "?first_prior=50&second_prior=50"
                   "?first_prior=30&second_prior=70"
                   "?first_prior=10&second_prior=90";

// Redirect user to the next link
header("Location: " . $redirects[$counter_val % count($redirects)]);
?>
```

This will not work on the GitHub folder, but will be useful for when the experiment will be on the lab server. To take the experiment from the Github folder, click on one of the following paths (one per condition; it's always the same file, but with different URL variables passed in):
* [Condition 1](main.html?first_prior=90&second_prior=10)
* [Condition 2](main.html?first_prior=70&second_prior=30)
* [Condition 3](main.html?first_prior=50&second_prior=50)
* [Condition 4](main.html?first_prior=70&second_prior=30)
* [Condition 5](main.html?first_prior=90&second_prior=10)
* [Condition 6](main.html?first_prior= &second_prior= )


## Experience and Class Review ##
I had very little coding experience prior to this class. I knew a bit of Python and Javascript, but I had no idea how to solve coding problems. This has been the main thing I learned in this class. Other helpful things I learned: how to use git, how to efficiently get help through slack.

I found it very useful to learn by trial and error in the bigger project, while that method was less clear to me when at the beginning we solved small coding exercises: sometimes I just didn't know where to start, and when that happens one does not progress at all. Getting input from the instructors on the way one looks for help when stuck in coding was certainly one of the main accomplishments of this course for me.

As to what missed, not really a lot, apart from the absence of structure that comes physiologically with a course that involves students of so different coding levels. One way to give this structure  might be to give weekly assignments, maybe partly linked to the final projects, starting from the very beginning of the semester, and on which all students get regular  input (not only if they ask for it). This would nudge students into having a more gradual workflow and asking more for help, both on slack and during the class. I personally find asking for help the only real way to progress, as coding alone too often leaves one lost in useless Stackoverflow threads, difficult documentation, incomprehensible error message decipherment, and white sheet panic.

## Reference ##
Kahneman, D., & Tversky, A. (1973). On the psychology of prediction. Psychological review, 80(4), 237.
