---
layout: page
mathjax: true
coursetitle: RBE474X/595-A01-SP -- Deep Learning for Perception
title: Nifty Neural Networks!
permalink: /rbe474x/fall2024a/proj/p1/
---

Table of Contents:
- [1. Deadline](#due)
- [2. Starter Package](#starterpkg)
- [3. Parts 1 to 3: The 474X Package](#part14)
- [4. Parts 4 and 5: The 595 Package](#part56)
- [5. Submission Guidelines](#sub)
  - [5.1. File tree and naming](#files)
  - [5.2. Report](#report)
- [6. Allowed and Disallowed functions](#funcs)
- [7. Collaboration Policy](#coll)
- [8. Acknowledgements](#ack)

<a name='due'></a>
## 1. Deadline 
**11:59:59 PM, Sept 02, 2024. Group Submissions (Groups of 3).**

<a name='due'></a>
## 2. Starter Package
The Starter Package with the code and data can be downloaded from <a href="https://github.com/pearwpi/rbe474x_p1">here</a>. The starter code is in a <a href="https://jupyter.org/">Jupyter notebook</a> with instructions.

<a name='part14'></a>
## 3. Parts 1 to 3: The 474X Package

This part has to be completed for both the undergraduate and graduate version of the course. The details can be found in the Parts 1 to 3 of the `.ipynb` file.

<a name='part56'></a>
## 4. Parts 4 and 5: The 595 Package

This part has to be completed for graduate version of the course along with parts 1 to 3. The details can be found in Parts 4 and 5 of the `.ipynb` fil. Undergraduates can complete this part for a maximum of 25% extra credit.


<a name='sub'></a>

## 5. Submission Guidelines

**If your submission does not comply with the following guidelines, you'll be given ZERO credit.**

### 5.1. File tree and naming

Your submission on ELMS/Canvas must be a ``zip`` file, following the naming convention ``GroupGROUPNUM_p1.zip``. If your group number is ``4``, then the submission file should be named ``Group4_p1.zip``. The `GROUPNUM` can be found on Canvas. The file **must have the following directory structure**. Do not change the files to run the code. You can have any helper functions in sub-folders as you wish, be sure to index them using relative paths and if you have command line arguments for your codes, make sure to have default values too. Please provide detailed instructions on how to run your code in ``README.md`` file. 

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
Furthermore, the size of your submission file should <b>NOT</b> exceed more than <b>100MB</b>.
</p>

The file tree of your submission <b>SHOULD</b> resemble this:

```
GroupGROUPNUM_p1.zip
├── src
|    ├── main_notebook.ipynb
|    ├── artifacts/
|    ├── Other code files
|    ├── mlp.pth
|    ├── cnn_inbuilt.pth
|    └── cnn_custom.pth
├── Report.pdf 
└── README.md
```


<a name='report'></a>

### 5.2. Report 
For each part of the project, explain briefly what you did, and describe any interesting problems you encountered and/or solutions you implemented. You must include the following details in your writeup:

- Your report **MUST** be typeset in LaTeX in the IEEE Tran format provided to you in the ``Draft`` folder and should of a conference quality paper. Feel free to use any online tool to edit such as [Overleaf](https://www.overleaf.com) or install LaTeX on your local machine.
- Answer all the questions asked in the Jupyter notebook in the LaTeX report. Include outputs if asked/required to explain your answer. Try to always include outputs if applicable, these could be images (if output is an image) or numbers in some cases. 


<a name='funcs'></a>

## 6. Allowed and Disallowed functions

<b> Allowed:</b>

- Any functions regarding reading, writing and displaying/plotting images in `cv2`, `matplotlib`
- Basic math utilities including convolution operations in `numpy` and `math`
- Any functions for pretty plots
- Any functions for debugging, plotting (TensorBoard, Weights and Biases and so on) and metric implementation (confusion matrix)
- Usage of ChatGPT (or any other LLM) is allowed as long as you include the prompts used in your report and blatantly do not plagiarize from ChatGPT (includes copy pasting entire code)

<b> Disallowed:</b>

- Any function that implements in-part or full the assignment including usage of LLMs such as ChatGPT to write code for this


If you have any doubts regarding allowed and disallowed functions, please drop a public post on [Piazza](https://piazza.com/wpi/fall2024/rbe474x). 

<a name='coll'></a>

## 7. Collaboration Policy

<p style="background-color:#ddd; padding:5px">
<b>NOTE:</b> 
You are <b>STRONGLY</b> encouraged to discuss the ideas with your peers. Treat the class as a big group/family and enjoy the learning experience. 
</p>

However, the code should be your own, and should be the result of you exercising your own understanding of it. If you reference anyone else's code in writing your project, you must properly cite it in your code (in comments) and your writeup. For the full honor code refer to the [RBE474X/595-A01-SP Fall 2024 A-term website](https://pear.wpi.edu/teaching/rbe474x/fall2024a.html).

<a name='ack'></a>

## 8. Acknowledgements

This project was partly inspired by University of Maryland's CMSC764 (Advanced Numerical Optimization) course.

