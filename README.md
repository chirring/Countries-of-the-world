## Table of Contents 目录  
* Part I: Introduction to the dataset  
* Part II: Asking questions  
* Part III: Solving the question  
*  &emsp;&emsp;1.Overview of the data set  
*  &emsp;&emsp;2.Handling missing data  
*  &emsp;&emsp;3.Population and Area of Different Region of the world  
*  &emsp;&emsp;4.Population density of the world  
*  &emsp;&emsp;5.Population Composition of the World  
*  &emsp;&emsp;6.Population density distribution  
*  &emsp;&emsp;7.Per capita GDP of the world  
*  &emsp;&emsp;8.Industrial distribution ratio   
*  &emsp;&emsp;9.Farming situation of the World  
* Part IV: Conclusion  

* 第一部分:介绍数据集  
* 第二部分:提问  
* 第三部分:解决问题  
* &emsp;&emsp;1.数据集的概述  
* &emsp;&emsp;2.处理缺失数据  
* &emsp;&emsp;3.世界不同地区的人口和面积  
* &emsp;&emsp;4.世界人口密度  
* &emsp;&emsp;5.世界人口组成  
* &emsp;&emsp;6.人口密度分布  
* &emsp;&emsp;7.世界人均GDP  
* &emsp;&emsp;8.工业分布比例  
* &emsp;&emsp;9.世界农业状况  
* 第四部分:结论  
  
  
  
  
  
## Part I: Introduction to the dataset
The data set contains 20 fields, including：  
>Country  
>Region  
>Population  
>Area (sq. mi.)：Square mile  
>Pop. Density (per sq. mi.)：Population density (per square mile)  
>Coastline (coast/area ratio)  
>Net migration  
>Infant mortality (per 1000 births)  
>GDP ($ per capita)  
>Literacy (%)：Reflecting the rate of education  
>Phones (per 1000)：the rate of having a mobile phone per 1,000 people  
>Arable (%)：Arable area accounts for the total area  
>Crops (%)：Crop area accounts for the total area  
>Other (%)：other area accounts for the total area  
>Climate：Climate comfort  
>Birthrate  
>Deathrate  
>Agriculture：The proportion of agriculture in the three major industries  
>Industry：The proportion of industry in the three major industries  
>Service：The proportion of service in the three major industries  

## 第一部分:数据集简介  
数据集包含20个字段，包括:  
>国家  
>区域  
>人口  
>面积(平方。mi):平方英里  
>流行。密度(每平方。密歇根州):人口密度(每平方英里)  
>海岸线(海岸/面积比)  
>净移民  
>婴儿死亡率(每1000个婴儿)  
>国内生产总值(人均美元)  
>识字率(%):反映受教育程度  
>手机(每1000人):每1000人拥有手机的比率  
>耕地(%):耕地面积占总面积  
>作物(%):作物面积占总面积  
>其他(%):其他区域占总面积  
>气候:气候舒适  
>出生率  
>死亡率  
>农业:农业在三大产业中的比重  
>产业:产业在三大产业中的比重  
>服务:服务在三大行业中的比重  
  
  
  
  
  
  
## Part II: Asking questions  
We want to know  
* 1.The distribution of population, area, and population density  
* 2.The relationship between population and area, and climate  
* 3.Comparison of population and birth rate, mortality rate, and mobility rate  
* 4.Distribution of population density on the earth  
* 5.Comparison of GDP per capita in each region  
* 6.Comparison of arable area and crop area to total area  
* 7.The composition ratio of agriculture, industry and service industry  

## 第二部分:问问题  
我们想知道  
* 1.人口、面积和人口密度的分布  
* 2.人口、面积和气候的关系  
* 3.人口与出生率、死亡率和流动率的比较  
* 4.地球上人口密度的分布  
* 5.各地区人均GDP的比较  
* 6.耕地面积和作物面积与总面积的比较  
* 7.农业、工业和服务业的构成比例  
  
  
  
  
## Part III: Solving the question   
## 第三部分:解决问题  

### 1.  Overview of the data set   
### 1.  数据集的概述  

    import numpy as np  
    import pandas as pd  
    import matplotlib.pyplot as plt  
    import seaborn as sns  
    %matplotlib inline  
    from matplotlib.ticker import FuncFormatter  
    from mpl_toolkits.basemap import Basemap  
    import matplotlib.pyplot as plt  

    df=pd.read_csv('C:/Users/Administrator/Desktop/countries of the world.csv')  

