
# Implement Statistics with Functions

## SWBATs
* Create functions to model measures of central tendency and dispersion
* Perform basic statistical analysis of given data using measures of central tendency and dispersion. 

## Introduction 
In this lab you'll dive deep into calculating the measures of central tendency and dispersion introduced in previous lessons. You will implement the code the formulas for these functions in python which will require you to use the programming skills that you have gained in first two sections of the module. So let's get started with this.

### Dataset

For this lab, we'll use the [NHIS dataset](http://people.ucsc.edu/~cdobkin/NHIS%202007%20data.csv) containing weights, heights and some other attributes for a number of surveyed individuals. The context of this survey is outside the scope this lab, so we'll just go ahead and load the heights column as a list for us to run some simple statistical experiments. We'll use the pandas library to import the data into our python environment. This process will be covered in detail in the next section. Let's do this for you to give you a head start.  


```python
import pandas as pd
df = pd.read_csv('nhis.csv')
df.head()
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
      <th>HHX</th>
      <th>FMX</th>
      <th>FPX</th>
      <th>SEX</th>
      <th>BMI</th>
      <th>SLEEP</th>
      <th>educ</th>
      <th>height</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>33.36</td>
      <td>8</td>
      <td>16</td>
      <td>74</td>
      <td>260</td>
    </tr>
    <tr>
      <th>1</th>
      <td>20</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>26.54</td>
      <td>7</td>
      <td>14</td>
      <td>70</td>
      <td>185</td>
    </tr>
    <tr>
      <th>2</th>
      <td>69</td>
      <td>1</td>
      <td>2</td>
      <td>2</td>
      <td>32.13</td>
      <td>7</td>
      <td>9</td>
      <td>61</td>
      <td>170</td>
    </tr>
    <tr>
      <th>3</th>
      <td>87</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>26.62</td>
      <td>8</td>
      <td>14</td>
      <td>68</td>
      <td>175</td>
    </tr>
    <tr>
      <th>4</th>
      <td>88</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>27.13</td>
      <td>8</td>
      <td>13</td>
      <td>66</td>
      <td>168</td>
    </tr>
  </tbody>
</table>
</div>



We are only interested in the heigt column, so we'll save this now as a list.


```python
height = df.height.values.tolist()
print (len(height))
print (height)
```

    4785
    [74, 70, 61, 68, 66, 98, 99, 70, 65, 64, 65, 66, 99, 61, 61, 65, 68, 64, 64, 66, 67, 72, 61, 96, 65, 61, 66, 64, 72, 64, 64, 71, 64, 72, 70, 66, 74, 76, 68, 66, 66, 69, 61, 64, 66, 69, 66, 71, 71, 63, 68, 75, 64, 62, 96, 65, 64, 60, 62, 61, 63, 72, 72, 63, 67, 65, 64, 61, 66, 65, 68, 64, 71, 65, 63, 64, 72, 65, 71, 72, 64, 65, 72, 69, 66, 64, 96, 66, 96, 69, 67, 66, 59, 60, 62, 70, 61, 66, 97, 96, 62, 67, 59, 70, 66, 60, 68, 67, 65, 71, 63, 63, 71, 67, 65, 66, 66, 70, 68, 64, 62, 60, 73, 68, 62, 61, 65, 66, 64, 60, 59, 61, 63, 98, 60, 62, 63, 65, 68, 72, 67, 72, 66, 61, 96, 64, 96, 64, 71, 72, 65, 67, 63, 68, 65, 96, 71, 62, 69, 64, 66, 63, 67, 69, 66, 61, 68, 60, 72, 70, 65, 67, 63, 69, 64, 96, 67, 74, 62, 69, 60, 73, 96, 67, 98, 62, 69, 64, 68, 72, 74, 72, 64, 66, 68, 65, 61, 69, 71, 66, 65, 68, 65, 68, 68, 72, 62, 60, 72, 71, 65, 62, 74, 71, 67, 67, 66, 69, 67, 66, 96, 72, 65, 61, 68, 98, 68, 70, 68, 70, 71, 65, 69, 98, 68, 70, 67, 60, 65, 63, 74, 67, 67, 63, 67, 69, 96, 67, 65, 64, 72, 72, 67, 63, 64, 65, 68, 65, 64, 99, 64, 61, 60, 63, 69, 96, 96, 64, 70, 73, 65, 68, 98, 70, 60, 67, 70, 66, 67, 66, 72, 73, 69, 67, 63, 70, 69, 63, 60, 70, 66, 70, 67, 66, 60, 96, 69, 62, 67, 64, 72, 70, 67, 68, 72, 61, 65, 69, 65, 66, 69, 66, 63, 64, 62, 72, 67, 72, 62, 67, 64, 67, 62, 64, 69, 64, 71, 60, 70, 69, 66, 69, 96, 96, 66, 70, 75, 69, 73, 71, 69, 66, 67, 72, 62, 96, 64, 65, 73, 70, 96, 70, 64, 73, 60, 96, 70, 70, 71, 73, 63, 68, 63, 67, 70, 64, 68, 62, 71, 72, 64, 69, 67, 69, 69, 70, 98, 62, 76, 96, 63, 65, 70, 75, 65, 70, 66, 68, 98, 65, 68, 64, 62, 62, 66, 64, 73, 66, 65, 64, 67, 67, 65, 99, 66, 67, 70, 72, 62, 67, 72, 68, 63, 70, 60, 65, 61, 65, 99, 65, 74, 72, 98, 70, 70, 65, 65, 68, 96, 66, 66, 73, 70, 74, 68, 67, 65, 96, 71, 68, 69, 68, 72, 63, 62, 66, 63, 96, 64, 70, 65, 67, 62, 73, 62, 66, 64, 64, 65, 96, 62, 68, 73, 63, 63, 66, 63, 64, 65, 73, 65, 66, 62, 72, 65, 61, 64, 67, 96, 62, 69, 75, 62, 65, 66, 63, 67, 66, 70, 61, 63, 62, 63, 65, 63, 69, 68, 64, 65, 63, 64, 72, 64, 63, 71, 62, 96, 69, 72, 68, 99, 67, 73, 73, 64, 66, 63, 66, 69, 71, 63, 74, 66, 96, 66, 66, 66, 62, 63, 64, 62, 96, 64, 71, 63, 74, 60, 73, 64, 66, 71, 65, 71, 66, 64, 71, 98, 60, 71, 99, 71, 72, 71, 72, 68, 60, 65, 64, 73, 70, 71, 72, 64, 76, 73, 60, 63, 63, 96, 63, 96, 60, 67, 71, 72, 64, 66, 67, 72, 65, 73, 65, 64, 66, 66, 71, 72, 62, 64, 70, 97, 69, 73, 74, 59, 67, 96, 67, 67, 68, 68, 68, 66, 64, 62, 65, 73, 96, 67, 72, 68, 66, 69, 63, 73, 62, 76, 64, 69, 73, 69, 68, 73, 68, 71, 65, 60, 68, 96, 66, 72, 74, 63, 64, 71, 96, 75, 96, 70, 68, 70, 65, 69, 62, 62, 76, 64, 62, 67, 74, 96, 70, 66, 98, 74, 62, 67, 74, 63, 96, 96, 69, 64, 69, 76, 64, 74, 72, 69, 68, 65, 60, 64, 62, 65, 67, 65, 62, 73, 73, 69, 66, 69, 64, 65, 70, 69, 63, 64, 97, 64, 70, 96, 67, 69, 69, 68, 68, 65, 71, 66, 65, 97, 62, 69, 75, 61, 98, 71, 70, 65, 69, 66, 67, 70, 73, 69, 74, 70, 64, 72, 97, 62, 67, 99, 70, 65, 63, 67, 67, 96, 72, 59, 72, 63, 59, 65, 72, 69, 72, 60, 66, 70, 60, 70, 64, 68, 71, 68, 64, 61, 70, 65, 61, 63, 68, 66, 76, 96, 68, 66, 70, 66, 64, 68, 65, 73, 66, 69, 63, 65, 71, 70, 70, 72, 65, 64, 62, 64, 96, 60, 68, 66, 66, 72, 67, 67, 69, 66, 96, 61, 68, 63, 62, 66, 62, 64, 66, 68, 65, 74, 70, 69, 67, 60, 96, 68, 66, 64, 69, 64, 63, 72, 64, 65, 68, 70, 67, 69, 66, 61, 66, 71, 96, 96, 66, 70, 63, 76, 64, 66, 70, 71, 71, 66, 64, 66, 96, 75, 67, 72, 98, 96, 71, 66, 67, 72, 74, 71, 64, 63, 65, 60, 66, 72, 62, 69, 70, 65, 70, 72, 63, 65, 75, 67, 62, 69, 67, 75, 66, 68, 67, 72, 67, 61, 60, 65, 66, 61, 65, 62, 72, 67, 62, 98, 71, 96, 66, 67, 65, 63, 70, 70, 71, 96, 63, 68, 66, 65, 63, 61, 62, 68, 63, 66, 67, 62, 63, 67, 68, 64, 98, 69, 66, 61, 63, 69, 64, 64, 72, 66, 60, 65, 74, 68, 71, 62, 65, 63, 65, 96, 65, 66, 72, 67, 63, 66, 69, 65, 65, 71, 73, 72, 68, 62, 64, 66, 63, 69, 63, 67, 59, 71, 64, 98, 71, 64, 65, 67, 65, 65, 71, 71, 59, 67, 68, 71, 98, 64, 67, 72, 76, 96, 98, 72, 68, 64, 69, 64, 62, 98, 71, 72, 62, 98, 71, 69, 65, 64, 60, 72, 96, 64, 72, 65, 70, 62, 62, 66, 72, 69, 63, 72, 72, 70, 67, 64, 65, 66, 69, 65, 67, 66, 65, 62, 60, 66, 67, 69, 61, 63, 64, 66, 96, 70, 63, 72, 96, 67, 69, 69, 64, 70, 67, 71, 64, 63, 74, 64, 73, 66, 70, 68, 61, 62, 65, 71, 69, 67, 61, 68, 60, 74, 65, 72, 60, 70, 62, 70, 72, 69, 67, 66, 71, 69, 59, 67, 65, 63, 63, 98, 62, 97, 66, 65, 64, 68, 64, 65, 65, 66, 67, 96, 68, 62, 97, 71, 70, 69, 61, 63, 66, 96, 59, 62, 71, 76, 70, 73, 71, 59, 63, 69, 64, 64, 66, 69, 66, 98, 71, 69, 98, 72, 67, 62, 75, 63, 67, 64, 61, 70, 67, 65, 66, 75, 62, 69, 74, 67, 68, 64, 69, 75, 66, 68, 64, 96, 72, 59, 75, 64, 66, 68, 68, 68, 96, 96, 69, 72, 63, 63, 96, 66, 63, 68, 67, 64, 64, 72, 64, 70, 75, 68, 70, 64, 64, 62, 66, 65, 66, 96, 75, 62, 74, 72, 66, 98, 69, 66, 61, 71, 66, 67, 64, 70, 68, 73, 71, 96, 66, 64, 96, 62, 65, 72, 68, 69, 68, 62, 69, 74, 66, 72, 72, 60, 76, 70, 67, 72, 63, 62, 68, 63, 73, 63, 71, 60, 65, 64, 62, 66, 61, 68, 96, 68, 67, 61, 67, 72, 72, 62, 68, 62, 68, 72, 99, 68, 68, 72, 66, 68, 96, 69, 63, 63, 64, 65, 73, 67, 64, 65, 65, 68, 63, 66, 64, 63, 65, 63, 74, 63, 67, 69, 73, 63, 62, 70, 63, 64, 69, 65, 69, 69, 66, 69, 60, 64, 60, 66, 67, 96, 67, 96, 63, 62, 70, 71, 64, 72, 69, 63, 96, 67, 69, 71, 66, 71, 64, 64, 72, 72, 70, 64, 65, 70, 97, 64, 65, 96, 64, 62, 71, 98, 64, 70, 66, 65, 67, 72, 64, 62, 68, 70, 96, 74, 62, 62, 69, 64, 67, 96, 71, 63, 71, 66, 70, 61, 68, 63, 96, 70, 62, 60, 64, 76, 63, 75, 63, 71, 70, 67, 96, 65, 64, 62, 68, 66, 68, 64, 72, 63, 71, 70, 96, 62, 61, 64, 69, 66, 75, 64, 68, 68, 65, 69, 71, 65, 96, 67, 66, 66, 65, 70, 64, 72, 68, 67, 65, 65, 64, 64, 64, 66, 63, 96, 74, 60, 63, 62, 96, 68, 65, 66, 74, 67, 63, 71, 67, 69, 73, 69, 62, 63, 67, 65, 68, 67, 71, 97, 68, 60, 65, 65, 96, 68, 68, 65, 70, 68, 70, 71, 67, 64, 64, 59, 59, 61, 65, 60, 70, 62, 60, 69, 62, 69, 68, 66, 62, 68, 64, 70, 64, 62, 69, 96, 66, 59, 68, 68, 73, 68, 67, 96, 76, 96, 65, 73, 64, 64, 71, 65, 62, 71, 60, 62, 65, 72, 69, 70, 71, 68, 96, 61, 96, 96, 65, 63, 66, 66, 62, 61, 66, 65, 61, 68, 65, 96, 64, 65, 70, 63, 60, 69, 63, 62, 60, 62, 67, 66, 65, 66, 72, 98, 68, 64, 67, 65, 96, 71, 72, 62, 98, 96, 96, 71, 63, 68, 67, 70, 65, 67, 66, 72, 67, 69, 67, 96, 70, 60, 61, 61, 65, 96, 73, 63, 96, 62, 76, 62, 99, 68, 63, 70, 71, 73, 65, 67, 64, 71, 60, 68, 65, 67, 67, 67, 69, 67, 65, 62, 71, 64, 60, 62, 66, 64, 69, 60, 60, 60, 66, 71, 62, 72, 63, 64, 72, 65, 72, 68, 66, 76, 69, 62, 97, 67, 65, 68, 66, 66, 70, 66, 72, 67, 72, 74, 64, 64, 74, 96, 99, 69, 67, 68, 98, 66, 63, 71, 61, 63, 69, 71, 71, 96, 63, 66, 62, 64, 59, 64, 69, 66, 68, 65, 65, 65, 72, 68, 98, 65, 71, 96, 72, 62, 63, 65, 66, 66, 65, 67, 68, 75, 67, 68, 68, 67, 64, 69, 73, 68, 72, 71, 65, 96, 62, 67, 67, 71, 70, 61, 69, 65, 66, 71, 61, 63, 65, 69, 73, 63, 63, 69, 72, 70, 60, 69, 66, 73, 71, 68, 63, 67, 66, 64, 75, 62, 70, 99, 96, 76, 65, 69, 71, 67, 70, 70, 72, 70, 61, 68, 61, 63, 68, 61, 63, 69, 98, 68, 96, 70, 62, 71, 73, 70, 75, 66, 74, 70, 64, 64, 68, 65, 69, 64, 73, 73, 68, 70, 75, 62, 63, 68, 67, 69, 66, 64, 61, 63, 60, 64, 62, 68, 74, 63, 62, 63, 66, 60, 66, 65, 65, 67, 69, 67, 62, 70, 64, 62, 96, 72, 66, 66, 69, 68, 74, 74, 97, 69, 96, 70, 73, 67, 63, 72, 66, 75, 63, 69, 68, 71, 70, 64, 65, 72, 70, 64, 67, 96, 64, 63, 70, 65, 68, 62, 66, 73, 63, 65, 66, 66, 74, 71, 96, 68, 73, 66, 70, 70, 68, 67, 66, 65, 69, 63, 64, 75, 66, 62, 68, 68, 70, 68, 70, 67, 69, 64, 68, 69, 65, 62, 71, 68, 66, 65, 67, 67, 62, 98, 66, 96, 67, 65, 70, 98, 62, 64, 70, 69, 96, 72, 67, 65, 62, 62, 68, 61, 96, 62, 64, 63, 69, 69, 65, 71, 66, 64, 72, 96, 68, 65, 72, 68, 64, 72, 64, 72, 96, 98, 60, 65, 73, 67, 65, 65, 66, 71, 96, 62, 60, 98, 59, 62, 70, 68, 70, 65, 72, 96, 69, 96, 68, 68, 66, 96, 63, 68, 62, 64, 65, 70, 62, 67, 68, 64, 64, 67, 66, 69, 65, 64, 99, 96, 64, 63, 72, 97, 71, 61, 70, 64, 70, 71, 59, 64, 69, 66, 74, 64, 64, 67, 63, 67, 64, 68, 61, 64, 69, 66, 67, 70, 63, 62, 66, 64, 63, 68, 64, 74, 65, 99, 66, 71, 74, 75, 65, 68, 66, 98, 66, 64, 68, 68, 67, 68, 70, 63, 59, 71, 64, 76, 67, 71, 68, 64, 96, 66, 66, 64, 96, 71, 96, 76, 71, 63, 96, 68, 96, 71, 73, 75, 63, 67, 72, 61, 65, 64, 62, 67, 65, 68, 68, 72, 59, 63, 70, 67, 68, 96, 73, 65, 66, 67, 64, 72, 75, 62, 65, 71, 72, 69, 62, 68, 72, 65, 62, 64, 73, 98, 65, 96, 74, 61, 63, 71, 69, 67, 69, 67, 69, 61, 66, 70, 60, 74, 61, 68, 62, 66, 61, 72, 73, 96, 96, 67, 70, 66, 64, 73, 67, 69, 60, 69, 68, 68, 67, 96, 75, 98, 75, 96, 70, 63, 61, 61, 70, 61, 96, 67, 69, 67, 71, 75, 63, 67, 70, 69, 66, 61, 68, 62, 68, 62, 66, 76, 62, 64, 61, 69, 63, 97, 69, 72, 75, 71, 70, 65, 66, 96, 66, 69, 61, 69, 68, 65, 96, 60, 68, 63, 71, 62, 72, 73, 66, 68, 63, 70, 62, 71, 68, 97, 70, 62, 68, 69, 64, 76, 65, 63, 64, 68, 61, 61, 63, 71, 76, 65, 68, 64, 64, 96, 68, 68, 71, 63, 62, 70, 65, 67, 64, 62, 64, 61, 64, 67, 72, 70, 60, 70, 64, 67, 59, 69, 67, 69, 96, 61, 70, 68, 71, 69, 69, 68, 68, 63, 96, 61, 64, 62, 96, 74, 64, 65, 72, 69, 64, 69, 68, 65, 61, 73, 63, 60, 65, 75, 61, 67, 66, 70, 69, 67, 59, 75, 64, 65, 66, 67, 61, 70, 96, 61, 69, 68, 63, 65, 69, 63, 64, 70, 61, 99, 64, 63, 67, 96, 67, 64, 68, 74, 72, 67, 96, 64, 63, 65, 66, 71, 61, 61, 96, 68, 68, 68, 65, 64, 96, 69, 67, 64, 63, 60, 69, 64, 64, 65, 65, 67, 72, 68, 68, 69, 64, 72, 97, 67, 65, 59, 66, 64, 67, 74, 70, 70, 68, 71, 65, 66, 61, 70, 70, 69, 69, 65, 66, 66, 67, 63, 68, 98, 67, 68, 64, 74, 98, 65, 63, 64, 68, 63, 70, 66, 68, 96, 69, 61, 65, 96, 62, 73, 64, 66, 96, 61, 67, 66, 66, 64, 75, 67, 72, 67, 72, 74, 62, 63, 60, 72, 62, 64, 62, 62, 69, 64, 67, 67, 61, 64, 66, 74, 59, 69, 68, 65, 66, 73, 75, 67, 66, 71, 65, 99, 71, 69, 73, 64, 72, 64, 97, 67, 76, 64, 59, 69, 69, 66, 73, 71, 67, 65, 68, 65, 96, 62, 72, 71, 67, 69, 98, 96, 65, 72, 64, 62, 63, 96, 73, 98, 65, 69, 62, 73, 74, 73, 71, 61, 69, 68, 63, 64, 62, 72, 72, 66, 96, 62, 96, 69, 96, 96, 63, 64, 66, 63, 62, 69, 67, 64, 63, 68, 68, 72, 73, 63, 66, 65, 66, 73, 99, 66, 62, 96, 60, 64, 96, 96, 66, 75, 66, 69, 74, 71, 62, 64, 72, 96, 71, 68, 76, 64, 61, 67, 96, 66, 96, 61, 60, 68, 72, 64, 67, 68, 96, 71, 67, 67, 67, 70, 96, 68, 64, 72, 64, 67, 63, 68, 70, 98, 62, 65, 71, 64, 76, 66, 99, 69, 75, 60, 72, 71, 61, 69, 68, 76, 61, 63, 69, 65, 99, 68, 73, 66, 69, 99, 62, 67, 73, 61, 98, 96, 67, 66, 70, 70, 96, 73, 65, 96, 68, 70, 63, 61, 67, 64, 61, 64, 62, 72, 97, 64, 66, 73, 72, 60, 71, 64, 68, 99, 66, 62, 75, 72, 64, 72, 99, 69, 60, 65, 68, 66, 74, 60, 68, 63, 96, 66, 69, 67, 64, 73, 60, 72, 66, 63, 65, 67, 64, 60, 66, 70, 65, 67, 72, 65, 70, 61, 60, 71, 63, 70, 70, 73, 62, 62, 67, 68, 61, 71, 69, 62, 98, 63, 64, 67, 65, 60, 96, 67, 68, 72, 96, 64, 66, 66, 64, 71, 67, 62, 70, 97, 69, 64, 64, 71, 68, 68, 68, 64, 69, 62, 65, 64, 64, 64, 65, 67, 65, 63, 71, 74, 68, 75, 96, 68, 69, 73, 67, 61, 67, 68, 69, 65, 66, 67, 65, 69, 69, 63, 66, 67, 76, 65, 67, 68, 67, 65, 65, 69, 61, 96, 67, 71, 63, 73, 68, 63, 62, 65, 66, 67, 63, 70, 62, 74, 66, 73, 66, 64, 69, 60, 68, 64, 73, 59, 66, 65, 96, 69, 67, 63, 66, 67, 61, 76, 67, 76, 64, 67, 62, 67, 70, 67, 64, 67, 62, 64, 68, 64, 68, 68, 69, 96, 67, 63, 67, 69, 72, 68, 67, 66, 70, 64, 65, 67, 62, 63, 71, 68, 67, 68, 96, 61, 65, 74, 62, 64, 65, 66, 70, 67, 63, 75, 60, 68, 96, 66, 67, 60, 64, 72, 96, 64, 70, 66, 65, 67, 68, 73, 68, 73, 97, 65, 96, 63, 70, 63, 73, 68, 61, 64, 71, 64, 65, 98, 72, 63, 65, 62, 64, 72, 96, 65, 70, 70, 63, 70, 67, 68, 72, 67, 62, 67, 70, 65, 64, 72, 72, 68, 69, 75, 69, 60, 64, 71, 67, 68, 71, 64, 67, 96, 66, 63, 66, 70, 62, 65, 61, 68, 67, 67, 61, 67, 59, 69, 65, 63, 71, 69, 71, 96, 72, 68, 60, 63, 66, 70, 65, 72, 62, 76, 68, 68, 64, 68, 67, 68, 64, 73, 75, 71, 96, 66, 73, 67, 64, 73, 71, 96, 66, 72, 63, 70, 70, 98, 72, 67, 67, 99, 64, 67, 63, 70, 70, 67, 70, 67, 72, 63, 66, 65, 65, 60, 96, 72, 61, 67, 70, 64, 70, 66, 67, 72, 63, 64, 73, 67, 73, 67, 75, 68, 69, 69, 63, 66, 74, 68, 64, 64, 68, 65, 63, 66, 97, 67, 64, 68, 63, 75, 63, 64, 65, 63, 64, 67, 67, 62, 68, 65, 64, 72, 99, 65, 98, 65, 97, 60, 64, 61, 73, 72, 96, 62, 68, 63, 96, 74, 75, 70, 64, 64, 68, 66, 70, 68, 63, 67, 67, 66, 62, 71, 74, 68, 71, 66, 69, 96, 96, 72, 99, 69, 61, 72, 96, 72, 70, 65, 63, 61, 65, 66, 66, 68, 68, 64, 70, 67, 66, 98, 66, 72, 64, 96, 72, 98, 97, 68, 70, 64, 63, 67, 69, 61, 72, 76, 66, 63, 68, 64, 71, 64, 68, 65, 67, 64, 65, 69, 63, 62, 69, 66, 64, 70, 67, 68, 62, 67, 64, 71, 63, 63, 66, 62, 96, 66, 64, 66, 68, 72, 97, 60, 96, 60, 66, 69, 69, 64, 62, 59, 96, 64, 69, 62, 67, 63, 72, 65, 69, 70, 67, 62, 66, 69, 67, 62, 72, 96, 67, 69, 69, 62, 64, 74, 63, 67, 70, 62, 68, 63, 99, 70, 96, 63, 64, 70, 96, 98, 64, 71, 66, 65, 68, 66, 73, 61, 72, 61, 67, 63, 64, 62, 66, 66, 71, 66, 71, 68, 67, 68, 64, 69, 74, 64, 96, 67, 61, 70, 70, 63, 66, 67, 97, 68, 71, 73, 62, 73, 67, 70, 64, 61, 65, 69, 60, 60, 64, 63, 64, 66, 71, 74, 64, 66, 64, 65, 61, 66, 74, 96, 99, 59, 67, 71, 62, 70, 66, 67, 68, 63, 61, 74, 65, 67, 63, 76, 67, 69, 96, 65, 65, 96, 68, 74, 60, 63, 67, 96, 62, 99, 64, 64, 96, 71, 66, 98, 73, 68, 97, 97, 96, 65, 69, 66, 63, 66, 65, 62, 66, 66, 66, 69, 70, 96, 66, 72, 63, 96, 67, 73, 61, 74, 66, 65, 66, 74, 66, 72, 70, 65, 73, 68, 63, 71, 65, 59, 75, 67, 68, 63, 70, 98, 61, 61, 69, 97, 69, 66, 68, 64, 64, 96, 70, 65, 68, 74, 64, 61, 69, 76, 70, 63, 69, 64, 68, 67, 68, 72, 65, 66, 72, 63, 60, 70, 60, 65, 97, 73, 63, 64, 67, 71, 67, 96, 66, 71, 68, 72, 66, 67, 62, 62, 69, 65, 75, 64, 70, 66, 72, 64, 61, 62, 73, 96, 63, 71, 68, 97, 70, 63, 62, 71, 63, 66, 73, 70, 64, 63, 75, 62, 65, 68, 98, 68, 66, 96, 71, 73, 69, 68, 73, 67, 69, 62, 66, 62, 62, 64, 67, 72, 76, 69, 75, 66, 96, 66, 62, 64, 65, 63, 66, 65, 73, 67, 66, 72, 70, 66, 70, 70, 69, 64, 96, 96, 66, 70, 68, 66, 73, 69, 61, 96, 59, 73, 69, 67, 69, 69, 67, 70, 73, 72, 62, 62, 69, 65, 66, 71, 75, 62, 72, 66, 60, 70, 63, 65, 71, 68, 66, 96, 72, 64, 96, 68, 67, 66, 61, 60, 72, 74, 62, 63, 61, 63, 96, 74, 71, 63, 71, 68, 97, 72, 74, 65, 73, 75, 66, 67, 73, 66, 72, 71, 98, 96, 70, 65, 62, 66, 61, 96, 64, 75, 74, 96, 66, 64, 96, 60, 61, 72, 67, 66, 67, 71, 62, 67, 64, 96, 62, 70, 69, 64, 60, 67, 69, 68, 66, 68, 96, 64, 63, 66, 65, 71, 71, 63, 64, 62, 69, 70, 74, 62, 66, 63, 64, 68, 70, 65, 63, 62, 69, 96, 69, 96, 65, 64, 64, 97, 59, 68, 67, 70, 66, 66, 69, 67, 96, 70, 75, 64, 60, 74, 62, 66, 72, 71, 75, 96, 64, 70, 96, 67, 97, 60, 71, 68, 66, 74, 67, 64, 65, 72, 67, 65, 65, 73, 65, 63, 66, 65, 64, 68, 62, 74, 72, 73, 65, 67, 63, 64, 70, 72, 98, 66, 70, 65, 59, 96, 70, 67, 61, 70, 68, 65, 60, 74, 61, 62, 65, 63, 66, 74, 60, 62, 72, 64, 74, 67, 64, 76, 75, 73, 67, 63, 67, 96, 66, 99, 66, 63, 70, 69, 96, 65, 96, 60, 67, 67, 74, 69, 74, 66, 68, 67, 68, 64, 60, 67, 96, 72, 72, 62, 59, 65, 70, 64, 64, 70, 66, 75, 71, 97, 64, 72, 96, 70, 72, 70, 69, 66, 66, 68, 73, 65, 65, 64, 64, 64, 66, 96, 66, 71, 68, 70, 64, 61, 64, 72, 64, 60, 64, 66, 64, 65, 72, 75, 66, 66, 73, 66, 72, 64, 64, 64, 59, 70, 69, 59, 74, 62, 63, 66, 70, 66, 64, 65, 73, 96, 59, 96, 61, 63, 66, 68, 72, 70, 70, 65, 71, 76, 96, 64, 69, 64, 64, 96, 60, 73, 73, 98, 73, 64, 63, 69, 68, 74, 96, 72, 70, 63, 65, 70, 65, 64, 65, 65, 71, 64, 67, 96, 63, 72, 64, 71, 66, 67, 68, 62, 70, 65, 63, 62, 59, 65, 67, 72, 67, 65, 63, 70, 63, 66, 63, 65, 72, 98, 64, 65, 63, 97, 67, 69, 68, 69, 61, 98, 59, 69, 65, 74, 96, 68, 72, 65, 68, 61, 62, 69, 64, 71, 96, 73, 72, 67, 62, 69, 96, 72, 64, 69, 66, 67, 65, 72, 96, 63, 72, 65, 96, 96, 96, 70, 96, 69, 68, 70, 73, 66, 76, 74, 68, 96, 65, 72, 69, 61, 72, 72, 70, 66, 67, 96, 65, 64, 69, 68, 64, 66, 69, 68, 64, 68, 68, 71, 62, 73, 73, 69, 67, 72, 71, 62, 65, 64, 63, 66, 70, 68, 64, 68, 73, 68, 65, 67, 71, 66, 66, 74, 68, 62, 69, 69, 64, 72, 69, 73, 65, 66, 71, 71, 62, 61, 70, 65, 60, 71, 71, 71, 70, 63, 67, 64, 66, 64, 69, 61, 70, 98, 71, 71, 62, 64, 66, 68, 66, 64, 67, 63, 65, 71, 70, 75, 96, 68, 73, 67, 73, 71, 66, 70, 65, 96, 96, 65, 68, 70, 63, 72, 62, 63, 62, 62, 67, 75, 71, 73, 69, 62, 96, 71, 66, 70, 64, 61, 74, 72, 71, 65, 69, 70, 62, 60, 72, 71, 68, 72, 69, 96, 66, 74, 65, 75, 64, 66, 67, 73, 74, 66, 73, 68, 69, 73, 65, 66, 66, 69, 71, 64, 62, 63, 62, 69, 65, 64, 72, 72, 68, 71, 64, 96, 72, 63, 67, 62, 72, 71, 65, 96, 66, 72, 72, 67, 74, 96, 69, 67, 74, 64, 60, 66, 64, 96, 72, 67, 60, 64, 61, 68, 59, 67, 72, 67, 59, 69, 75, 66, 63, 59, 63, 64, 75, 64, 96, 62, 68, 64, 66, 64, 63, 65, 72, 65, 61, 76, 66, 63, 66, 96, 70, 63, 62, 67, 64, 71, 61, 61, 67, 71, 65, 60, 65, 67, 66, 60, 66, 65, 72, 68, 76, 65, 67, 67, 67, 64, 59, 69, 61, 65, 72, 70, 66, 69, 64, 72, 66, 71, 96, 61, 67, 96, 72, 70, 68, 66, 66, 96, 72, 65, 66, 65, 60, 64, 63, 65, 73, 74, 96, 66, 63, 61, 66, 69, 63, 75, 70, 66, 65, 65, 68, 68, 68, 65, 68, 72, 70, 98, 63, 67, 71, 68, 70, 71, 97, 73, 96, 72, 63, 60, 66, 62, 64, 69, 67, 68, 69, 73, 73, 66, 71, 64, 69, 65, 63, 61, 73, 64, 69, 66, 98, 62, 65, 65, 63, 63, 60, 75, 75, 65, 60, 60, 64, 62, 66, 64, 59, 96, 71, 68, 72, 62, 69, 63, 63, 64, 59, 63, 71, 71, 96, 62, 69, 67, 96, 64, 75, 65, 60, 69, 67, 63, 74, 96, 67, 66, 73, 65, 71, 65, 65, 68, 98, 64, 72, 65, 64, 61, 68, 72, 75, 64, 64, 63, 64, 63, 62, 68, 67, 70, 71, 66, 67, 63, 68, 72, 67, 76, 64, 74, 72, 62, 67, 70, 64, 62, 71, 96, 98, 67, 63, 70, 70, 63, 70, 72, 96, 96, 70, 71, 69, 59, 61, 64, 64, 67, 62, 65, 71, 62, 66, 68, 71, 74, 69, 68, 75, 97, 61, 67, 64, 69, 64, 73, 68, 64, 72, 65, 66, 64, 71, 70, 61, 71, 74, 69, 73, 98, 63, 67, 60, 64, 67, 69, 70, 64, 63, 96, 67, 64, 64, 62, 74, 64, 67, 68, 74, 68, 96, 73, 65, 66, 71, 63, 68, 64, 65, 69, 69, 68, 68, 71, 61, 62, 62, 64, 60, 66, 66, 72, 96, 64, 96, 71, 73, 74, 68, 66, 71, 76, 67, 62, 60, 72, 66, 67, 66, 71, 69, 63, 72, 76, 68, 70, 72, 76, 69, 67, 70, 65, 66, 63, 60, 67, 68, 67, 65, 71, 60, 71, 66, 65, 70, 63, 63, 60, 69, 96, 96, 96, 64, 67, 71, 72, 70, 69, 66, 61, 66, 98, 64, 72, 96, 71, 60, 63, 64, 67, 73, 69, 62, 69, 96, 69, 69, 96, 97, 63, 62, 60, 64, 96, 74, 65, 71, 64, 69, 65, 66, 71, 70, 75, 59, 73, 68, 99, 63, 62, 60, 67, 75, 76, 63, 73, 75, 65, 60, 74, 65, 96, 66, 73, 69, 65, 64, 61, 66, 61, 76, 66, 63, 74, 98, 59, 73, 65, 66, 68, 62, 74, 76, 62, 68, 67, 63, 67, 68, 74, 67, 71, 72, 71, 64, 63, 65, 68, 67, 61, 96, 68, 64, 70, 61, 66, 71, 96, 96, 99, 64, 96, 66, 65, 71, 74, 69, 68, 65, 68, 63, 64, 73, 67, 64, 64, 70, 72, 73, 64, 63, 72, 97, 65, 70, 63, 96, 97, 67, 66, 67, 72, 67, 65, 67, 67, 96, 70, 65, 67, 74, 67, 65, 64, 69, 60, 63, 72, 71, 68, 62, 68, 98, 74, 68, 66, 71, 65, 68, 69, 59, 72, 62, 75, 61, 62, 71, 64, 61, 69, 69, 70, 73, 65, 67, 68, 96, 96, 76, 66, 70, 67, 70, 67, 67, 96, 61, 66, 62, 72, 98, 72, 71, 96, 66, 72, 66, 73, 74, 64, 63, 65, 62, 73, 63, 63, 69, 66, 74, 70, 60, 66, 66, 61, 98, 60, 63, 71, 63, 60, 60, 96, 67, 61, 69, 69, 70, 65, 65, 72, 66, 67, 62, 68, 68, 96, 66, 70, 69, 66, 70, 66, 64, 64, 68, 67, 62, 67, 97, 66, 69, 64, 67, 97, 73, 65, 71, 69, 67, 67, 60, 75, 67, 69, 68, 61, 64, 63, 67, 73, 97, 68, 64, 65, 64, 74, 72, 59, 62, 60, 69, 62, 71, 69, 59, 66, 63, 63, 62, 70, 70, 96, 96, 61, 68, 66, 96, 70, 60, 63, 62, 71, 96, 70, 59, 69, 60, 64, 65, 72, 71, 64, 66, 68, 66, 67, 62, 68, 70, 66, 70, 70, 63, 73, 67, 65, 65, 66, 72, 61, 64, 69, 71, 65, 64, 70, 62, 71, 68, 65, 96, 66, 63, 96, 64, 63, 68, 72, 64, 62, 64, 68, 69, 64, 68, 73, 72, 76, 68, 62, 67, 69, 69, 71, 61, 62, 68, 68, 66, 67, 64, 98, 75, 67, 96, 64, 64, 62, 63, 98, 60, 64, 74, 66, 64, 66, 69, 63, 70, 69, 69, 64, 64, 62]


So around 4700 records of height, thats great. How about plotting a histogram for these values. 

## Plotting Histograms

In the cell below, Import matplotlib as we saw earlier and plot a histogram of these values. Use a bin size of 8. Considering the height in inches, record your initial observations in the following cell. 


```python
# Import matplotlib and plot histogram forheight data
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('ggplot')
plt.hist(height, bins=8)
```




    (array([ 917., 1972., 1230.,  228.,    0.,    0.,    0.,  438.]),
     array([59., 64., 69., 74., 79., 84., 89., 94., 99.]),
     <a list of 8 Patch objects>)




![png](index_files/index_8_1.png)



```python
# The most frequent , or central , or typical value is between 65 and 70.
# Around 400 individuals are extremely tall , nearing 8 feet
# The distribution seems to have OUTLIERS 
```

Do you spot anything unsual above , some outliers maybe ?

## Calculating mean 

So first let's calculate the mean for the height list. Recall the formula for calculating mean as shown earlier. 

![](mean.gif)

Using the python skills you have learned so far, create a function `get_mean()` to perform following tasks: 
* Input a list of numbers (like the height list we have above)
* calculate the sum of numbers and length of the list 
* Calculate mean from above, round off to 2 decimals and return it.


```python
def get_mean(data):

    count = 0
    for i in data:
        count += i
    mean = round(count / len(data), 2)
    
    return mean

