
<a href="https://www.bigdatauniversity.com"><img src = "https://ibm.box.com/shared/static/wbqvbi6o6ip0vz55ua5gp17g4f1k7ve9.png" width = 300, align = "center"></a>
# <center>K-Nearest Neighbors</center>

<img src = "https://ibm.box.com/shared/static/860wrw1jvullt57vl470fe7zikucwzzh.png", height = 400, width = 400, align = 'right'>
<img src = "https://ibm.box.com/shared/static/f7wewzfjxozemzlhsf7tay1me0alyofa.png", height = 150, width = 150, align = 'left'>

### <b>Welcome to Lab 2a of Machine Learning 101 with Python.</b>
<p><b>Machine Learning is a form of artificial intelligence (AI), where the system can "learn" without explicitly being coded</b></p>

In this lab exercise, you will learn the about the different evaluation models and metrics. You will be able to identify the strengths and weaknesses of each model and how to incorporate the Bias-Variance trade-off to them.


### Some Notebook Commands Reminders:
<ul>
    <li>Run a cell: CTRL + ENTER</li>
    <li>Create a cell above a cell: a</li>
    <li>Create a cell below a cell: b</li>
    <li>Change a cell to Markdown: m</li>
    
    <li>Change a cell to code: y</li>
</ul>

<b> If you are interested in more keyboard shortcuts, go to Help -> Keyboard Shortcuts </b>

<b> <i> Before starting the lab, please run the following code in order to access the solutions </i> </b>


```python
from IPython.core.display import HTML
HTML("""
<style type="text/css">
    #ans:hover { background-color: black; }
    #ans {padding: 6px; 
        background-color: white; 
        border: green 2px solid;
        font-weight: bold}
</style>
""") 
```





<style type="text/css">
    #ans:hover { background-color: black; }
    #ans {padding: 6px; 
        background-color: white; 
        border: green 2px solid;
        font-weight: bold}
</style>




# K-Nearest Neighbors 
In this Lab you will load the Skulls dataset data, fit the data, and use K-Nearest Neighbors to predict a data point. But what is **K-Nearest Neighbors**?

**K-Nearest Neighbors** is an algorithm for supervised learning. Where the data is 'trained' with data points corresponding to their classification. Once a point is to be predicted, it takes into account the 'K' nearest points to it to determine it's classification.

### Here's an visualization of the K-Nearest Neighbors algorithm.

<img src = "https://ibm.box.com/shared/static/mgkn92xck0z05v7yjq8pqziukxvc2461.png">

In this case, we have data points of Class A and B. We want to predict what the star (test data point) is. If we consider a k value of 3 (3 nearest data points) we will obtain a prediction of Class B. Yet if we consider a k value of 6, we will obtain a prediction of Class A.

In this sense, it is important to consider the value of k. But hopefully from this diagram, you should get a sense of what the K-Nearest Neighbors algorithm is. It considers the 'K' Nearest Neighbors (points) when it predicts the classification of the test point.

---
## <u>Train/Test Split on the Skulls Dataset with K-Nearest Neighbors</u>

### Import Libraries
Import the Following Libraries:
<ul>
    <li> numpy (as np) </li>
    <li> pandas </li>
    <li> KNeighborsClassifier from sklearn.neighbors </li>
</ul>


```python
import numpy as np 
import pandas 
from sklearn.neighbors import KNeighborsClassifier
```

Hover over the green box below for the answer
<p id="ans"> 
<font color = "white"> import numpy as np <br>
import pandas <br>
from sklearn.neighbors import KNeighborsClassifier</font>
</p>

Using my_data as the <b>skulls.csv</b> data read by panda, declare variables <b>X</b> as the <b>Feature Matrix</b> (<i>data of my_data</i>) and <b>y</b> as the <b>response vector</b> (<i>target</i>)<br>
<i>Note: Use the <b>target</b> function for the <b>response vector</b> and the <b>removeColumns</b> function for the <b>Feature Matrix</b> </i>


