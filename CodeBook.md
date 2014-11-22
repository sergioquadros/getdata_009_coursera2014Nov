---
title: "Code Book"
author: "Sérgio Henrique Silveira de Quadros"
date: "2014-11-21"
output: html_document
---
We used the data from the HAR dataset [in this link](<https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip>), it has 30 volunteers(19-48 years) that was separeted randomly in two sets: **test** with 2947 observations and **train** with 7352 observations.

|subject|sets|
|---|---|
|2,4,9,10,12,13,18,20,24|test|
|1,3,5,6,7,8,11,14,15,16,17,19,21,22,23,25,26,27,28,29,30|train|

Both groups realized six scheduled activities wearing a smartphone under video-recorded that was labeled manually by researchers. I gave to first variable the name "subject" whose values stayed at **subject_test.txt** and second variable's name whose values stayed at **y_test.txt**. This values was integers from 1 to 6 and they was replaced with more descriptive activity values: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING.
In the same way the train set had treated.

The smartphone's sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings in unnamed columns/window that was stored in **Inertials Signals** files for each set). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. 

From **Inertials Signals** files in **test** directory, the **X_test.txt** was calculated 561 variables for each observation that was named in a peculiar mode by authors and this names was stored in **features.txt**. This names was repeated and they used forbidden characters that in this work I corrected with ```make.names(features, unique = TRUE)```. That variables' names from 303th to 316th columns was repeated in 317:330 and 331:344 respectively and gained sufixes _2_ and _3_; at similar way it occured in more two blocks: 382:423 and 461:502. Values inside each block in **X_test.txt** wasn't equals, so i'd kept those names with apends. This choice named variables from 3rd to 563th columns. The authors achieved similar set with train group.

For a quick genealogy in variables' names from 3rd to 563th were achieved using a Fast Fourier Transformation, **FFT**, so it makes two domains: time with **t** prefix for 3rd up to 267th; frequency with **f** prefix for another ones. 
We have two main components detected by our sensors: linear and angular acceleration that could be affected by gravity, magnetic field and movement's mode like jerks and spins in that subjects' bodies. So they was combined and born this myriad of names, but not only that, we also want one big collection of statistics and physical quantities like mean, standard deviation, maximum, minimum, skewness, kurtosis, irq, sma, entropy, energy and his bands, etc for each axis.

My recipe for getting tidy data with this [run_analisys.R](<https://github.com/sergioquadros/getdata_009_coursera2014Nov/blob/master/run_analisys.R>) script in this case was:
+  Download and unzip directories and files;
+  Creating a set of working directories with relative path names;
+  Reading files and giving descriptive labels for variables:
  +  "**subject**" for values in **subject_test.txt** and **subject_train.txt**;
  +  "**activity**" for values in **y_test.txt** and **y_train.txt**;
  +  Suiting variables' names in **feafures.txt** for from 3rd to 563th columns.
+  Naming columns of values in **X_test.txt** and **X_train.txt** with suited variables in **feafures.txt**
+  Suiting descriptive values for "**activity**" variable in **y_test.txt** and **y_train.txt**;
+  Binding columns:
  +  In this order named **subject_test.txt**, named and suited **y_test.txt** and **X_test.txt** for **test** data frame;
  +  In this order named **subject_train.txt**, named and suited **y_train.txt** and **X_train.txt** for **train** data frame.
+  Biding rows for **set** and **train** data frames to make a tidy set named **first_tidy** and printing in a text file format **first_tidy.txt**;
+  Summarizing the **first_tidy** set with mean and sd for each column from 3rd to 563th and printing as **tidy2.txt**, my choice putting in each column the name of variable, his mean and his sd;
+  Summarizing the **first_tidy** set grouped by **subject** and **activity** with mean and printing as **tidy3.txt**.

Many thanks for course staff and colleagues, also for Forum's tips and course materials.

## References
Visit for more information in Jorge Reyes et al.:

+  <http://www.jucs.org/jucs_19_9/energy_efficient_smartphone_based>

+  <https://sites.google.com/site/jorgereyesresearch/publications>

+  <http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones>








