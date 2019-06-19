<font size=10>Table of Contents</font>  
&emsp;&emsp;Part I: Introduction to the dataset  
&emsp;&emsp;Part II: Asking questions  
&emsp;&emsp;Part III: Solving the question  
  
  
  
<font size=8>Part I: Introduction to the dataset  
&emsp;&emsp;Country  
&emsp;&emsp;Region  
&emsp;&emsp;Population  
&emsp;&emsp;Area (sq. mi.)：Square mile  
&emsp;&emsp;Pop. Density (per sq. mi.)：Population density (per square mile)  
&emsp;&emsp;Coastline (coast/area ratio)  
&emsp;&emsp;Net migration  
&emsp;&emsp;Infant mortality (per 1000 births)  
&emsp;&emsp;GDP ($ per capita)  
&emsp;&emsp;Literacy (%)：Reflecting the rate of education  
&emsp;&emsp;Phones (per 1000)：the rate of having a mobile phone per 1,000 people  
&emsp;&emsp;Arable (%)：Arable area accounts for the total area  
&emsp;&emsp;Crops (%)：Crop area accounts for the total area  
&emsp;&emsp;Other (%)：other area accounts for the total area  
&emsp;&emsp;Climate：Climate comfort  
&emsp;&emsp;Birthrate  
&emsp;&emsp;Deathrate  
&emsp;&emsp;Agriculture：The proportion of agriculture in the three major industries  
&emsp;&emsp;Industry：The proportion of industry in the three major industries  
&emsp;&emsp;Service：The proportion of service in the three major industries  
  
  
  
  
<font size=8>Part II: Asking questions  
&emsp;&emsp;1.The distribution of population, area, and population density  
&emsp;&emsp;2.The relationship between population and area, and climate  
&emsp;&emsp;3.Comparison of population and birth rate, mortality rate, and mobility rate  
&emsp;&emsp;4.Distribution of population density on the earth  
&emsp;&emsp;5.Comparison of GDP per capita in each region  
&emsp;&emsp;6.Comparison of arable area and crop area to total area  
&emsp;&emsp;7.The composition ratio of agriculture, industry and service industry  
  
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
&emsp;df[col]=df[col].astype(str).str.replace(',','.').astype(float)  
  
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
&emsp;return '$%.1f$x$10^{9}$' % (x/1000000000)  
  

  
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
  
#基于上面的结果，继续研究人口密度  
plt.subplots(1,1,figsize=(10,5))  
plt.bar(x=df2['Pop. Density (per sq. mi.)'].index,height=df2['Pop. Density (per sq. mi.)'])  
plt.title('Population density of the world',pad=30,fontsize=20)  
plt.xlabel('Region',labelpad=30,fontsize=15)  
plt.ylabel('Population density(per sq. mi.)',labelpad=30,fontsize=15)  
plt.xticks(rotation=90)  
  
![2](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/2Population%20density%20of%20the%20world.png)  
  
#人口组成成分，人口净增长率和迁移率的关系  
df2['Net Growth Rate']=df2['Birthrate']-df2['Deathrate']  
  
bar_width=0.3  
N = len(df1)  
index = np.arange(N)  
  
fig, ax = plt.subplots(1,1,figsize=(10,5))  
ax.bar(index -  bar_width/2, df2['Net Growth Rate'],bar_width)  
ax.bar(index +  bar_width/2, df2['Net migration'],bar_width)  
plt.title('Population Composition of the World',pad=30,fontsize=20)  
plt.xlabel('Region',labelpad=30,fontsize=15)  
plt.ylabel('Rate',labelpad=30,fontsize=15)  
plt.xticks(np.arange(N),df2['Net Growth Rate'].index,rotation=90)  
ax.legend(('Net Growth Rate','Net migration'))  
  
![3](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/3Population%20Composition%20of%20the%20Worldpng.png)  
  
  
#在地图上显示人口密度分布图  
  
fig=plt.figure(figsize=(15,15))  
  
map = Basemap(projection='mill',lon_0=180)  
  
