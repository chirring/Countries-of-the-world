import numpy as np  
import pandas as pd  
import matplotlib.pyplot as plt  
import seaborn as sns  
%matplotlib inline  
from matplotlib.ticker import FuncFormatter  
from mpl_toolkits.basemap import Basemap  
import matplotlib.pyplot as plt  

df=pd.read_csv('C:/Users/Administrator/Desktop/countries of the world.csv')  

#去除无用列，把数据的","替换为".",用平均值填充缺失数据  
df=df.drop(['Other (%)','Climate'],axis=1)  

df['Region']=df['Region'].str.strip()  
for col in df[['Pop. Density (per sq. mi.)','Coastline (coast/area ratio)','Net migration','Infant mortality (per 1000 births)','GDP ($ per capita)','Literacy (%)','Phones (per 1000)','Arable (%)','Crops (%)','Birthrate','Deathrate','Agriculture','Industry','Service']]:  
    df[col]=df[col].astype(str).str.replace(',','.').astype(float)  
  
df.fillna(df.mean())  
  
#以'Region'分组，研究'Population','Area (sq. mi.)'，因为它们总和有意义  
df1=df[['Region','Population','Area (sq. mi.)']]  
df1=df1.groupby('Region').sum()  

#以'Region'分组，研究其他列，因为它们平均数有意义  
df2=df[['Region','Pop. Density (per sq. mi.)','Coastline (coast/area ratio)','Net migration','Infant mortality (per 1000 births)','GDP ($ per capita)','Literacy (%)','Phones (per 1000)','Arable (%)','Crops (%)','Birthrate','Deathrate','Agriculture','Industry','Service']]  
df2=df2.groupby('Region').mean()  
  
#一起研究'Population'和'Area (sq. mi.)'  
  
N = len(df1)  
y1 = df1['Population']  
y2 = df1['Area (sq. mi.)']  
index = np.arange(N)  
  
def formatnum(x, pos):  
    return '$%.1f$x$10^{9}$' % (x/1000000000)  
  
# def formatnum(x, pos):  
#     return '$%.1f$x$10^{7}$' % (x/10000000)  
  
fig, ax1 = plt.subplots(1,1,figsize=(10,5))  
ax2 = ax1.twinx()  
  
bar_width = 0.3  
ax1.bar(index -  bar_width/2 , y1, bar_width , color='y')  
ax2.bar(index +  bar_width/2 , y2, bar_width , color='b')  
  
ax1.set_title('Population and Area of Different Region of the world',fontsize=25,pad=30)  
ax1.set_xlabel('Region',fontsize=20)  
ax1.set_ylabel('Population',fontsize=20)  
ax2.set_ylabel('Area',fontsize=20)  
  
plt.xticks(np.arange(N))  
ax1.set_xticklabels(df1.index,rotation=90 )  
  
formatter = FuncFormatter(formatnum)  
  
ax1.yaxis.set_major_locator(formatter)  
ax1.yaxis.set_major_formatter(formatter)  
ax1.set_yticks(np.arange(0,4000000000,step=1000000000))  
  
formatter = FuncFormatter(formatnum)  
  
ax2.yaxis.set_major_locator(formatter)  
ax2.yaxis.set_major_formatter(formatter)  
ax2.set_yticks(np.arange(0,30000000,step=10000000))  
  
plt.show()  
  
![1](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/1Population%20and%20Area%20of%20Different%20Region%20of%20the%20world.png)  