### 2.  Handling missing data  
### 2.  处理缺失值   

    #Remove the useless columns  
    df=df.drop(['Other (%)','Climate'],axis=1)  


    #Replace the "," with ".", and fill the missing data with the average
    df['Region']=df['Region'].str.strip()  
    for col in df[['Pop. Density (per sq. mi.)','Coastline (coast/area ratio)','Net migration','Infant mortality (per 1000 births)','GDP ($ per capita)','Literacy (%)','Phones (per 1000)','Arable (%)','Crops (%)','Birthrate','Deathrate','Agriculture','Industry','Service']]:  
        df[col]=df[col].astype(str).str.replace(',','.').astype(float)  
    df.fillna(df.mean())  
  
  
    #Group by'Region', and study 'Population', 'Area (sq. mi.)' because they are summed to be meaningful
    df1=df[['Region','Population','Area (sq. mi.)']]  
    df1=df1.groupby('Region').sum()  


    #Group by 'Region', and study the other columns because their averages make sense
    df2=df[['Region','Pop. Density (per sq. mi.)','Coastline (coast/area ratio)','Net migration','Infant mortality (per 1000 births)','GDP ($ per capita)','Literacy (%)','Phones (per 1000)','Arable (%)','Crops (%)','Birthrate','Deathrate','Agriculture','Industry','Service']]  
    df2=df2.groupby('Region').mean()  
  
  

### 3.  Population and Area of Different Region of the world  
### 3.  世界不同地区的人口和面积  
  
    #Set some common parameters for drawing
    N = len(df1)  
    y1 = df1['Population']  
    y2 = df1['Area (sq. mi.)']  
    index = np.arange(N)  
  
    #study 'Population', 'Area (sq. mi.)'
    def formatnum(x, pos):  
        return '$%.1f$x$10^{9}$' % (x/1000000000)  
  
  
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
  
We can see that
* Asia has the largest area, almost 4, 5 times that of the second largest South America.  
* Although Asia is large, the population is less than South America.  
* India, Latin America, and North America have a very small proportion of the population.  
  
我们可以看到  
* 亚洲面积最大，几乎是第二大南美洲的4.5倍  
* 虽然亚洲人口很多，但人口比南美洲少  
* 印度、拉丁美洲和北美的人口比例非常小  
  
### 4.Population density of the world  
### 4.世界人口密度
  
    #Based on the above results, continue to study population density  
    plt.subplots(1,1,figsize=(10,5))  
    plt.bar(x=df2['Pop. Density (per sq. mi.)'].index,height=df2['Pop. Density (per sq. mi.)'])  
    plt.title('Population density of the world',pad=30,fontsize=20)  
    plt.xlabel('Region',labelpad=30,fontsize=15)  
    plt.ylabel('Population density(per sq. mi.)',labelpad=30,fontsize=15)  
    plt.xticks(rotation=90)  
  
![2](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/2Population%20density%20of%20the%20world.png)  
  
* Asia has the highest population density, followed by Western Europe and the third is the Far East, which seems to be inconsistent with the above comparison of population and area.  
*  亚洲的人口密度最高，其次是西欧，第三是远东，这似乎与上述人口和面积的比较不一致  
  
### 5.  Population Composition of the World    
### 5.  世界人口组成    

    #Study the relationship between demographic composition, net population growth rate and mobility  
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
  
The results of this picture are thought-provoking  
* The net growth rate of Africa, America and Australia is high, but the population has an outflow trend.  
* The population situation in Europe is very serious. The population of the Baltic Sea is seriously negatively growing and outflowing.Although Western Europe has a small net growth rate, the migration population is more than the birth population, because of the refugee policy. And the population change in Eastern Europe also tends to be outflowing, and there are no newborn babies.  
* Although India is large, the situation of population outflow is also very serious, and people cannot be retained.  
* The development of Asia is relatively stable, with a considerable population born, and a small number of people moving in.   

