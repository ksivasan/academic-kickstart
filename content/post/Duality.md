+++
# Date this page was created.
date = "2017-04-11"

# Project title.
title = "Automated Measurement of Language Outcomes for Neurodevelopmental Disorders - NIDCD R01"

# Project summary to display on homepage.
summary = "Improving conversational use of spoken language is an important goal for many new interventions and treatments for children with neurodevelopmental disorders. However, progress in testing these treatments is limited by the lack of informative outcome measures to indicate whether or not an intervention or treatment is having the desired effect on a child's conversational use of language (i.e., discourse skills). The goal of this project is to evaluate whether Natural Language Processing methods can be translated into meaningful outcome measure for individuals with a range of neurodevelopmental disorders. This project was recently funded by the National Institute of Deafness and Other Communication Disorders."

# Optional image to display on homepage (relative to `static/img/` folder).
image_preview = ""

# Tags: can be used for filtering projects.
# Example: `tags = ["machine-learning", "deep-learning"]`
tags = ["natural-language-processing", "autism", "outcomes"]

# Optional external URL for project (replaces project detail page).
external_link = ""

# Does the project detail page use math formatting?
math = false

# Optional featured image (relative to `static/img/` folder).
[header]
image = ""
caption = ""

+++


While I was working on a problem using Power method (thanks to STAT545), I stopped to check why power method works and what is matrix deflation. I swear I had numerous courses on Linear Algebra before but did not know about this (weird education system). Before going further, let's stop and get familiarized with what is power method.

For a given matrix, if you want to find the dominant eigen value and eigen vector, you can simply multiply the matrix with a random vector, normalize the resulting vector and again, multiply the matrix with this new resultant vector. Repeat the process till it converges. You can use Rayleigh method to recover the eigen value. You can use this method to find the rest of the eigen vectors and eigen values too. Let's see how to do that.

The concept of spectral shift is used to find the second dominant eigen value and eigen vector. In order to do this spectral shift, we need to find another matrix B whose eigen values are same as the original matrix A except the dominant eigen value of A is replaced by zero in B. The new matrix B may or may not have all eigen values similar but the first dominant and second dominant eigen vectors must be same. Wow, if your head is spinning - you are on track! Although there are trivial approaches to find a representative matrix B that solves problem at hand, there are a big chunk of matrices that can be solution to this problem. Why? To answer this, you need to understand spectral decomposition. 

Spectral decomposition says that a matrix can be thought as something formed of individual rank 1 matrices formed by vectors that make them.  
$A=\sum \lambda_i x_i y_i^T$ But where is the point $y_i$ coming from if we thought the matrix A is formed of points. That's where we need to realize the duality of matrices. For a given matrix A, we can think the matrix is either formed by arranging a set of points in row-wise or column wise. Since these two exist, we have two eigen vector pair representing the dominant direction in each scenario. Ie. if arranged row wise, we get a dominant eigen vector which has a partner (or dual) in other direction. This is also called as dual basis which says a matrix made of left eigen vectors say M and right eigen vectors say, N has their product giving an identity. Vola! Thus, the two coordinate systems are related by this!

Coming back to our original problem, if we want to find second dominant eigen value and eigen vector, we remove the contribution of the first dominant eigen value and eigen vector, and then, we have n-1 to 1 more eigen vectors that make up the matric B. 
