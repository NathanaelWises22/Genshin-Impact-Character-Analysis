# Genshin-Impact-Character-Analysis

![30419](https://user-images.githubusercontent.com/92627169/162481274-ee3b17cd-5343-4225-b02f-2cc6cd5ba7d2.png)

I've been very invested in this game for over a year,  So when i came across an article https://medium.com/analytics-vidhya/exploratory-analysis-on-genshin-impact-characters-with-pandas-seaborn-are-liyues-stronger-than-f46a9af07fb5So which beautifully show an EDA about Genshin's characters and with the recent 2.6 update, Naturally, I want to try to make my own version of analysis, inspired by the article. 

With Kamisato Ayato joining, there's 48 Playable Character we can gacha, a question arise on my mind, "How strong is Ayato?", "How is he compared to other characters?" , "Is my Waifu the best Character??".
So on this occasion i tried to make a simple Exploratory Data Analysis about Genshin Impact character base stats at lvl 90.

This is not a recommendation about which character is the best. Because of the nature of the game, with different playstyle,elemental reaction,equipment and team composition etc, greatly affect the damage output. So, the best character is highly subjective according to the player preference.

Instead I try to make a simple comparison with base stat of ATK & DEF of the characters at level 90, and a few simple features such as Elements, Weapon Type & Nation.

From the data i tried to answer these few questions :
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
We'll be using Pandas to read html source table and creating Dataframe, and using Seaborn and Matplotlib for simple Data Visualization.

We'll be using the data from Genshin Impact Fandom Wiki. Please take note that I take the data on 5-4-2022.

From the genshin impact wiki there's 2 table that we want to take.

![2022-04-09 (3)](https://user-images.githubusercontent.com/92627169/162575951-165df46b-e464-4fa4-abfc-357f01b84f10.png)

![2022-04-09 (4)](https://user-images.githubusercontent.com/92627169/162575921-edbeb56e-8d53-4d2f-bebd-cc4ee8dc2daa.png)

```
url_1='https://genshin-impact.fandom.com/wiki/Characters/Comparison#Normal_Attacks'
url_2='https://genshin-impact.fandom.com/wiki/Characters'

#parsing table from url1 & url2 to dataframe
tables_1 = pd.read_html(url_1)
tables_2 = pd.read_html(url_2)
df1 = tables_1[1]
df2 = tables_2[3]

```
Using Pandas, we parse the table into a dataframe.
While the first table show the basic stats of HP,ATK and DEF. The second table show the characters Element,Weapon and Region.
After we get our data, let's remove any NaN column and merge the 2 datasets into 1.

```
df1 = df1.drop(['Icon'], axis = 1)
df2 = df2.drop(['Icon','Rarity'], axis = 1) #because the Icon and Rarity turn up NaN in th

df3 = df1.merge(df2)

df3.head()

```
![2022-04-09 (3)](https://user-images.githubusercontent.com/92627169/162576040-74ccba38-0357-4cf2-8119-fe612ba20cf9.png)

We get the new dataframe .

I want to make a comparison of the character gender, but because the newest table didn't include it I manually added it by creating a new column.
If you know a better way or a better code please help by expanding this repository.

```
df3.insert(5, "Gender", ['M', "F",  'F', "M", "F","F", 'M',"M", 'M','F',"F","F" ,"F","M","F","F",'M',"M","F" ,"M" ,"F" ,"F","F","F" ,"F" ,
                         "F","F","F","F","M","F","F","F","F",'F',"M","M","Player choice","M","F","M","M","F","F","F","F","F","M" ], True)
 ```
![2022-04-09](https://user-images.githubusercontent.com/92627169/162575962-0965274d-5062-408f-9561-71ad9e35ca64.png)

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

There's 31 Female character and 16 Male character in-game with 1 character (that being Traveller) that player can choose the gender themselves with a total of 48 character that you can play

```
ax = sns.countplot(x='Weapon',data = df3,palette= palette_weapon)
for p in ax.patches:
   ax.annotate('{:.1f}'.format(p.get_height()), (p.get_x()+0.25, p.get_height()+0.01))

plt.show()
```
![Weapon count](https://user-images.githubusercontent.com/92627169/162574882-972db713-3f74-4931-9b1a-e112d2fad40b.png)

Sword seems to be everyone favorite weapon
```
ax = sns.countplot(x='Region',data = df3,palette = palette_Region)
for p in ax.patches:
   ax.annotate('{:.1f}'.format(p.get_height()), (p.get_x()+0.25, p.get_height()+0.01))

plt.show()
```
![Character per region count](https://user-images.githubusercontent.com/92627169/162574891-674f18e6-3ed6-4142-be6d-80794ba2a400.png)

Mondstat currently have the most playable character with 18 characters. Meanwhile Snezhnaya currently only have Childe as PC ( Signora when??). This ignores The Traveller and Aloy who doens't have their own region.

```
ax = sns.countplot(x='Element',data = df3, palette = palette_element)
for p in ax.patches:
   ax.annotate('{:.1f}'.format(p.get_height()), (p.get_x()+0.25, p.get_height()+0.01))

plt.show()
```
![Element count](https://user-images.githubusercontent.com/92627169/162574887-a35c838d-342b-457c-8063-1028b5e2c103.png)

Apparently Pyro and Cryo are the most popular element with electro coming in second.

```
# plotting All character ATK & DEF 

sns.set(rc = {'figure.figsize':(15,8)})
sns.scatterplot(x=df3['DEF'], y=df3['ATK'], hue = df3["Element"], style = df3["Region"], palette = palette_final, s = 50);

for i in range(df3.shape[0]):
    plt.text(x=df3.DEF[i]+2.5,y=(df3.ATK[i]+5 if df3.Name[i]=="Beidou" else df3.ATK[i]-0.2),s=df3.Name[i], 
          fontdict=dict(color='black',size=8),
          bbox=dict(facecolor='pink',alpha=0.1))
```
![Attack-Def plot](https://user-images.githubusercontent.com/92627169/162574923-3d5b3c20-0510-4c33-a097-b1ab79a20863.png)

The character with highest base attack at lvl 90 is Xiao the Adepti, coming in second is Yae Miko,Eula,and Ayaka. With Itto being the character with highest base DEF at lvl 90, and  Qiqi,Albedo and HuTao coming in behind. 

```
# plotting All character ATK & HP 

sns.set(rc = {'figure.figsize':(15,8)})
sns.scatterplot(x=df3['ATK'], y=df3['HP'], hue = df3["Element"],style = df3["Region"], palette = palette_final, s = 100);

for i in range(df3.shape[0]):
    plt.text(x=df3.ATK[i]+2.5,y=(df3.HP[i]+5 if df3.Name[i]=="Beidou" else df3.HP[i]-0.2),s=df3.Name[i], 
          fontdict=dict(color='black',size=7),
          bbox=dict(facecolor='pink',alpha=0.1))
 ```
![Hp-ATK plot](https://user-images.githubusercontent.com/92627169/162574934-74e25030-a4f4-448f-86c3-c966f41a17c0.png)

The healthiest chara is The Ghost Girl HuTao with a base HP at lvl 90 being >15k, even higher than our Geo Daddy,Zhongli. And the most squishy character is Fischl & Sucrose

```
# compare Mondstadt & Liyue & Inazuma Element
ax= sns.catplot(x="Element",col='Region', kind="count", palette= palette_element , data=df3)
```
![Element per region](https://user-images.githubusercontent.com/92627169/162574941-b75c266b-3e29-473b-84ad-9762aa3a4f01.png)

```
# compare Mondstadt & Liyue & Inazuma Weapon
ax= sns.catplot(x="Weapon",col='Region', kind="count", palette= palette_weapon , data=df3)
```
![weapon per region count](https://user-images.githubusercontent.com/92627169/162574958-85e06e7b-c07f-456e-a8f4-8f5bd8188ef2.png)

While Catalyst seems to be the most used weapon in Mondstat, Polearm seems to be the most favorites in Liyue.

# Conclusion

The character with highest base ATK at lvl 90 is Xiao.
The character with highest base DEF at lvl 90 is Arataki Itto.
and the character with highest base HP at lvl 90 is HuTao.

This shows that while Ayato doesn't have a realy-really-really high ATK, He does have HP at higher tier compared to most of other character.

Currently, Mondstat have the most Playable Character. Could it be because it's the earliest region thats released in-game?
And it seems that Cryo and Pyro are both the most common Element in-game. I really anticipated when they released the last 'Dendro' element.

Because of other factors such as Playstyle, Elemental reaction, Team composition, Ascension Stat,Talent level etc, it is exponentially harder to determine with absolute certainty which is the best character. So I concluded that The best character is the most chacater you had fun with, your most favorite character. 
Yae Miko is my favorite character to boot.
Who's your favorite Genshin character?
