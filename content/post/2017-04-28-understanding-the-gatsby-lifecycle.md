---
title: Exploratory Data Analysis on canada649 Lottery
date: 2020-07-22T07:00:00+00:00
hero: "/images/hero-2.jpg"
excerpt: ''
timeToRead: 10
authors:
- M.K

---
Hello World!.

## **_#Data cleaning and preparation_**

#### **_#importing the libraries_**

    %matplotlib inline
    import os
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import matplotlib.dates as mdates
    from matplotlib.dates import DateFormatter
    
    #Handle date time conversions between pandas and matplotlib
    from pandas.plotting import register_matplotlib_converters
    register_matplotlib_converters()
    import  seaborn as sns
    
    
    #use white grid plot background from seaborn 
    #sns.set(font_scale = 1.5, style= "whitegrid")
    
    import random

#### **_#Assigning the keyword 'lottery' _**

    lottery = pd.read_csv('new649.csv',
    
                         parse_dates = ['DRAW DATE'],
    
                         index_col = ['DRAW DATE'],
    
                         na_values =[999.9])
    
    lottery.head()

#### **_#let check to see if there are any null values_**

    lottery.isnull().any().any()

> ##### thanksfully, the answer is FALSE

    lottery.count()

#### **_#let look at the shape of the data_**

    lottery.shape

#### **_#having a look at the 11 columns in the dataset_**

    lottery.columns

    lottery.info()

> ###### **_Looking at the data info, seems only the draw date is under an object catergories , while the rest are integers_**

> ###### **_Now removing the un_needed columns from the dataframe. such as "Product", "DRAW NUMBER", "SEQUENCE NUMBER"_**

#### **_#USING THE drop_column function_**

    drop_cols = ['PRODUCT', 'DRAW NUMBER', 'SEQUENCE NUMBER']

#### **_#assigning a new keyword to read the new dataframes from the dataset_**

    clean_lottery = lottery.drop(drop_cols, axis = 1)
    clean_lottery.head()

###### 

###### 

> ###### **_the product, draw number and sequence number columns are dropped_**
>
> ###### **_looking at the new dataset , there are only the numbers drawns and bonus. so i thought, there should be a sum of the all the numbers drawns_**
>
> ###### **_Here, a sum column will be added to the dataframe containing only the sum total of the drawn numbers from 1 to 6, including the bonus number_**
>
> ###### **_Now, we could do this in the old school way of having to type a long quote of codes like this  below :_**

    clean_lottery["TOTAL NUMBER"] = clean_lottery['NUMBER DRAWN 1'] + clean_lottery['NUMBER DRAWN 2'] + clean_lottery['NUMBER DRAWN 3'] + clean_lottery['NUMBER DRAWN 4'] + clean_lottery['NUMBER DRAWN 5'] + clean_lottery['NUMBER DRAWN 6'] + clean_lottery['BONUS NUMBER']
    clean_lottery.head()

**_But before checking out the other means of getting the sum total of the number drawn, first we have to remove the dataframe 'TOTAL NUMBER' from the dataset_**

    TOTAL = clean_lottery.pop('TOTAL NUMBER')
    TOTAL.head(10)

#### **_#Now to find out if it's really the dataframe has being popped out_**

    clean_lottery.head(10)

> **_It's sure has !_**
>
> **_Now shall we proceed ?..._**
>
> **_first we assign the clean_lottery dataset to a keyword column_list of the dataframe to sum up_**

#### **_#assigning the clean_lottery dataset to a keyword column_list of the dataframe to sum up_**

    column_list =  list(clean_lottery)

**_And then add the new dataframe to the dataset, assigning it to the column list with the 'sum' function to get the figures of the number drawns_**

    clean_lottery["TOTAL NUMBER"] = clean_lottery[column_list].sum(axis = 1)
    clean_lottery.head(20)

**_Now you see, and if you check the figures of this, comparing it with the former total number above , you will see it's the same... So we have it a more comfortable method_**

**_So we have the TOTAL NUMBER of the drawn numbers, but then looking at the figures , they are not in an order form, so let have they put in an order form using the sort_value function_**

    clean_lottery = clean_lottery.sort_values(by = 'TOTAL NUMBER', ascending = False)
    clean_lottery.head(20)

**_VOILA!!!,  Now we have them in an order state from the biggest to the smallest. Speaking of smallest , how about we have a look_**

    clean_lottery['TOTAL NUMBER'].max()

    clean_lottery['TOTAL NUMBER'].min()

**_48!! unbelievable..._**

**_let have a look at the Statistics Summary of the dataset_**

    clean_lottery.describe().transpose()

    clean_lottery['TOTAL NUMBER'].mode()

#### **_#shall we see how many times 170 occured_**

    clean_lottery['TOTAL NUMBER'].value_counts().to_frame().head(10)

**_Looking the output above, some total number occurred more than once. could it be that some drawn numbers from 1 to 6 with the bonus number occurred the same numbers to have given the same total number ?._**

**_Let have a look at the dataframe from number drawn 1 to bonus number for any duplicate_**

**_#checking for duplicate_**

    clean_lottery[['NUMBER DRAWN 1', 'NUMBER DRAWN 2', 'NUMBER DRAWN 3', 'NUMBER DRAWN 4', 'NUMBER DRAWN 5', 'NUMBER DRAWN 6', 'BONUS NUMBER']].duplicated().head(20)

**_Surprisingly, it's all came out as False._**

**_So what could have made the TOTAL NUMBER figures duplicate?, let have a llok at the duplicated dataframes in the TOTAL NUMBER_**

**_#checking the duplicated Numbers in the TOTAL NUMBER dataframe and outputting them with the full dataframe from the Date to the bonus number dataset_**

    dep_lottery = clean_lottery[clean_lottery.duplicated(['TOTAL NUMBER'])]
    dep_lottery.head(15)

**_Turn out the duplicated Total numbers are made up of different combinations of numbers from Number drawn 1 to the Bonus number, but ending up duplicating the TOTAL NUMBER_**

## **# Data Visualization**

**_So far so good, it's time to do some visualiztion_**

#### **_#lottery dates and time_**

    lotteryDate = clean_lottery['1984-02-25':'1984-11-12']
    lotteryDate.head()

fig, ax = plt.subplots(figsize=(12, 12))

\#add x-axis and y-axis

ax.bar(lotteryDate.index.values, lotteryDate\['NUMBER DRAWN 1'\],

color = 'red')

\#set title and labels for axes

ax.set(xlabel = 'Date',

  ylabel = 'NUMBER DRAWN 1',  title= 'Lotto')

plt.show()

fig, ax = plt.subplots(figsize=(12, 12))

ax.bar(lotteryDate.index.values, lotteryDate\['NUMBER DRAWN 2'\],

       color = 'purple')

\#set title and labels for axes

ax.set(xlabel = 'Date',

      ylabel = 'NUMBER DRAWN 1',
    
      title= 'Lotto')

plt.tight_layout()

    plt.show()