```python
my_data = pandas.read_csv("https://vincentarelbundock.github.io/Rdatasets/csv/HSAUR/skulls.csv", delimiter=",")
```


```python
my_data.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>epoch</th>
      <th>mb</th>
      <th>bh</th>
      <th>bl</th>
      <th>nh</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>c4000BC</td>
      <td>131</td>
      <td>138</td>
      <td>89</td>
      <td>49</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>c4000BC</td>
      <td>125</td>
      <td>131</td>
      <td>92</td>
      <td>48</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>c4000BC</td>
      <td>131</td>
      <td>132</td>
      <td>99</td>
      <td>50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>c4000BC</td>
      <td>119</td>
      <td>132</td>
      <td>96</td>
      <td>44</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>c4000BC</td>
      <td>136</td>
      <td>143</td>
      <td>100</td>
      <td>54</td>
    </tr>
  </tbody>
</table>
</div>




```python
def target(numpyArray, targetColumnIndex):
    target_dict = dict()
    target = list()
    count = -1
    for i in range(len(my_data.values)):
        if my_data.values[i][targetColumnIndex] not in target_dict:
            count += 1
            target_dict[my_data.values[i][targetColumnIndex]] = count
        target.append(target_dict[my_data.values[i][targetColumnIndex]])
    return np.asarray(target)
```


```python
# Remove the column containing the target name since it doesn't contain numeric values.
# Also remove the column that contains the row number
# axis=1 means we are removing columns instead of rows.
# Function takes in a pandas array and column numbers and returns a numpy array without
# the stated columns
def removeColumns(pandasArray, *column):
    return pandasArray.drop(pandasArray.columns[[column]], axis=1).values
```


```python
X = removeColumns(my_data, 0, 1)
y = target(my_data, 1)
```

Hover over the green box below for the answer
<p id="ans"><font color = "white">X = removeColumns(my_data, 0, 1)<br>
y = target(my_data, 1)</font>
</p>

Now to perform <b>train/test split</b> we have to split the <b>X</b> and <b>y</b> into two different sets: The <b>training</b> and <b>testing</b> set. Luckily there is a sklearn function for just that!

Import the <b>train_test_split</b> from <b>sklearn.cross_validation</b>


```python
from sklearn.cross_validation import train_test_split
```

    /home/tambaf/.local/lib/python2.7/site-packages/sklearn/cross_validation.py:41: DeprecationWarning: This module was deprecated in version 0.18 in favor of the model_selection module into which all the refactored classes and functions are moved. Also note that the interface of the new CV iterators are different from that of this module. This module will be removed in 0.20.
      "This module will be removed in 0.20.", DeprecationWarning)


Hover over the green box below for the answer
<p id="ans"><font color = "white">
from sklearn.cross_validation import train_test_split</font>
</p>

Now <b>train_test_split</b> will return <b>4</b> different parameters. We will name this <b>X_trainset</b>, <b>X_testset</b>, <b>y_trainset</b>, <b>y_testset</b>. The <b>train_test_split</b> will need the parameters <b>X</b>, <b>y</b>, <b>test_size=0.3</b>, and <b>random_state=7</b>. The <b>X</b> and <b>y</b> are the arrays required before the split, the <b>test_size</b> represents the ratio of the testing dataset, and the <b>random_state</b> ensures we obtain the same splits.


```python
 X_trainset, X_testset, y_trainset, y_testset = train_test_split(X, y, test_size=0.3, random_state=7)