test1 = [5, 4, 1, 3, 2]
test2 = [4, 2, 3, 1]

print(get_mean(test1)) # 3
print(get_mean(test2)) # 2.5
```

    3.0
    2.5


Now we'll test the function by passing in the heigt list.


```python
# After creating the function, pass the height list to the function 
mean = get_mean(height)

# Uncomment following command
print("Sample Mean:", mean)

# Sample Mean: 69.5783
```

    Sample Mean: 69.58


So we have our mean length, 69.5, and this confirms our observations from the histogram. But we also some outliers in out data above and we know outliers effect the mean calculation by pulling mean value in their direction.  So let's remove these outliers and create a new list to see if our mean shifts of stays. We'll use a threshold of 80 inches, i.e. filter out any values greater than 80. 
 
Perform following tasks:

* Create a function `filter_list()` that inputs a list 
* Perform a for loop to iteratively check and aappend values to a new list if < 80. 
* Return the new list 


```python
def filter_list(listA):
    listB = []
    for l in listA:
        if l < 80:
            listB.append(l)
    return listB

test = [60, 70,80, 90]
filter_list(test) # [60, 70]
```




    [60, 70]



Great, now we can filter our height list and plot a new histogram for the new list to see if things change considerably.  


```python
# Filter the height list using above function
height_filtered = filter_list(height)
```


```python
# Plot a histogram for the new list - use 8 bins as before
plt.hist(height_filtered, bins = 8);
```


![png](index_files/index_20_0.png)



```python
# Get the mean of the new list using our get_mean() function
get_mean(height_filtered)
# 66.85
```




    66.85



Now based on your findings before and after the outliers in mean and histogram, record your observations below:


```python
# The outliers effected the mean by about 3 inches.
# In the absence of outliers, the distribution becomes very UNIFORM
# More in line with everyday observations in terms of individuals' heights
```

Right, in some analytical situations we may not be able to exclude the outliers in such a naive manner. So let's calculate other measures of central tendency as well. We'll move on to calculating the median value for our original height data. 

## Calculating Median 

The median is the value directly in the middle of the a dataset. In statistical terms, this is the median quartile. If the dataset was sorted from lowest value to highest value, the median is the value that would be larger than the first 50% of the data, and smaller than the second 50%.

If the dataset has an odd number of values, then the median is the middle number.
If the datasaet has an even number of values, then we take the mean of the middle two numbers.

In the cell below, write a function that takes in an array of numbers and returns the median value for that dataset. Make sure you first check for even / odd and perform computation accordingly. So its `Sorting > checking even/odd > calculating median`. Let's give it a try. 

(Hint: you can use modulo operator `%` in python to check if a value is even or odd)


```python
def get_median(data):

    data_sorted = sorted(data)

    if len(data_sorted) % 2 == 0:
        val1_index = int((len(data_sorted) / 2) - 1)
        val2_index = val1_index + 1
        return (data_sorted[val1_index] + data_sorted[val2_index]) / 2

    else:
        med_index = (len(data_sorted) // 2) 
        return data_sorted[med_index]

test1 = [5, 4, 1, 3, 2]
test2 = [4, 2, 3, 1]

print(get_median(test1)) # 3
print(get_median(test2)) # 2.5
```

    3
    2.5


Great, now we can pass in our height list to this function to check the median. 


```python
get_median(height)
# 67
```




    67



So we have 67 , which is much closer to the filtered list mean (66.85) than the mean we calculated with actual list (69.58). So median in this case seems to be a much better indicative of the central tendency found in the dataset. 

But remember we also have mode ! Maybe this can give us an even better insight into the typical values in the dataset based on how frequent a value is. So let's calculate that. 

## Calculating Mode

The mode is the value that shows up the most in a dataset. A dataset can have 0 or more modes. If no value shows up more than once, the dataset is considered to have no mode value. If two numbers show up the same number of times, that dataset is considered bimodal. Datasets where multiple values all show up the same number of times are considered multimodal.

In the cell below, write a function that takes in an list of numbers and returns another list containing the mode value(s). In case of only one mode, the list would have a single element. 

Hint: Building frequency distribution table using dictionaries is probably the easiest way to approach this problem. Use each unique element from the height list as a key, and frequency of this element as the value and build a dictionary. You can then simply identify the keys (heights) with maximum values. 


```python
def get_mode(data):

    # Create and populate frequency distribution
    frequency_dict = {}
    
    # If an element is not in the dictionary , add it with value 1
    # If an element is already in the dictionary , +1 the value
    for i in data:
        if i not in frequency_dict:
            frequency_dict[i] = 1
        else:
            frequency_dict[i] += 1
    
    # Create alist for mode values
    modes = []
    
    #from the dictionary, add element(s) to the modes list with max frequency
    highest_freq = max(frequency_dict.values())
    for key, val in frequency_dict.items():
        if val == highest_freq:
            modes.append(key)
    # Return the mode list 
    return modes

test1 = [1, 2, 3, 5, 5, 4]
test2 = [1, 1, 1, 2, 3, 4, 5, 5, 5]
print(get_mode(test1)) # [5]
print(get_mode(test2)) # [1, 5]
```

    [5]
    [1, 5]


Thats done. Now can see calculate mode and compare it with our mean and median values. 


```python
get_mode(height)
```




    [64]



So the mode value is much lower than our mean and median calculated earlier. What do you make of this? The answer to that could be subjective and depends on the problem. i.e. If your problem is to identify sizes for garments that would sell the most, you can not disregard mode. However, if you want to get an idea about the general or typical height of individuals, you can probably still do with median and average. 

To get an even clearer picture, We know we need to see how much the values deviate from the central values we have identified. We have seen variance and standard deviation before as measures of such dispersion. Let's have a go at these to strengthen our understanding around this data. 


## Calculate Variance

The formula for variance, has been shown earlier as: 
![](variance.jpg)

You are required to write a function In the cell below, that takes an array of numbers as input and returns the Variance of the sample as output.


```python
def get_variance(sample):

    # First, calculate the sample mean
    N = len(sample) - 1
    total = sum(sample)
    sample_mean = total/N
    
    # Now, subtract the sample mean from each point and square the result. 
    val_minus_mu_accumulator = 0
    for i in sample:
        val_minus_mu_accumulator += (i - sample_mean)**2
    
    # Divde the total by the number of items in the sample  
    variance = val_minus_mu_accumulator / N
    
    return round(variance, 2)

test1 = [1, 2, 3, 5, 5, 4]
test2 = [1, 1, 1, 2, 3, 4, 5, 5, 5]
print(get_variance(test1)) # 3.2
print(get_mean(test1))
print(get_variance(test2)) # 3.4
```

    3.2
    3.33
    3.41


Now we can test the variance of our height list with get_variance() function. 


```python
get_variance(height)
```




    87.74



SO this value, As we learned earlier, tells us a abit about the deviation but not in the units of underlying data. This is because it squares the values of deviations. Standard deviation, however, can deal with this issue as it takes the square roots of differences. So that would probably be a bit more revealing. 

## Calculate Standard Deviation

In the cell below, write a function that takes an array of numbers as input and returns the standard deviation of that sample as output.

Recall that the formula for Standard Deviation is:

![](std.gif)

you would need `sqrt` method from math library to calculate the square root. 


```python
from math import sqrt

def get_stddev(list):
    
    sum = 0
    
    meann = get_mean(list)
    
    for i in range(len(list)):
        sum += pow((list[i]-meann),2)

    stddev = sqrt(sum/len(list)-1)
    
    return round(stddev, 2) 

test = [120,112,131,211,312,90]

print (get_stddev(test))
```

    76.7


So now we can finally calculate stndard deviation for our height list and inspect the results. 


```python
get_stddev(height)
```




    9.31



So 9.3 inches is how the deviation is present in our dataset. As we are still including outlier values, this might still slightly be effected but these results are now much more reliable. 

We shall finally build a boxplot for height data and see if it agrees with our understanding for this data that we have developed up to this point. USe the matplotlib's boxplot method with height data and comment on the output 

## Build a BoxPlot

Follow the boxplot method shown earier and build a boxplot for height data. See if you can spot the outliers? Are the observations gathered from boxplot inline with our calculations? 


```python
# Build a box plot for the height data 
plt.boxplot(height);
plt.title('Height Data')
```




    Text(0.5,1,'Height Data')




![png](index_files/index_51_1.png)



```python
# Record your observations here 

# We can clearly see the extreme outliers. outliers
# The median value is around 67
# The range of values (excluding outliers) is from 58 - 76 inches
# more than fifty percent of values lie inside the range of around 64 - 71 inches
```

### Findings
So there we have it. We have done an indepth analysis of individuals' heights using measure of central tendency of the data (67 - 68) inches, and the standard spread of the data to be around 9 inches around the mean. So we can expect MOST of the individuals to lie between 64 to 71 inches. These figures have been confirmed by our calculations as well as visual analysis of the data with histograms and boxplots. 

We shall learn how o further this analysis using more sophisticated statistical methods as models as we progress through the course. We shall also learn how these basic techniques provide you with a strong foundation to develop your intuitions for machine learning and predictive analysis. 

## Summary 

In this lab, we performed a basic, yet detailed statistical analysis around measuring the tendencies of center and spread in a given dataset. We looked at building a number of functions for calculate different measures and also used some statistical visualizations to strengthen our intuitions around the dataset. We shall see how we can simplify this process as we study numpy and pandas libraries to ease out the programming load while calculating basic statistics. 
