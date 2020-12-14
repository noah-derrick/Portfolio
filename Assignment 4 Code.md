```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
```


```python
cond_labels = ['CTRL', 'ADAPT']

contr_labels = [4, 8, 12, 16, 24, 32, 48, 64, 84, 100]

rep_labels = list(np.arange(1, 8))
num_reps = len(rep_labels)

time_labels = list(np.arange(4000))

stim_on_time = 2000
stim_off_time = stim_on_time + 1000

adapt_on_time = 0
adapt_off_time = adapt_on_time + 2000
```


```python
df = pd.read_csv('crowder_1_neuron.csv')
df
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
      <th>neuron</th>
      <th>time</th>
      <th>repetition</th>
      <th>contrast</th>
      <th>condition</th>
      <th>spike</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>m6_11</td>
      <td>0</td>
      <td>1</td>
      <td>4</td>
      <td>CTRL</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>m6_11</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>CTRL</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>m6_11</td>
      <td>2</td>
      <td>1</td>
      <td>4</td>
      <td>CTRL</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>m6_11</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>CTRL</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>m6_11</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>CTRL</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>639995</th>
      <td>m6_11</td>
      <td>3995</td>
      <td>8</td>
      <td>100</td>
      <td>ADAPT</td>
      <td>0</td>
    </tr>
    <tr>
      <th>639996</th>
      <td>m6_11</td>
      <td>3996</td>
      <td>8</td>
      <td>100</td>
      <td>ADAPT</td>
      <td>0</td>
    </tr>
    <tr>
      <th>639997</th>
      <td>m6_11</td>
      <td>3997</td>
      <td>8</td>
      <td>100</td>
      <td>ADAPT</td>
      <td>0</td>
    </tr>
    <tr>
      <th>639998</th>
      <td>m6_11</td>
      <td>3998</td>
      <td>8</td>
      <td>100</td>
      <td>ADAPT</td>
      <td>0</td>
    </tr>
    <tr>
      <th>639999</th>
      <td>m6_11</td>
      <td>3999</td>
      <td>8</td>
      <td>100</td>
      <td>ADAPT</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>640000 rows × 6 columns</p>
</div>




```python
fig = plt.figure()
rep = 1
contr = 100
cond = 'CTRL'

for rep in rep_labels:
    spike_times = df[(df['repetition']==rep) & (df['contrast']==contr) & (df['condition']==cond) & (df['spike']==1)].loc[:, 'time']

    plt.vlines(spike_times, rep-1, rep);

    plt.axvspan(stim_on_time, stim_off_time, alpha=contr/100, color='grey')

    plt.xlim([0, max(time_labels)+1])
    plt.ylim([0, len(rep_labels)])
    plt.title(cond + ' ' + str(contr) + '% contrast')
    plt.xlabel('Time (ms)')
    plt.ylabel('Repetition',)
    plt.yticks([x + 0.5 for x in range(num_reps)], [str(x + 1) for x in range(num_reps)], size=8) 
    plt.tight_layout()

```




    
![png](Assignment%204%20Code_files/Assignment%204%20Code_3_0.png)
    




```python
fig = plt.figure(figsize=[12,18])

subplot_counter = 1

for contr in contr_labels:
    for cond in cond_labels:

        ax = fig.add_subplot(len(contr_labels), len(cond_labels), subplot_counter)

        if cond == 'ADAPT':
            plt.axvspan(0, stim_on_time-1, alpha=0.5, color='grey')

        plt.axvspan(stim_on_time, stim_off_time, alpha=contr/100, color='grey')

        for rep in rep_labels:
            spike_times = df[(df['repetition']==rep) & (df['contrast']==contr) & (df['condition']==cond) & (df['spike']==1)].loc[:, 'time']

            plt.vlines(spike_times, rep-1, rep);

        plt.xlim([0, max(time_labels)+1])
        plt.ylim([0, len(rep_labels)])
        plt.title(cond + ' ' + str(contr) + '% contrast')
        plt.xlabel('Time (ms)')
        plt.ylabel('Repetition',)
        plt.yticks([x + 0.5 for x in range(num_reps)], [str(x + 1) for x in range(num_reps)], size=8) 
        plt.tight_layout()
        subplot_counter += 1
        
plt.show()
```




    
![png](Assignment%204%20Code_files/Assignment%204%20Code_4_0.png)
    