```

Hover over the green box below for the answer
<p id="ans"> 
<font color = "white">
X_trainset, X_testset, y_trainset, y_testset = train_test_split(X, y, test_size=0.3, random_state=7)</font>
</p>

Now let's print the shape of the training sets to see if they match.


```python
print X_trainset.shape
print y_trainset.shape
print y_trainset
```

    (105, 4)
    (105,)
    [1 2 2 3 0 0 1 3 0 3 2 2 1 0 0 1 2 3 3 1 0 4 0 0 4 3 0 3 1 1 4 3 0 1 0 4 3
     1 1 1 2 3 0 4 3 3 0 3 0 4 3 1 0 4 2 4 4 3 1 0 4 0 1 2 4 4 3 2 0 4 0 1 2 2
     4 1 2 2 4 3 1 4 0 1 2 1 0 0 1 2 0 4 4 2 4 1 3 2 2 0 4 3 3 2 0]


Hover over the green box below for the answer
<p id="ans">
<font color = "white">
print X_trainset.shape<br>
print y_trainset.shape</font>
</p>

Let's check the same with the testing sets! They should both match up!


```python
print X_testset.shape
print y_testset.shape
```

    (45, 4)
    (45,)


Hover over the green box below for the answer
<p id="ans"> 
<font color = "white">
print X_testset.shape<br>
print y_testset.shape</font>
</p>

Now similarly with the last lab, let's create declarations of <b>KNeighborsClassifier</b>. Except we will create <b>3</b> different ones:<br>
<b>neigh</b>   -> <b>n_neighbors = 1</b> <br>
<b>neigh23</b> -> <b>n_neighbors = 23</b> <br>
<b>neigh90</b> -> <b>n_neighbors = 90</b> <br>


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white">
neigh = KNeighborsClassifier(n_neighbors = 1)<br>
neigh23 = KNeighborsClassifier(n_neighbors = 23)<br>
neigh90 = KNeighborsClassifier(n_neighbors = 90)</font>
</p>

Now we will fit each instance of <b>KNeighborsClassifier</b> with the <b>X_trainset</b> and <b>y_trainset</b>


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white">
neigh.fit(X_trainset, y_trainset)<br>
neigh23.fit(X_trainset, y_trainset)<br>
neigh90.fit(X_trainset, y_trainset)</font>
</p>

Now in our last lab, we only predicted for <b>one</b> datapoint. However, you are able to predict with <b>multiple</b> datapoints. We can do this by just passing in the <b>y_testset</b> which contains multiple test points into a <b>predict</b> function of <b>KNeighborsClassifier</b>.

Let's pass the <b>y_testset</b> in the <b>predict</b> function each instance of <b>KNeighborsClassifier</b> but store it's returned value into <b>pred</b>, <b>pred23</b>, <b>pred90</b> (corresponding to each of their names)



```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white">
pred = neigh.predict(X_testset)<br>
pred23 = neigh23.predict(X_testset)<br>
pred90 = neigh90.predict(X_testset)</font>
</p>

Awesome! Now let's compute neigh's <b>prediction accuracy</b>. We can do this by using the <b>metrics.accuracy_score</b> function


```python
from sklearn import metrics
print("Neigh's Accuracy: "), metrics.accuracy_score(y_testset, pred)
```

Interesting! Let's do the same for the other instances of KNeighborsClassifier.


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white">
print("Neigh23's Accuracy: "), metrics.accuracy_score(y_testset, pred23)<br>
print("Neigh90's Accuracy: "), metrics.accuracy_score(y_testset, pred90)</font>
</p>

As shown, the accuracy of <b>neigh23</b> is the highest. When <b>n_neighbors = 1</b>, the model was <b>overfit</b> to the training data (<i>too specific</i>) and when <b>n_neighbors = 90</b>, the model was <b>underfit</b> (<i>too generalized</i>). In comparison, <b>n_neighbors = 23</b> had a <b>good balance</b> between <b>Bias</b> and <b>Variance</b>, creating a generalized model that neither <b>underfit</b> the data nor <b>overfit</b> it.

---
## <u>Train/Test Split on the Diabetes Dataset with Linear Regression</u>

#### Now similar to the previous lab, we will be working with the Diabetes Dataset.

### Importing Libraries
Import the following libraries: 
    <ol>- <b>load_diabetes</b> from <b>sklearn.datasets</b></ol>
    <ol>- <b>LinearRegression</b> from <b>sklearn.linear_model</b></ol>
    <ol>- <b>matplotlib.pyplot</b> (as <b>plt</b>) </ol>
