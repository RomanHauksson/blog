---
title: "Does sleep help me type faster?"
date: 2023-07-01
# weight: 1
# aliases: ["/first"]
tags: [""]
# author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: true
hidemeta: false
comments: false
description: ""
canonicalURL: "https://roman.computer/sleep_helping_typing"
disableHLJS: true # to disable highlightjs
disableShare: true
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: false
ShowRssButtonInSectionTermList: true
UseHugoToc: true

# cover:
#     image: "<image path/url>" # image path/url
#     alt: "<alt text>" # alt text
#     caption: "<text>" # display caption under cover
#     relative: false # when using page bundles set this to true
#     hidden: true # only hide on current single page
# editPost:
#     URL: "https://github.com/<path_to_repo>/content"
#     Text: "Suggest Changes" # edit text
#     appendFilePath: true # to append file path to Edit link
---

# Does more sleep help me type faster?

Over the past year, I\'ve been meticulously recording the times I go to
bed and get up, as well as compulsively taking typing speed tests.
Because my typing speed at any given moment feels like an rough measure
of my cognitive state, I was curious to see whether there was any
correlation between my typing speed from each test and the amount of
sleep I had gotten the night before.

First, i exported my sleep data from [Plees
Tracker](https://vmiklos.hu/plees-tracker/) and my typing speed data
from [Monkeytype](https://monkeytype.com/) and put them into a Pandas
dataframe.

<div id="213160eb" class="cell code" markdown="1" execution_count="4">

``` python
# %pip install pandas
import pandas as pd
sleep_data = pd.read_csv('sleep.csv')
sleep_data.head()
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
      <th>sid</th>
      <th>start</th>
      <th>stop</th>
      <th>rating</th>
      <th>comment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>1647495229195</td>
      <td>1647525654514</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4</td>
      <td>1647589951572</td>
      <td>1647624766308</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>5</td>
      <td>1647683304465</td>
      <td>1647711928143</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6</td>
      <td>1647766415876</td>
      <td>1647785459740</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7</td>
      <td>1647849099344</td>
      <td>1647876705164</td>
      <td>0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>

</div>

<div id="a04263b1" class="cell code" markdown="1" execution_count="5">

``` python
typing_data = pd.read_csv('typing.csv', sep='|')
typing_data.head()
```

<div class="output execute_result" markdown="1" execution_count="5">

```{=html}
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
      <th>_id</th>
      <th>isPb</th>
      <th>wpm</th>
      <th>acc</th>
      <th>rawWpm</th>
      <th>consistency</th>
      <th>charStats</th>
      <th>mode</th>
      <th>mode2</th>
      <th>quoteLength</th>
      <th>...</th>
      <th>punctuation</th>
      <th>numbers</th>
      <th>language</th>
      <th>funbox</th>
      <th>difficulty</th>
      <th>lazyMode</th>
      <th>blindMode</th>
      <th>bailedOut</th>
      <th>tags</th>
      <th>timestamp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>64482e748b42543b37ae539a</td>
      <td>NaN</td>
      <td>132.0</td>
      <td>96.58</td>
      <td>139.2</td>
      <td>81.92</td>
      <td>330,5,1,0</td>
      <td>time</td>
      <td>30</td>
      <td>-1</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>english</td>
      <td>none</td>
      <td>normal</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>NaN</td>
      <td>1682452084000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>642b61d10a366c07ed74b57d</td>
      <td>NaN</td>
      <td>146.4</td>
      <td>94.47</td>
      <td>158.4</td>
      <td>89.90</td>
      <td>366,14,1,2</td>
      <td>time</td>
      <td>30</td>
      <td>-1</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>english</td>
      <td>none</td>
      <td>normal</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>NaN</td>
      <td>1680564689000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>642b5a2e0a366c07ed62b2c0</td>
      <td>NaN</td>
      <td>133.6</td>
      <td>93.78</td>
      <td>148.0</td>
      <td>88.10</td>
      <td>334,15,3,1</td>
      <td>time</td>
      <td>30</td>
      <td>-1</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>english</td>
      <td>none</td>
      <td>normal</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>NaN</td>
      <td>1680562734000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>642b59c10a366c07ed62b005</td>
      <td>True</td>
      <td>151.6</td>
      <td>100.00</td>
      <td>151.6</td>
      <td>89.29</td>
      <td>379,0,0,0</td>
      <td>time</td>
      <td>30</td>
      <td>-1</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>english</td>
      <td>none</td>
      <td>normal</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>NaN</td>
      <td>1680562625000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>642b47850a366c07ed448763</td>
      <td>NaN</td>
      <td>133.2</td>
      <td>95.34</td>
      <td>146.0</td>
      <td>83.14</td>
      <td>333,10,1,2</td>
      <td>time</td>
      <td>30</td>
      <td>-1</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>english</td>
      <td>none</td>
      <td>normal</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>NaN</td>
      <td>1680557957000</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 24 columns</p>
</div>
```

</div>

</div>

<div id="66dfebd6" class="cell markdown" markdown="1">

This function calculates the overlap between two time periods and is
used to calculate how much sleep I\'ve gotten in the 24 hours before
each typing test.

</div>

<div id="4a39683f" class="cell code" markdown="1" execution_count="3">

``` python
def time_overlap(start1, end1, start2, end2):
    return max(0, min(end1, end2) - max(start1, start2))
```

</div>

<div id="b55bc9b1" class="cell code" markdown="1" execution_count="4">

``` python
typing_data['sleep miliseconds'] = 0
```

</div>

<div id="ef0c219b" class="cell code" markdown="1" execution_count="5">

``` python
typing_data.reset_index()
sleep_data.reset_index()
for test_index, typing_test in typing_data.iterrows():
    for sleep_index, sleep in sleep_data.iterrows():
        past_day_sleep_miliseconds = time_overlap(
            typing_test['timestamp'] - 1000 * 60 * 60 * 24,
            typing_test['timestamp'],
            sleep['start'],
            sleep['stop']
        )
        typing_data.loc[test_index, 'sleep miliseconds'] += past_day_sleep_miliseconds
```

</div>

<div id="f3deb7f9" class="cell code" markdown="1" execution_count="6">

``` python
# Remove every row where I got zero sleep.
# These are days in which I took a typing test but wasn't tracking my sleep.
typing_data = typing_data[typing_data['sleep miliseconds'] > 0]

# Make a new column with the number of hours of sleep instead of miliseconds.
typing_data['sleep hours'] = typing_data['sleep miliseconds'] / (1000 * 60 * 60)
```

</div>

<div id="9d28e107" class="cell code" markdown="1" execution_count="7">

``` python
%pip install scipy
from scipy.stats import pearsonr
```

<div class="output stream stdout" markdown="1">

    Requirement already satisfied: scipy in /opt/homebrew/lib/python3.11/site-packages (1.10.1)
    Requirement already satisfied: numpy<1.27.0,>=1.19.5 in /opt/homebrew/lib/python3.11/site-packages (from scipy) (1.25.0)

    [notice] A new release of pip is available: 23.0.1 -> 23.1.2
    [notice] To update, run: python3.11 -m pip install --upgrade pip
    Note: you may need to restart the kernel to use updated packages.

</div>

</div>

<div id="271d2b2a" class="cell code" markdown="1" execution_count="20">

``` python
typing_speeds = typing_data['wpm']
typing_accuracies = typing_data['acc']
sleep_hours = typing_data['sleep hours']
```

</div>

<div id="b93f745f" class="cell code" markdown="1" execution_count="9">

``` python
speed_sleep_correlation, speed_sleep_p_value = pearsonr(sleep_hours, typing_speeds)
accuracy_sleep_correlation, accuracy_sleep_p_value = pearsonr(sleep_hours, typing_accuracies)
```

</div>

<div id="bdcdfc9c" class="cell code" markdown="1" execution_count="15">

``` python
print(f'My typing speed and the amount of sleep I got has a Pearson correlation coefficient of {round(speed_sleep_correlation, 2)} with a p-value of {round(speed_sleep_p_value, 3)}.')
print(f'My typing accuracy and sleep has a correlation coefficient of {round(accuracy_sleep_correlation, 2)} with a p-value of {round(accuracy_sleep_p_value, 3)}.')
```

<div class="output stream stdout" markdown="1">

    My typing speed and the amount of sleep I got has a Pearson correlation coefficient of 0.22 with a p-value of 0.033.
    My typing accuracy and sleep has a correlation coefficient of -0.09 with a p-value of 0.394.

</div>

</div>

<div class="cell code" markdown="1">

``` python
```

</div>

<div id="3a34af14" class="cell code" markdown="1" execution_count="18">

``` python
test_timestamps = typing_data['timestamp']
```

</div>

<div id="df29384b" class="cell code" markdown="1" execution_count="27">

``` python
time_vs_speed_correlation, time_vs_speed_p_value = pearsonr(test_timestamps, typing_speeds)
time_vs_accuracy_correlation, time_vs_accuracy_p_value = pearsonr(test_timestamps, typing_accuracies)
time_vs_sleep_correlation, time_vs_sleep_p_value = pearsonr(test_timestamps, sleep_hours)
```

</div>

<div id="100789d7" class="cell code" markdown="1" execution_count="29">

``` python
print(f'Timestamp vs. typing speed correlation: {round(time_vs_speed_correlation, 2)}; p-value: {round(time_vs_speed_p_value, 8)}.')
print(f'Timestamp vs. typing accuracy correlation: {round(time_vs_accuracy_correlation, 2)}; p-value: {round(time_vs_accuracy_p_value, 3)}.')
print(f'Timestamp vs. sleep amount correlation: {round(time_vs_sleep_correlation, 2)}; p-value: {round(time_vs_sleep_p_value, 8)}.')
```

<div class="output stream stdout" markdown="1">

    Timestamp vs. typing speed correlation: 0.43; p-value: 1.584e-05.
    Timestamp vs. typing accuracy correlation: -0.21; p-value: 0.049.
    Timestamp vs. sleep amount correlation: 0.52; p-value: 8e-08.

</div>

</div>

<div id="89b5ae3d" class="cell code" markdown="1" execution_count="41">

``` python
# make a scatterplot graph of typing speed over time
%pip install matplotlib
import matplotlib.pyplot as plt
```

<div class="output stream stdout" markdown="1">

    Collecting matplotlib
      Downloading matplotlib-3.7.1-cp311-cp311-macosx_11_0_arm64.whl (7.3 MB)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 7.3/7.3 MB 5.8 MB/s eta 0:00:0000:0100:01
    acosx_11_0_arm64.whl (229 kB)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 229.3/229.3 kB 7.8 MB/s eta 0:00:00
    acosx_10_9_universal2.whl (2.5 MB)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.5/2.5 MB 12.2 MB/s eta 0:00:00a 0:00:01
    acosx_11_0_arm64.whl (63 kB)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 63.1/63.1 kB 4.2 MB/s eta 0:00:00
    ent already satisfied: numpy>=1.20 in /opt/homebrew/lib/python3.11/site-packages (from matplotlib) (1.25.0)
    Requirement already satisfied: packaging>=20.0 in /Users/roman/Library/Python/3.11/lib/python/site-packages (from matplotlib) (23.1)
    Collecting pillow>=6.2.0
      Downloading Pillow-9.5.0-cp311-cp311-macosx_11_0_arm64.whl (3.1 MB)
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.1/3.1 MB 14.6 MB/s eta 0:00:0000:0100:01
    ent already satisfied: python-dateutil>=2.7 in /Users/roman/Library/Python/3.11/lib/python/site-packages (from matplotlib) (2.8.2)
    Requirement already satisfied: six>=1.5 in /Users/roman/Library/Python/3.11/lib/python/site-packages (from python-dateutil>=2.7->matplotlib) (1.16.0)
    Installing collected packages: pyparsing, pillow, kiwisolver, fonttools, cycler, contourpy, matplotlib
    Successfully installed contourpy-1.1.0 cycler-0.11.0 fonttools-4.40.0 kiwisolver-1.4.4 matplotlib-3.7.1 pillow-9.5.0 pyparsing-3.1.0

    [notice] A new release of pip is available: 23.0.1 -> 23.1.2
    [notice] To update, run: python3.11 -m pip install --upgrade pip
    Note: you may need to restart the kernel to use updated packages.

</div>

<div class="output stream stderr" markdown="1">

    Matplotlib created a temporary config/cache directory at /var/folders/4g/wp7_2pd96lg8wz5kqrshcg_40000gn/T/matplotlib-7xsnkee2 because the default path (/Users/roman/.matplotlib) is not a writable directory; it is highly recommended to set the MPLCONFIGDIR environment variable to a writable directory, in particular to speed up the import of Matplotlib and to better support multiprocessing.

</div>

<div class="output execute_result" markdown="1" execution_count="41">

    <matplotlib.collections.PathCollection at 0x147de6250>

</div>

<div class="output display_data" markdown="1">

![](images/2de8b9fba3d6f2821b31a6cee10f648cd044c404.png)

</div>

</div>

<div id="9d3caccf" class="cell code" markdown="1" execution_count="42">

``` python
plt.scatter(test_timestamps, typing_speeds)
```

<div class="output execute_result" markdown="1" execution_count="42">

    <matplotlib.collections.PathCollection at 0x147f02610>

</div>

<div class="output display_data" markdown="1">

![](images/2de8b9fba3d6f2821b31a6cee10f648cd044c404.png)

</div>

</div>

<div id="305b5b71" class="cell code" markdown="1" execution_count="43">

``` python
plt.scatter(test_timestamps, typing_accuracies)
```

<div class="output execute_result" markdown="1" execution_count="43">

    <matplotlib.collections.PathCollection at 0x147fe9bd0>

</div>

<div class="output display_data" markdown="1">

![](images/a35870c143fa50b174f6d3391474cd5b8ed4227c.png)

</div>

</div>

<div id="d3bff9f2" class="cell code" markdown="1" execution_count="44">

``` python
plt.scatter(test_timestamps, sleep_hours)
```

<div class="output execute_result" markdown="1" execution_count="44">

    <matplotlib.collections.PathCollection at 0x15018c0d0>

</div>

<div class="output display_data" markdown="1">

![](images/09bb0775f9389c692b365fcfa9f57c7b024fd998.png)

</div>

</div>

<div id="de6ea8c9" class="cell code" markdown="1" execution_count="48">

``` python
# add a trendline to the graph
import numpy as np

z = np.polyfit(test_timestamps, sleep_hours, 1)
p = np.poly1d(z)
plt.scatter(test_timestamps, sleep_hours)
plt.plot(test_timestamps, p(test_timestamps))
```

<div class="output execute_result" markdown="1" execution_count="48">

    [<matplotlib.lines.Line2D at 0x1503304d0>]

</div>

<div class="output display_data" markdown="1">

![](images/566018bd6af5c90f3b065eea9a1429f9ae1f061f.png)

</div>

</div>

<div id="0c38b8a9" class="cell code" markdown="1" execution_count="49">

``` python
z = np.polyfit(test_timestamps, typing_speeds, 1)
p = np.poly1d(z)
plt.scatter(test_timestamps, typing_speeds)
plt.plot(test_timestamps, p(test_timestamps))
```

<div class="output execute_result" markdown="1" execution_count="49">

    [<matplotlib.lines.Line2D at 0x15039ee50>]

</div>

<div class="output display_data" markdown="1">

![](images/c14c7aebcb29303070227e719fbe8120f7874e15.png)

</div>

</div>

<div id="e320ea7c" class="cell markdown" markdown="1">

Looks like I need to detrend the typing speed data and the sleep data
before I see how they\'re related. I can\'t use the built-in detrending
function because it only works on time series data, so I\'ll write my
own.

</div>

<div id="b83fec06" class="cell code" markdown="1" execution_count="55">

``` python
def detrend(x, y):
    z = np.polyfit(x, y, 1)
    p = np.poly1d(z)
    y_detrended = y - p(x)
    return y_detrended
```

</div>

<div id="57439024" class="cell code" markdown="1" execution_count="56">

``` python
sleep_hours_detrended = detrend(test_timestamps, sleep_hours)
typing_speed_detrended = detrend(test_timestamps, typing_speeds)
```

</div>

<div id="5f63b495" class="cell code" markdown="1" execution_count="60">

``` python
# scatterplots of detrended data over time
plt.scatter(test_timestamps, typing_speed_detrended)

z = np.polyfit(test_timestamps, typing_speed_detrended, 1)
p = np.poly1d(z)
plt.scatter(test_timestamps, typing_speed_detrended)
plt.plot(test_timestamps, p(test_timestamps))
```

<div class="output execute_result" markdown="1" execution_count="60">

    [<matplotlib.lines.Line2D at 0x157f46750>]

</div>

<div class="output display_data" markdown="1">

![](images/513802a6cbcf959fa66c128b76b514eb6994e4ce.png)

</div>

</div>

<div id="8e021a5d" class="cell code" markdown="1" execution_count="61">

``` python
# now let's figure out whether detrended sleep hours and typing speed are correlated

speed_sleep_correlation, speed_sleep_p-value = pearsonr(sleep_hours_detrended, typing_speed_detrended)
print('typing speed correlation: ', speed_sleep_correlation)
print('typing speed p-value: ', speed_sleep_p-value)

z = np.polyfit(sleep_hours_detrended, typing_speed_detrended, 1)
p = np.poly1d(z)
plt.scatter(sleep_hours_detrended, typing_speed_detrended)
plt.plot(sleep_hours_detrended, p(sleep_hours_detrended))
```

<div class="output stream stdout" markdown="1">

    typing speed correlation:  -0.0066709331270842775
    typing speed p-value:  0.9496779683405152

</div>

<div class="output execute_result" markdown="1" execution_count="61">

    [<matplotlib.lines.Line2D at 0x157fc7210>]

</div>

<div class="output display_data" markdown="1">

![](images/b5382a708e25faefda89072cc4fc39c1879401d5.png)

</div>

</div>

<div id="aba2da3e" class="cell code" markdown="1" execution_count="62">

``` python
speed_sleep_correlation, speed_sleep_p-value = pearsonr(sleep_hours, typing_speed_detrended)
print('typing speed correlation: ', speed_sleep_correlation)
print('typing speed p-value: ', speed_sleep_p-value)

z = np.polyfit(sleep_hours, typing_speed_detrended, 1)
p = np.poly1d(z)
plt.scatter(sleep_hours, typing_speed_detrended)
plt.plot(sleep_hours, p(sleep_hours))
```

<div class="output stream stdout" markdown="1">

    typing speed correlation:  -0.0056782230053800675
    typing speed p-value:  0.9571587555674622

</div>

<div class="output execute_result" markdown="1" execution_count="62">

    [<matplotlib.lines.Line2D at 0x16a041690>]

</div>

<div class="output display_data" markdown="1">

![](images/f73d5eedd8a619c1f29a6bb194ce0d898db00194.png)

</div>

</div>
