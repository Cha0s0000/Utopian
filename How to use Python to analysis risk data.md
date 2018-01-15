## **Before reading:**

I write it on my PC typora ,but  the passage may go wrong when I copy the passage to Utopian post editor.So I upload to the Github. If the article layout  make you dazzled please read on the below url.Thanks.

[The more readable version](https://github.com/Cha0s0000/Utopian/blob/master/How%20to%20use%20Python%20to%20analysis%20risk%20data.md)



---

## 1.Brief introduction

With the growth of network information security data scale, application of data analysis technology for network security is becoming more and more important.Through **Honeypot** log data ,let us use Python to briefly analyse what the  public do by using proxy IP.

> When some hackers or technicians on the Internet want to hack something, they need to hide their IP so that they will use proxy IP to hide their identity . The **Honeypot** is an active defense security technology model, which is designed to be a special attack or intrusion and deception object .The **Honeypot** is set up to protect system, as well as  being  used to collect the hacker information.

----

## 2. Python  package introduction

- [Panda](https://github.com/pandas-dev/pandas)

  > **Pandas** is a Python package providing fast, flexible, and expressive data
  > structures designed to make working with "relational" or "labeled" data both
  > easy and intuitive. It aims to be the fundamental high-level building block for
  > doing practical, real world data analysis in Python. Additionally, it has
  > the broader goal of becoming **the most powerful and flexible open source data
  > analysis / manipulation tool available in any language**. It is already well on
  > its way toward this goal.

- [**Matplotlib**](https://github.com/matplotlib/matplotlib)

  > **Matplotlib** is a Python 2D plotting library which produces publication-quality
  > figures in a variety of hardcopy formats and interactive environments across
  > platforms. Matplotlib can be used in Python scripts, the Python and IPython
  > shell (à la MATLAB or Mathematica), web application servers, and various
  > graphical user interface toolkits.

----

## 3.Analyse datas step by step

- ### Import Python package 

  ```
  import pandas as pd 
  from datetime import timedelta , datetime 
  import matplotlib.pylpot as plt 
  import numpy as np 
  ```

  ​

- ### Get data log

  We prepare the proxy IP using log as the data which we used to analyse.The proxy IP log is stored in the form of CSV.To read data from the CSV file,just typing "pandas.read_csv".  One line of code can read all the data to a two-dimensional table structure DataFrame variable .

  ``` 
  analysis_data = pd.read_csv('./honeypot_data/csv')
  ```

  Of course ,Pandas provides a IO tool which allows large files to be read in block. It is really convient for us. I have tried to read large files to test the  performance of the IO  tool, completely  loading of about 215 billion 300 million data will probably only need 90 seconds, the performance is quite good.



- ### Generally check the data

  In general, before analysis of the data ,we had better first have a general 
  understanding of the data, such as the total number of data, data variables, data distribution , data duplication, missing data,abnormal data and so on .

  ​

  - Several simple functions 

    ```
     #view the rows and columns of the datra
    anslysis_data.shape()

    # view the first 5 lines of data
    anslysis_data.head()  # you can also input parameter

    # view the last 5 lines of data
    anslysis_data.tail()   # you can also input parameter

    ```

  - List information about user proxy IP dates, proxy header information, proxy access to domain names, proxy methods, source IP, and honeypot node and so on.

    ```
    df.select_dtypes(include=['0']).describe()
    ```

    ![](https://steemitimages.com/DQmU5AAkgZL85pfMLTyKciuej5GFfFXtrCv3Lp4MF8QBHGB/%E5%9B%BE%E7%89%87.png)

    ```
    df.select_dtypes(include=['float64']).describe()
    ```

    ![](https://steemitimages.com/DQmRQLKfmnLwgjVRrrdsxkXfyha8TmyLENqvUNnz7ngrUg6/%E5%9B%BE%E7%89%87.png)

  ***Find fields such as scan_os_sub_fp, scan_scan_mode, and so on whose values may be NaNN**.*

  ​

- ### Data cleaning

  Because the source data usually contains some empty values or even empty columns, it will affect the time and efficiency of data analysis. After previewing the data summarization, these invalid data need to be processed.

  ```
  # Remove all rows containing empty values
  analysis_data.dropna()

  #Remove all columns that are empty values
  analysis_data.dropna(axis=1,how='all')

  #Remove data from the specified column,like'proxy_host' and 'srcip'
  analysis_data.dropna(subset=['proxy_host','srcip]')

  #Remove rows with a value less than 10 in all row fields
  analysis_data.dropna(thresh=10)
  ```

  ​

- ### statistical analysis

  - ###### After a preliminary understanding of some of the information in the data,we use the data slice method of pandas,'loc'.

    ` loc([start_row_index:end_row_index,[‘timestampe’, ‘proxy_host’, ‘srcip’]]) `

  ```
  # Select the variables :'timestampe', 'proxy_host', 'srcip'
  analysis_data = analysis_data.loc([:,['timestampe', 'proxy_host', 'srcip']])
  ```

  - ###### Let's first look at the daily use of data in the honeypot agent. We count the data on a daily basis, understand the daily amount of data PV, and draw the results out of the trend.

   ```
  daily_proxy_data = analysis_data[analysis_data.module=='proxy']
  daily_proxy_visited_count = daily_proxy_data.timestamp.value_counts().sort_index()
  daily_proxy_visited_count.plot()
   ```

  ![](https://steemitimages.com/DQmahLnYM4jhhRwMJvvTqFpodnPHXHK2wNkNivdJ1CkKdFC/%E5%9B%BE%E7%89%87.png)

  *To discard the data column, in addition to an invalid value and requirement, some redundant column table itself also needs to be cleared in this link, such as DataFrame and index, the type of description, by discarding of these data, and generate new data, can make the data capacity to effectively reduce and improve. Computational efficiency*

  ​

  - ###### From the above analysis, the use of honeypot agents was explosively increased in some days. So the data are not normal these days.Let's take a look at the amount of IP data and its growth per day. You can calculate the amount of IP data per day by using the ***nunique ()*** method directly after ***groupby***.

  ```
  daily_proxy_data = analysis_data[analysis_data.module=='proxy']
  daily_proxy_visited_count = daily_proxy_data.groupby(['proxy_host']).srcip.nunique()
  daily_proxy_visited_count.plot()
  ```

  ![](https://steemitimages.com/DQmNcvU2RU55agpBJgdqNJfKJNxovmPzs1GsiCjWCVPhjom/%E5%9B%BE%E7%89%87.png)

  ​

  - ###### we select the host and IP fields, groupby method to group each domain name (host), and then unique statistics in the IP access for each domain name.

  ```
  host_associate_ip = proxy_data.loc[:,['proxy_host','srcip']]
  grouped_host_ip = host_associate_ip.groupby(['proxy_host']).srcip.nunique()
  print(grouped_host_ip.sort_values(ascending=False).head(10))
  ```

  ![](https://steemitimages.com/DQmbc6HguEHRGnwTAFX4NfXMEqFeENZsDGj3DG9WTFJzDcL/%E5%9B%BE%E7%89%87.png)	

  Check the log data to and find information such as the collection of second-hand car prices, workers' recruitment and so on. From the hot point of host, generally we use the main agent or access to Baidu, QQ, Google, Bing that even woman and children all know site information.

  ​

  - ###### Then let's see who uses proxy IP to do something bad  most, which is to see whose IP has access to the largest number of different host.

  ```
  host_associate_ip = proxy_data.loc[:,['proxy_host','srcip']]
  grouped_host_ip = host_associate_ip.groupby(['srcip']).proxy_host.nunique()
  print(grouped_host_ip.sort_values(ascending=False).head(10))
  ```

  ![](https://steemitimages.com/DQmePCwgjEN58ZtVHqFtSxgFddxupKJwhcLiAMKAYnjF2G1/%E5%9B%BE%E7%89%87.png)	

  Well, the guy whose IP is 123..*.155 has a lot of access records and then looks at the log. He has been collecting hotel information in a large amount.

  ​

  - ###### We'll probably know what the are doing , and let's see how long they use proxy, who uses proxy for a long time.

  ```
  date_ip = analysis_data.loc[:,['timestamp','srcip']]
  grouped_date_ip = date_ip.groupby(['timestamp','srcip'])

  #Calculate the date of every src IP using the proxy 
  all_srcip_duratime_times = ....
  #Calculate the longest one
  duration_date_cnt = count_date(all_srcip_duration_times)
  ```

  ![](https://steemitimages.com/DQme59wWzABCB1zYpyJVJoxHtanausE6NbaRjmWpvcjFU1o/%E5%9B%BE%E7%89%87.png)

  Well, I'll have a little understand of what those people do, who is the longest, and so on. Get the log of the users whose IP = 80... 38 .And find that the young man had been getting the Sohu images for a long time.

  - ###### The honeypot has deployed multiple nodes throughout the country, and let us look at the total number of honeypot nodes for each source IP scanning, and to understand the coverage of the IP scanning nodes. The results are as follows

  ```
  node = df[df.module=='scan']
  node = node.loc[:,['srcip','origin_details']]
  grouped_node_count = node.groupby(['srcip']).count()
  print grouped_node_count.sort_values(['origin_details'],ascending=False).head(10)
  ```

  ![](https://steemitimages.com/DQmefAL6YJoMQnRwfriJW6zES7X1ucVgDg8HsAceqBjg55p/%E5%9B%BE%E7%89%87.png)

  ​

  From the above two tables, we can conclude some conclusions:  the user whose source IP is 182... 205  to scan the honeypot nodes for such a long time that it is marked as  dangerous user.

----

# That is all .Thanks for reading patiently.
