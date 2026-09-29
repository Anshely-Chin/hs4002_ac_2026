# HS4002 Week 8 Progress Report

## Project Overview
Broadly, we test whether identity non-verification is associated with mental health outcomes and whether this relationship is moderated by network diversity, such that low verification is more strongly associated with suicide ideation in more homogeneous networks.

## Dataset
This proposal uses data from the National Longitudinal Study of Adolescent Health (Add Health) dataset (Hummer et al. 2026), which contains a nationally representative sample of adolescents in grade 7 to 12 in the United States. For this proposal, we draw from the Wave 1 Dataset 1 (DS1) and Dataset 3 (DS3). Both were conducted in the 1994-1995 school year, through an in-school questionnaire and in-home interview. 

The dataset is the public-use dataset, which contains only 1/3 of the actual sample and includes only generic survey data. Sensitive data such as relationship identifiers were removed. For example, DS3 only contains dummy variables of network data.

We created a maximum of 6 possible role identities that the adolescents can have (student, child, religious, vocational, best friend and romantic partner).

## Variables
### Dependent variable: suicide ideation
Variable name -> "suicide"

Taken from respondents’ answers to the questions: “during the past 12 months, did you ever seriously think about committing suicide?”. These were coded as a binary variable (0 = no, 1 = yes)

### Independent variables
#### Student identity verified
Variable name -> idstudent_binary

The student identity is the only identity that we assume all adolescents to possess, given that the Add Health study was administered in school. Hence, this variable measures verification of the student identity, created using their responses to five original variables:

I feel like I am part of this school, 
I have a lot of good qualities, 
I am happy to be at this school, 
I feel socially accepted, 
I feel safe in my school

Each variable was originally coded as (1) strongly agree, (2) agree, (3) neither agree nor disagree, (4) disagree, (5) strongly disagree.

Each variable was converted from string data to numerical data, then reverse coded so that (5) became "strongly agree" and (1) became "strongly disagree", so that the greater value indicated greater verification. For each respondent, I obtained the mean value of the sum of the five values. Finally, it was recoded into a binary variable where any scores >= 4 were recoded as "Identity verified = 1" and the rest were recoded as "Identity not verified = 0".

#### Child identity
Variable name -> idchild_binary

This variable was created using the adolescents' responses to two original variables:

How much do you think your father cares about you? / 
How much do you think your mother cares about you?

The original variables were coded on a scale of “1 = not at all” to “5 = very much”. As some respondents were missing one parent, we consider the child identity to be possibly activated as long as the adolescent has one parent. I took the maximum number out of the two variables, where the greater value indicated higher identity verification. It was then recoded into a binary variable where any scores >= 4 were recoded as "Identity verified = 1" and the rest were recoded as "Identity not verified = 0".

#### Religious identity
Variable name -> idreligion 

This variable was created by recoding the adolescents' responses to: "How important is religion to you?". 

The original variable was coded on a scale of (1) very unimportant, (2) fairly unimportant, (3) fairly important, (4) very important. 

These were recoded as a binary variable:

(3) fairly important, (4) very important = 1
(1) very unimportant, (2) fairly unimportant = 0

We renamed the columns "Identity verified = 1" and "Identity not verified = 0" to match the other variables.

#### Vocational identity
Variable name -> idworker 

This variable was created by recoding the adolescents' responses to: "In the last 4 weeks, did you work for money outside of home?". These were coded as a binary variable (0 = no, 1 = yes). We also renamed the columns "Identity verified = 1" and "Identity not verified = 0" to match the other variables.

#### Best friend identity
Variable name -> havebestfriend

This variable was created using adolescents' nominations of alters (found in Wave 1, DS3). 

We draw from the original dummy variables "Ego nominated a male best friend" and "Ego nominated a female best friend". Both variables were coded as "1 = Ego nominated a male/female best friend, 0 = Ego did not nominate a male/female best friend". 

We took the sum of the variables and recoded it into an ordinal variable: (2) has a male and female best friend, (1) has either a male or female best friend, (0) has no best friends

#### Romantic partner identity
Variable name -> romanticpartner

This variable was created by recoding the adolescents' responses to: "In the last 18 months, have you had a special romantic relationship with anyone?". These were coded as a binary variable (0 = no, 1 = yes). Again, we simply renamed the columns "Identity verified = 1" and "Identity not verified = 0" to match the other variables.
