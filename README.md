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

Using Pandas, we parse the table into a dataframe

After we get our data, let's remove any NaN column and merge the 2 datasets into 1.


We get the new dataframe .

I want to make a comparison of the character gender, but because the newest table didn't include it I manually added it by creating a new column.
If you know a better way or a better code please help by expanding this repository.

After we done with our dataframe, let's move on to our visualization.
First lets define the color palette dictionary for a better visual.

```
palette_element ={"Pyro": "#FA1A0D", "Hydro": "#0D8BC4", "Geo": "#FD8D04", "Anemo": "#4CD95A", "Electro" : "#B071C1", "Cryo": "#85D8DF", "None":"#E3E3E3"}
palette_Region ={"Mondstadt": "#ABCE30", "Liyue": "#FA7711", "Snezhnaya": "#58B4EE", "Inazuma": "#B071C1", "Nan":"#E3E3E3"}
palette_weapon ={"Sword": "#FA1A0D", "Bow": "#0D8BC4", "Catalyst": "#FD8D04", "Claymore": "#4CD95A","Polearm" : "#B071C1"}
palette_sex ={"M": "#0D8BC4", "F": "#FA1A0D", "Player choice": "#E3E3E3"}


palette_final = dict(palette_weapon)
palette_final.update(palette_element)
palette_final.update(palette_Region)
palette_final.update(palette_sex)


# define only important variable
categorical = ['Region', 'Weapon', 'Element', "Gender"]
numerical = ['ATK', 'DEF']

```

If you notice that in element & region I include a "None" and "Nan", both are for the Traveller and Aloy which doesn't have their respective element or region.

First, lets take a look about the Gender comparison of Playable character in the game.

```
ax = sns.countplot(x='Gender',data = df3, palette= palette_sex)
for p in ax.patches:
   ax.annotate('{:.1f}'.format(p.get_height()), (p.get_x()+0.25, p.get_height()+0.01))

plt.show()
```
![Male vs Female Character](https://user-images.githubusercontent.com/92627169/162573813-0644a288-547d-4a56-971a-54ab11901342.png)




Yae Miko is my favorite character to boot. Who's your favorite Genshin character?