<p>Ensure that you include <b>%matplotlib inline</b> to allow the plots to show up in the notebook </p>


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white">
from sklearn.datasets import load_diabetes <br>
from sklearn.linear_model import LinearRegression <br>
import matplotlib.pyplot as plt <br>
%matplotlib inline</font>
</p>

Now create an instance of the diabetes data set by using the <b>load_diabetes</b> function as a variable called <b>diabetes</b>.


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white"> diabetes = load_diabetes()</font>
</p>

We will also be working with <b>one feature</b> like the last lab.


```python
diabetes_X = diabetes.data[:, None, 2]
```

Now create an instance of the LinearRegression called LinReg


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white"> LinReg = LinearRegression()</font>
</p>

Now let's use <b>diabetes_X</b> as the <b>Feature Matrix</b> and <b>diabetes.target</b> as the <b>response vector</b> and split it up using <b>train_test_split</b> function we imported earlier (<i>If you haven't, please import it</i>). The <b>train_test_split</b> function should have <b>test_size = 0.3</b> and a <b>random state = 7</b>. It should return values to <b>X_trainset</b>, <b>X_testset</b>, <b>y_trainset</b>, <b>y_testset</b> as in previous declarations.


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white">
X_trainset, X_testset, y_trainset, y_testset = train_test_split(diabetes_X, diabetes.target, test_size=0.3, random_state=7)</font>
</p>

Train the <b>LinReg</b> model using <b>X_trainset</b> and <b>y_trainset</b>


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white"> LinReg.fit(X_trainset, y_trainset)</font>
</p>

Now let's <i>plot</i> the graph (similar to last time lab)
<p> Use plt's <b>scatter</b> function to plot all the datapoints of <b>X_testset</b> and <b>y_testset</b> and color it <b>black</b> </p>
<p> Use plt's <b>plot</b> function to plot the line of best fit with <b>X_testset</b> and <b>LinReg.predict(X_testset)</b>. Color it <b>blue</b> with a <b>linewidth</b> of <b>3</b>. </p> <br>
<b>Note</b>: Please ignore the FutureWarning. 


```python

```

Hover over the green box below for the answer
<p id="ans">
<font color = "white"> plt.scatter(X_testset, y_testset, color='black')<br>
plt.plot(X_testset, LinReg.predict(X_testset), color='blue', linewidth=3)</font>
</p>

Now the prediction line will be based off of the <b>training</b> data. In comparsion, you can see a slight offset, but in general it reflects a good <b>prediction</b>.

## Want to learn more?

IBM SPSS Modeler is a comprehensive analytics platform that has many machine learning algorithms. It has been designed to bring predictive intelligence to decisions made by individuals, by groups, by systems – by your enterprise as a whole. A free trial is available through this course, available here: [SPSS Modeler for Mac users](https://cocl.us/ML0101EN_SPSSMod_mac) and [SPSS Modeler for Windows users](https://cocl.us/ML0101EN_SPSSMod_win)

Also, you can use Data Science Experience to run these notebooks faster with bigger datasets. Data Science Experience is IBM's leading cloud solution for data scientists, built by data scientists. With Jupyter notebooks, RStudio, Apache Spark and popular libraries pre-packaged in the cloud, DSX enables data scientists to collaborate on their projects without having to install anything. Join the fast-growing community of DSX users today with a free account at [Data Science Experience](https://cocl.us/ML0101EN_DSX)

---
# Additional Resources
<br>
K-Nearest Neighbors Algorithm: https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm 
<br><br>
Cross-Validation: http://scikit-learn.org/stable/modules/cross_validation.html 
<br><br>
Model Evaluation: http://scikit-learn.org/stable/modules/model_evaluation.html 


<hr>
Copyright &copy; 2016 [Big Data University](https://bigdatauniversity.com/?utm_source=bducopyrightlink&utm_medium=dswb&utm_campaign=bdu). This notebook and its source code are released under the terms of the [MIT License](https://bigdatauniversity.com/mit-license/).​
