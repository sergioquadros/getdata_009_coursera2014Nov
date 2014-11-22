---
title: "Code Book"
author: "Sérgio Henrique Silveira de Quadros"
date: "2014-11-21"
output: html_document
---
30 volunteers(19-48 years) was separeted randomly in two sets: **test** with 2947 observations and **train** with 7352 observations.

|subject|sets|
|---|---|
|2,4,9,10,12,13,18,20,24|test|
|1,3,5,6,7,8,11,14,15,16,17,19,21,22,23,25,26,27,28,29,30|train|

Both groups realized six activities - WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING - wearing a smartphone under video-recorded to label the data manually.
The smartphone's sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings in unnamed columns/window that was stored in **Inertials Signals** files for each set). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. 
From **Inertials Signals** files in **test** directory, the **X_test.txt** was calculated 561 variables for each observation that was named in a peculiar mode by authors and this names was stored in **features.txt**. This names was repeated and used forbidden characters that in this work I corrected with ```make.names(features, unique = TRUE)```. That variables'names from 303th to 316th columns was repeated in 317:330 and 331:344 respectively and gained sufixes _2_ and _3_; at similar way it occured in more two blocks: 382:423 and 461:502. Values inside each block wasn't equals, so i'd kept those names with apends. This choice named columns from 3rd to 563th.

I gave to first variable the name "subject" whose values stayed at **subject_test.txt** and second column's name was "activity" to values in **y_test.txt**. This values was integers from 1 to 6 and they was replaced with more descriptive activity values: WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING.



