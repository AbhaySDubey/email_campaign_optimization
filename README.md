# Email Campaign Optimization

## Introduction

This project aims to prepare a model that can help optimise email marketing campaign for an e-commerce company provided the details regarding the number of past purchases and the country the user resides in.

---

## Problem Statement

(As stated in the provided case study's problem description)

### Goal
Optimizing marketing campaigns is one of the most common data science tasks. Among the many marketing tools available, emails stand out as particularly efficient.

Emails are great because they are free, scalable, and can be easily personalized. Email optimization involves personalizing the content and/or the subject line, selecting the recipients, and determining the timing of the sends, among other factors. Machine Learning excels at this.

### Case Description

The marketing team of an e-commerce site has launched an email campaign. This site has email addresses from all the users who created an account in the past.

They have chosen a random sample of users and emailed them. The email lets the user know about a new feature implemented on the site. From the marketing team perspective, success is if the user clicks on the link inside of the email. This link takes the user to the company site.

You are in charge of figuring out how the email campaign performed and were asked the following questions:

- What percentage of users opened the email and what percentage clicked on the link within the email?
- The VP of marketing thinks that it is stupid to send emails in a random way. Based on all the information you have about the emails that were sent, can you build a model to optimize in future how to send emails to maximize the probability of users clicking on the link inside the email?
- By how much do you think your model would improve click through rate (defined as # of users who click on the link/total users who receive the email). How would you test that?
- Did you find any interesting pattern on how the email campaign performed for different segments of users? Explain.

---

## Workflow

### 1. Visualization and Analysis

**I'll begin with visualizing the features and analyizing their impact on link clicking rates:**

 - I've joined the email_opened and link_clicked tables for ease.
 - I'll use heatmaps, bar plots, etc. for visualizations between the link click rate and email opened rate against all the features provided.
 - I'll also try to visualize the relationships between 2 or more features and their impact on the link click rate and email opened rate.

### 2. Training a Binary Classification Model

**Next, I'll train a model to predict what users will click on the link provided in the email:**

 - First up, I'll one-hot encode the features:
    - email_text
    - email_version
    - user_country
 - I'll also transform the local time("hour" column) to the corresponding sine and cosine transformations, as it's better suited for classification tasks, allowing me to work with the cyclic nature of time (23 hours is just 1 hour away from 0 hours which is not interpreted the same way if I'd leave the values as they are provided in integral formats).
 - I'm training a Gradient Boosting Classifier for the binary classification task.

### 3. Preparing model for Optimizing Email Deliveries

- Here, I'll use the binary classifier model trained in the previous step to prepare a model that can help in optimizing how to send emails to maximize probability of users clicking on the link given inside the email.
- For this, I'll require the past purchase details and the country the user resides in as inputs (which as stated in the problem description will be available to us).
- Next, I'll be considering all possible combinations of weekdays, local time, email versions/personalizations, etc. to predict the most optimal combinations, returned as a list of dictionaries sorted based on decreasing probabilities of link clicks.

---

## Inferences

**Some of the inferences I was able to draw out of the data provided are:**

- the higher the number of past purchases the higher the chance of clicking on the link:
    - users with over 20 purchases have a 100% probability of clicking on the link

- users recieving a personalized email generally tend to click on the link

- the trend of users from different countries in clicking the links is, as follows:
    - users from US have the highest rate of link clicking, and emails sent throughout the week with a higher preference for Wednesday around midnight or during 10 am - 1 pm with personalized emails have higher link click rate
    - users from UK have the second highest rate of link clicking, and emails sent throughout the week around midnight or during 10 am - 1 pm or in the evening with personalized short-text emails have higher link click rate
    - users from FR have the third highest rate of link clicking, and emails sent on Monday, Wednesday, Thursday and Saturday around 9 pm with personalized short-text emails have higher link click rate
    - users from ES have the highest rate of link clicking, and emails sent on Monday, Tuesday and Wednesday throughout the day except during nighttime with personalized emails have higher link click rate

---

## Future Scope

- I'd really like to optimize the binary classification model further. There are quite a few issues given the imbalance in the dataset, so I'd try to hopefully work around that a little better.
- The current ***"email delivery optimization"*** isn't really as *efficient* and *robust* as I think it can be made. So, I would try to improve that, as well.