map.drawcoastlines()  
map.drawcountries()  
map.drawparallels(np.arange(-90,90,30),labels=[1,0,0,0])  
map.drawmeridians(np.arange(map.lonmin,map.lonmax+30,60),labels=[1,0,0,0])  
#map.drawmapboundary(fill_color='aqua')  
#map.fillcontinents(color='coral',lake_color='aqua')  
  
  
#lons=dict({'ASIA (EX. NEAR EAST)':22.,'C.W. OF IND. STATES':21.,'NEAR EAST':31.,'NORTHERN AMERICA':47., 'LATIN AMER. & CARIB':-1.,'OCEANIA':-23.,'NORTHERN AFRICA':23.,'SUB-SAHARAN AFRICA':-8.,'EASTERN EUROPE':63.,'WESTERN EUROPE':50.,'BALTICS':58.})  
#lats=dict({'ASIA (EX. NEAR EAST)':55.,'C.W. OF IND. STATES':21.,'NEAR EAST':114.,'NORTHERN AMERICA':-76.,'LATIN AMER. & CARIB':-116.,'OCEANIA':134.,'NORTHERN AFRICA':11.,'SUB-SAHARAN AFRICA':26.,'EASTERN EUROPE':88.,'WESTERN EUROPE':14.,'BALTICS':20.})  
  
#要显示的散点的经纬度  
lats=np.array([31.,70.,45.,47.,-10.,-23.,20.,-10.,60.,45.,60.])  
lons=np.array([114.,20.,50.,255.,290.,134.,20.,26.,110.,14.,30.])  
  
#要显示的散点的经纬度  
size=df2['Pop. Density (per sq. mi.)']/df2['Pop. Density (per sq. mi.)'].max()  
size=size.values  
  
x, y = map(lons,lats)  
map.scatter(x,y,s=size*500,marker='o',color='g')  
  
plt.title('Population density distribution',fontsize=30)  
plt.show()  
  
![4](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/4Population%20density%20distribution.png)  
  
#人均GDP对比  
plt.subplots(1,1,figsize=(10,5))  
plt.bar(x=df2['GDP ($ per capita)'].index,height=df2['GDP ($ per capita)'],width = 0.5)  
plt.title('Per capita GDP of the world',pad=20,fontsize=20)  
plt.xlabel('Region',labelpad=30,fontsize=15)  
plt.ylabel('GDP ($ per capita)',labelpad=30,fontsize=15)  
plt.xticks(rotation=90)  
plt.show()  

![5](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/5Per%20capita%20GDP%20of%20the%20world.png)  
  
#产业分布比例  
plt.subplots(1,1,figsize=(10,10))  
ind = df2.index  
width=0.5  
p1 = plt.barh(ind, df2['Agriculture'] ,width)  
p2 = plt.barh(ind, df2['Industry'],width,left=df2['Agriculture'])  
p3 = plt.barh(ind, df2['Service'],width,left=df2['Industry'])  

plt.title('Scores by group and gender',pad=20,fontsize=25)  
plt.ylabel('Region',labelpad=30,fontsize=20)  
#plt.yticks(rotation=90)  
plt.legend((p1[0], p2[0],p3[0]), ('Agriculture', 'Industry' , 'Service'))  
  
plt.show()  
  
![6](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/6Scores%20by%20group%20and%20genderpng.png)  
  
#可耕面积和农作物比例  
bar_width=0.3  
N = len(df2)  
index = np.arange(N)  
  
fig, ax = plt.subplots(1,1,figsize=(10,5))  
ax.bar(index -  bar_width/2, df2['Arable (%)'],bar_width)  
ax.bar(index +  bar_width/2, df2['Crops (%)'],bar_width)  
plt.title('Farming situation of the World',pad=30,fontsize=20)  
plt.xlabel('Region',labelpad=30,fontsize=15)  
plt.ylabel('Rate',labelpad=30,fontsize=15)  
plt.xticks(np.arange(N),df2['Arable (%)'].index,rotation=90)  
ax.legend(('Arable area ratio','Crop area ratio'))  
plt.show()  
  
![7](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/7Farming%20situation%20of%20the%20World.png)  
