---
title: "Code Book"
author: "Sérgio Henrique Silveira de Quadros"
date: "2014-11-21"
output: html_document
---
30 volunteers(19-48 years) was separeted randomly in two sets: **test** and **train**

|subject|sets|
|---|---|
|2,4,9,10,12,13,18,20,24|test|
|1,3,5,6,7,8,11,14,15,16,17,19,21,22,23,25,26,27,28,29,30|train|

Both groups realized six activities - WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING - wearing a smartphone under video-recorded to label the data manually.
The smartphone's sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window that was stored in Inertials Signals files). The sensor acceleration signal, which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body acceleration and gravity. The gravitational force is assumed to have only low frequency components, therefore a filter with 0.3 Hz cutoff frequency was used. 



