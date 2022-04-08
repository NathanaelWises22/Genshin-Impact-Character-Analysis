# Genshin-Impact-Character-Analysis

![30419](https://user-images.githubusercontent.com/92627169/162481274-ee3b17cd-5343-4225-b02f-2cc6cd5ba7d2.png)

I've been very invested in this game for over a year. So now with the new 2.6 update, I try to make my own simple analysis.
With Kamisato Ayato joining, there's 48 Playable Character we can gacha, a question arise in the minds of every player. "How strong is Ayato?"
On this occasion i tried to make a simple Exploratory Data Analysis about Genshin Impact character base stats at lvl 90.

This is not a recommendation about which character is the best. Because of the nature of the game with different playstyle,elemental reaction,equipment and team composition etc, greatly affect the character damage output. So, the best character is highly subjective according to the player preference.

Instead I try to make a simple comparison with base stat of ATK & DEF of the characters at level 90, and a few simple features such as Elements, Weapon Type & Nation.

From the data i tried to answer these few questions I had, which is :
  1. Who is the character with highest base ATK? From which Region?
  2. Who is the character with highest base Def?
  3. Who is the character with highest base HP?
  4. How's the comparison between the number of Playable Character per Region?
  5. How's the comparison between the number of Playable Character per Weapon type?
  6. How's the distribution of Vision per region?
 
 First let's import the important library
 
 ```
import pandas as pd
import seaborn as sns
import numpy as np
from matplotlib import pyplot as plt
```
Using Pandas to read html source table and creating Dataframe, and using Seaborn and Matplotlib for simple Data Visualization.

We'll be using the data from Genshin Impact Fandom Wiki. Please take note that the data used is last updated on 5-4-2022.
From the genshin impact wiki there's 2 table that we want to take.

```
url_1='https://genshin-impact.fandom.com/wiki/Characters/Comparison#Normal_Attacks'
url_2='https://genshin-impact.fandom.com/wiki/Characters'

#parsing table from url1 & url2 to dataframe
tables_1 = pd.read_html(url_1)
tables_2 = pd.read_html(url_2)
df1 = tables_1[1]
df2 = tables_2[3]

```

Using Pandas, we parse the table into dataframe






Yae Miko is my favorite character to boot. Who's your favorite Genshin character?