这幅图的结果发人深省  
* 非洲、美国、澳大利亚的净增长率较高，但人口有外流趋势。  
* 欧洲的人口情况很严重。波罗的海的人口正在消极地严重增长和外流。虽然西欧的净增长率较小，但由于难民政策，移民人口多于出生人口。东欧的人口变化也趋向于外流，没有新生婴儿。  
* 印度虽然大，但是人口外流的情况也很严重，人不能留。  
* 亚洲的发展相对稳定，有相当数量的人口出生，少量人口迁入。  
  
  
### 6.Population density distribution</font>   
### 6.人口密度分布</font>   

    #Display population density on the map
    fig=plt.figure(figsize=(15,15))  

    map = Basemap(projection='mill',lon_0=180)  

    map.drawcoastlines()  
    map.drawcountries()  
    map.drawparallels(np.arange(-90,90,30),labels=[1,0,0,0])  
    map.drawmeridians(np.arange(map.lonmin,map.lonmax+30,60),labels=[1,0,0,0])  
    #map.drawmapboundary(fill_color='aqua')  
    #map.fillcontinents(color='coral',lake_color='aqua')  


    #The latitude and longitude of the scatter  
    lats=np.array([31.,70.,45.,47.,-10.,-23.,20.,-10.,60.,45.,60.])  
    lons=np.array([114.,20.,50.,255.,290.,134.,20.,26.,110.,14.,30.])  

    size=df2['Pop. Density (per sq. mi.)']/df2['Pop. Density (per sq. mi.)'].max()  
    size=size.values  

    x, y = map(lons,lats)  
    map.scatter(x,y,s=size*500,marker='o',color='g')  

    plt.title('Population density distribution',fontsize=30)  
    plt.show()  

![4](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/4Population%20density%20distribution.png)  
  
### 7.  Per capita GDP of the world</font>  
### 7.  世界人均GDP</font>  

    plt.subplots(1,1,figsize=(10,5))  
    plt.bar(x=df2['GDP ($ per capita)'].index,height=df2['GDP ($ per capita)'],width = 0.5)  
    plt.title('Per capita GDP of the world',pad=20,fontsize=20)  
    plt.xlabel('Region',labelpad=30,fontsize=15)  
    plt.ylabel('GDP ($ per capita)',labelpad=30,fontsize=15)  
    plt.xticks(rotation=90)  
    plt.show()  

![5](https://github.com/chirring/Countries-of-the-world/blob/master/ResultPic/5Per%20capita%20GDP%20of%20the%20world.png)  

* It seems that the richest people are Western Europeans, followed by North Americans.  
* 最富有的人似乎是西欧人，其次是北美人.  
  
  
### 8.Industrial distribution ratio</font>  
### 8.工业分布比例</font>  
  
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
  
We can see that  
* Almost all countries or regions are mainly service industries.  
* The proportion of agriculture in Asia, India and Australia is larger than in other countries or regions.  
* The proportion of industry in North Africa and the Far East is relatively large.  
  
我们可以看到  
* 几乎所有的国家或地区都以服务业为主。  
* 亚洲、印度和澳大利亚的农业比重比其他国家或地区都大。  
* 北非和远东的工业比重比较大。  
  
### 9.Farming situation of the World</font>  
### 9.世界农业状况</font>  
  
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
  
* It is worth noting that although the arable area in Europe is large, the area of crops is very small and the proportion is seriously inappropriate.  
* 值得注意的是，虽然欧洲的耕地面积很大，但作物的面积非常小，比例非常不合适。  
  
   
   
   
   
 ### Part IV: Conclusion  
* East Asia has a large area and a large population density, but the economy is generally.
* Although Europe is small in size and its population density is second only to East Asia, its per capita GDP is almost three times that of East Asia. It also faces the problem of population decline and massive influx of refugees.
* Africa has a large area and a small population density. Agriculture still occupies a large proportion and the national economy is low.
* North America has a small population density, but its economic level is quite developed, mainly relying on the development of service industries.
  
第四部分:结论  
* 东亚地区面积大，人口密度大，但经济总体上比较落后。  
* 虽然欧洲国土面积小，人口密度仅次于东亚，但其人均GDP几乎是东亚的三倍。它还面临着人口下降和难民大量涌入的问题。  
* 非洲面积大，人口密度小。农业仍占较大比重，国民经济水平较低。  
* 北美人口密度小，但经济水平比较发达，主要依靠服务业的发展。  
