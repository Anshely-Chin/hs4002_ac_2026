# HS4002 Week 8 Progress Report

## Contents of this repository
#### "Table" folder
Contains tables for descriptive statistics of the sample, coefficients and average marginal effects for the logistic regression models

- Table 1: Descriptive statistics
- Table 2: Model 1's coefficients and AMEs
- Table 3: Model 2's coefficients and AMEs
- Table 4: Model 3's coefficients and AMEs (including interaction effect)
- Model comparisons of Model 1, 2 and 3 (Table 5)
- Interaction plot

#### "Code" folder
Contains the Jupyter notebook used to run data for this project. Exploratory models have been commented out (with #). The codes have been arranged to run in order of the variables created.

#### "Output" folder
Contains raw output from the notebook

## Project Overview
Broadly, we test whether identity non-verification is associated with mental health outcomes and whether this relationship is moderated by network diversity, such that low verification is more strongly associated with suicide ideation in more homogeneous networks.

This project examines whether identity verification acts as a protective mechanism against suicide ideation. We further examine whether having diverse ties, where the individual can activate and verify alternative identities, alters the protective capacity of identity verification for adolescents whose student identities are verified and those whose student identities are not. We test this relationship using logistic regression models, using average marginal effects of different identities and the interaction effect.

## Dataset
This proposal uses data from the National Longitudinal Study of Adolescent Health (Add Health) dataset (Hummer et al. 2026), which contains a nationally representative sample of adolescents in grade 7 to 12 in the United States. For this proposal, we draw from the Wave 1 Dataset 1 (DS1) and Dataset 3 (DS3). Both were conducted in the 1994-1995 school year, through an in-school questionnaire and in-home interview. 

The dataset is the public-use dataset, which contains only 1/3 of the actual sample and includes only generic survey data. Sensitive data such as relationship identifiers were removed. For example, Wave 1 DS3 only contains overall network data, without the exact friendship nominations (see https://addhealth.cpc.unc.edu/restricted-use-vs-public-use-data-files/).

For this proposal, we created a maximum of 6 possible role identities that the adolescents can have (student, child, religious, vocational, best friend and romantic partner).

## LLM-use declaration for writing code
The plans on how to create the variables were ours, but we did use chatbots to get the code on how to work with the variables. For example, we prompted the chatbots with questions like "what is the python code to sum variables / convert string data to numerical data / recode a variable to a binary variable / python code to find AMEs for logistic regression / visualising interaction effects".

## Variables
### Dependent variable: suicide ideation
Variable name -> *suicide*

Taken from respondents’ answers to the questions: “during the past 12 months, did you ever seriously think about committing suicide?”. These were coded as a binary variable (0 = no, 1 = yes)

### Independent variables
#### Student identity verified
Variable name -> *idstudent_binary*

The student identity is the only identity that we assume all adolescents to possess, given that the Add Health study was administered in school. Hence, this variable measures verification of the student identity, created using their responses to five original variables:

I feel like I am part of this school, 
I have a lot of good qualities, 
I am happy to be at this school, 
I feel socially accepted, 
I feel safe in my school

Each variable was originally coded as (1) strongly agree, (2) agree, (3) neither agree nor disagree, (4) disagree, (5) strongly disagree.

Each variable was converted from string data to numerical data, then reverse coded so that (5) became "strongly agree" and (1) became "strongly disagree", so that the greater value indicated greater verification. For each respondent, I obtained the mean value of the sum of the five values. Finally, it was recoded into a binary variable where any scores >= 4 were recoded as "Identity verified = 1" and the rest were recoded as "Identity not verified = 0".

#### Child identity
Variable name -> *idchild_binary*

This variable was created using the adolescents' responses to two original variables:

How much do you think your father cares about you? / 
How much do you think your mother cares about you?

The original variables were coded on a scale of “1 = not at all” to “5 = very much”. As some respondents were missing one parent, we consider the child identity to be possibly activated as long as the adolescent has one parent. I took the maximum number out of the two variables, where the greater value indicated higher identity verification. It was then recoded into a binary variable where any scores >= 4 were recoded as "Identity available = 1" and the rest were recoded as "Identity not available = 0".

#### Religious identity
Variable name -> *idreligion* 

This variable was created by recoding the adolescents' responses to: "How important is religion to you?". 

The original variable was coded on a scale of (1) very unimportant, (2) fairly unimportant, (3) fairly important, (4) very important. 

These were recoded as a binary variable:

(3) fairly important, (4) very important = 1

(1) very unimportant, (2) fairly unimportant = 0

We renamed the columns "Identity available = 1" and "Identity not available = 0" to match the other variables.

#### Vocational identity
Variable name -> *idworker* 

This variable was created by recoding the adolescents' responses to: "In the last 4 weeks, did you work for money outside of home?". These were coded as a binary variable (0 = no, 1 = yes). We also renamed the columns "Identity available = 1" and "Identity not available = 0" to match the other variables.

#### Best friend identity
Variable name -> *havebestfriend*

This variable was created using adolescents' nominations of alters (found in Wave 1, DS3). 

We draw from the original dummy variables "Ego nominated a male best friend" and "Ego nominated a female best friend". Both variables were coded as "1 = Ego nominated a male/female best friend, 0 = Ego did not nominate a male/female best friend". 

We took the sum of the variables and recoded it into an ordinal variable: (2) has a male and female best friend, (1) has either a male or female best friend, (0) has no best friends. This implies that an adolescent who has at least one best friend will have the best friend identity available, while one with no best friends will not have the best friend identity available.

The initial consideration for keeping the male and female best friend nominations separate was to possibly account for gendered differences, but we did not continue with it for this proposal.

#### Romantic partner identity
Variable name -> *romanticpartner*

This variable was created by recoding the adolescents' responses to: "In the last 18 months, have you had a special romantic relationship with anyone?". These were coded as a binary variable (0 = no, 1 = yes). Again, we simply renamed the columns "Identity available = 1" and "Identity not available = 0" to match the other variables.

### Moderator: availability of alternative ties
Variable name -> *heterogeneity*

This variable represents the total number of role identities an adolescent can possibly have. But first, we recoded the [havebestfriend] variable into a binary variable [idfriend_binary], where having >= 1 best friend meant that the best friend identity was available. Like the other binary variables, it was recoded as "Identity available = 1" and "Identity not available = 0".

The [heterogeneity] variable was then created by taking the sum of the five other role identities [idchild_binary, idreligion, idworker, idfriend_binary, romanticpartner]. For example, an adolescent whose heterogeneity value is 5 has five other possible avenues to activate and verify a different role identity. In other words, they have 5 other alternative identities available if the student identity is not verified. A current limitation is that there are no variables that also measure verification of the alternative identities.

## Data
The total sample size was 6502. We made a subset [suicide, idstudent_binary, idchild_binary, idreligion, idworker, havebestfriend, romanticpartner] and dropped rows with any missing variables. The final working sample size in this proposal was 3847. Descriptive statistics are presented in Table 1.

## Logistic regression
As the dependent variable [suicide] is a binary variable, we ran a logistic regression. Table 2 presents the results of the logistic regression, with the main effects of the independent variables in log-odds and average marginal effects.

Table 2's model: *suicide ~ idstudent_binary + idchild_binary + idreligion + idworker + havebestfriend + romanticpartner*

Table 3 presents the second logit model including the heterogeneity moderator. It reports the main effects of the variables in log-odds and average marginal effects.

Table 3's model: *suicide ~ idstudent_binary + idchild_binary + idreligion + idworker + havebestfriend + romanticpartner + heterogeneity*

Finally, since this project aims to examine whether the effect of identity non-verification is moderated by network heterogeneity, Table 4 presents the third logit model which includes the interaction effect between the student identity and network heterogeneity.

Table 4's model: *suicide ~ idstudent_binary + idchild_binary + idreligion + idworker + havebestfriend + romanticpartner + heterogeneity + idstudent_binary:heterogeneity*

## Data visualisations
### Marginal effects interaction plot
To understand whether having alternative identities (or role heterogeneity) moderates suicide ideation for adolescents' whose student identities are verified or fail to be verified, we made an interaction plot using *predictions* function from the *marginaleffects* package, isolating the interaction effect [idstudent_binary:heterogeneity]. The graph plotted the suicide ideation risk for every adolescent based on whether or not their student identity was verified, holding all their other role identity availability constant. This is intended to test whether accumulating more alternative identities can reduce suicide ideation when the primary identity (in this case, the student identity) is verified.

LLM-use declaration: we wanted to make a graph to show this effect but did not know what function to use. So we prompted a chatbot with questions like "python code for visualising an interaction effect with a dependent variable in a logit regression".
