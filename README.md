# Coursera Capstone: Helsinki house prices
![map with all stuff](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Choropleth%20map%20of%20Helsinki%2C%20with%20HSL%2Bprices%2Bterminals%2Brecommendations.PNG?raw=true)
The capstone project for the IBM Data Science Coursera course.\
Markus Taskila\
06.07.2021

### Description
This project will look into what are possible factors for the differing housing prices in Helsinki area depending on location. We will use FourSquare data to see if the nearby venues or the amount of venues is the main factor for increased housing prices. We will also look into government provided demographic data on Helsinki area postal code areas, as well as how big of a factor public transportation plays in the pricing. In the end suggestions for interesting housing opportunities are given.\ 

### Preface
The main reason for selecting this topic is that I wanted to create something that would help me and my friends to find better deals in the housing market. I hoped that with this project I could gather bigger data and bigger insights than is available through manually looking at houses and listings. So in essence I think this project was perfect started to get into data science, as I saw the both the good and the bad, desperately scoundreling the internet for solutions and better data and then feeling the greatest sence of accomplisment the next moment.\
![map with all stuff](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/wasted%2010h%20work.PNG?raw=true)
*10-20 h of work was replaced by a method that took 30min in QGIS*\

I hope that this project will give valuable insights to people interested in buying a used home in Helsinki area or even give inspiration for new ideas for your own project. 

### The problem
1. Housing prices are drastically increasing in Helsinki and there seems to be no sentiment remaining on what is good and which is bad investment location
2. There are not many property investemnt tools for Helsinki area that are free to use, visual and easy to consume

### The main goal of this report is to: 
1. Find out which location factors play the biggest role in determining housing prices
2. Are there postal code areas that have the decired attributes of a good investment, but have belove average pricing thus being an outlier and a worthy investment subject

### Hypothesis
Before setting on this journey to discover the variables responsible for deciding housing pricess, I had many interesting ideas what could be true but was not sure. I believed that living near public transportation increases price, living near services increase price, and both of these are mostly correct. There were many findings that really surprised me and I partly accidentally found out that for example that the best demographic indicator from my dataset was the educational background. 

### Process
Before starting to do anything I though longer than I usually stay still thinking and tried my best to have a solid plan. Later I found out that plan is good, but there will be blood, lots of changes and unseen problems. This is my plan that I tried my best to follow: 
1. Download and import necessary libraries
2. Retrieve, clean and combine needed data
3. Use Foursquare API to retrieve further data for our analysis
4.  a. Create Choropleth map to the visualize the price differences between areas\
    b. Create another Cloropleth map that includes train station locations and HSL Zones
5.  a. Use K-Means clustering with location price data, distance from train station and HSL zone\
    b. Use K-Means clustering with data retrieved from Foursquare\
    c. Use K-Means clustering with both the public transportation data and Foursquare data
6. Analyze the data

I found out that it's nearly impossible, atleast for a newbie, to stick to this kind of process. While analysing, studying and reflecting new, better and exciting ideas pop into once head and its easy to go from one thing to another and experiment. Due to my lack of experience with handling a project, this capstone blew up from 20 hours project to a 100-150 hour Godzilla of a project. Lots of time went fixing data, having dead ends, and even learning new software to get over a hurdle. I now understand the saying that 80% of data scientists time goes to data preprocessing. Nothing works if the data is not in the correct form.\

So in reality the actual process was not as clean as on the paper. I gathered data, processed the data, anaylized, found better data and repeated the process.\
\
##### In the final version, the following data was used:
* Greater Helsinki old housing sales/price data from 2010 to 2020
  * https://tilastokeskus.fi/til/ashi/index.html
* Postal area demographic data (Paavo database) (I got the data using QGIS and WFS directly from Stat)
  * https://pxnet2.stat.fi/PXWeb/pxweb/fi/Postinumeroalueittainen_avoin_tieto
* Public transportation data (HSL), areas, terminals
  * https://hri.fi/data/en_GB/dataset/hsl-n-terminaalit
  * https://hri.fi/data/en_GB/dataset/hsl-n-taksavyohykkeet

QGIS software was a big help in solving this project. Most of the finnish datasets had coordinates in EUREF-FIN coordinate form and most of free coordinate conversion tools didin't allow large amounts of conversions to the coordinate system that most of Python's geo libraries use. The struggle led me to find QGIS where I found out about WMS and WFS and that my favorite data source stat.fi had an open WFS service. Using QGIS I could get the datasets with geographic features already attached to the data and QGIS could convert the coordinates while exporting. \\
Never the less, there was still lots of data wrangling, which is to be expected. \

I used Foursquare to get the local venue data for each postal code area in Helsinki metropolitan area. After exploring the data I found out that there was not nearly enough data to have any precises or even directional insights based on the data. Most postal codes had only one or two entries and those two entries were usually 'Bus stop' or 'landscaping area'.\
##### Belowe are the clusters that K-means managed to create based on the Foursquare data:
![cluster 0](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/cluster%200.PNG?raw=true)
![cluster 1](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/cluster%201.PNG?raw=true)
![cluster 2](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/cluster%202.PNG?raw=true)
![cluster 3](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/cluster%203.PNG?raw=true)
![cluster 4](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/cluster%204.PNG?raw=true)
From these clusters you can see that only cluster 4 has some sensical insights. I decided early that it was not smart to continue analysing or changing the K-means in order to get better results, as there was simply not enough data from Four square.\

After Foursquare API experiement I moved on to analyze the demographic data. 

### Analysis
I revisited the analysis part many time after moving on because of unsatisfactory results. My analysis was based on finding the outliers of the data. If the data said that the more higher university degree population there is in an area, the higher the prices of houses, I would find the place that had lots people with higher level university degree and where the house prices did not catch up yet. These areas, I thought, would be a great place to start looking for investment property. \

In my first attempt I focused on the age structures of areas as the independent variable for determining tha housing prices. From the basic Pearson correlation analysis df.corr() the variable seemed interesting. 
##### Correlation between age structure of postal code area and prices
![age dataframe corr](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/price%20and%20age%20correlation%20table.PNG?raw=true)
![age correlation plot](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Correlation%20of%20price%20and%20age%20of%20population.PNG?raw=true)
The housing prices were changing depending on the age group. Areas with lots of teenagers and older parents had negative correlation, mostly due to larger family sizes and thus requirement for bigger apartments that have smaller price per square meter. The age groups from 25-34 had a strong positive correlation, which can be explained by higher amount of income and smaller size of families.\

When I further looked into the data as the independent variable, I found out that there was nearly not enough causation to keep the variable as a determening factor. Below is the correlation of year over year growth of the different age segments compared to the square meter prices of sold houses. As you can see, the accuracy and the correlation are not nearly good enough to base any big decisions on. 
![age growth correlation](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Correlation%20of%20price%20and%20growth%20of%20age%20groupPNG.PNG?raw=true)


#### Correlation between education structure of postal code area and prices
I decided to move on to look at the educational background of the inhabitants of different postal code areas. In the basic Pearson correlation analyis I could see that the educational background seem very promising to be the new star of the project. 
![education dataframe corr](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/price%20and%20education%20correlation%20table.PNG?raw=true)
As seen on the data, some of the independent variables had an correlation of almost 90% to price, while other had negative correlation of less than -80%. These correlations took me by surprice and I decided to move forward with the analysis using education structure as the base.\

In the scatter plot one can see that the data is tightly packed around the center line and move with the increase and decrease of price in correlation. In the plots you can see some data points out of the pack, and these are the outliers that I began hunting.
![education correlation plot](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Correlation%20of%20price%20and%20education%20level.PNG?raw=true)

##### Finding outliers from the scatter plots
My method of finding the outliers was based on the visual plots pointing me towards where to aim with df.loc[]. 
For the plots that had negative correlation, I picked out the data points that were weak in the independent variable and strong in the dependent varible. For the positive correlation plots the aim was the opposite. To have as strong as possible independent variable and dependent variable as low as possible. So basically the datapoints should always be below the line but towards left or right depending on if the correlation is negative or positive. 
![edu](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Population%20with%20education%20outliers.PNG?raw=true)
From the graph with correlation between the amount of people with education and price, I chose the datapoints that are circled. 

![basic edu](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Population%20with%20basic%20level%20educatio.PNG?raw=true)
From the graph with correlation between the amount of people with basic level education and price, I chose the datapoints that are circled. 

![high school](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Population%20with%20high%20school%20level%20educatio.PNG?raw=true)
From the graph with correlation between the amount of people with high school education and price, I chose the datapoints that are circled. 

![vocational school](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Population%20with%20vocational%20education%20outliers.PNG?raw=true)
From the graph with correlation between the amount of people with vocational school education and price, I chose the datapoints that are circled. 

![lower uni](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Population%20with%20lower%20uni%20level%20education.PNG?raw=true)
From the graph with correlation between the amount of people with lower university education and price, I chose the datapoints that are circled. 

![higher uni](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Population%20with%20higher%20uni%20level%20education.PNG?raw=true)
From the graph with correlation between the amount of people with higher university education and price, I chose the datapoints that are circled.\

In total I was able to get 22 potential postal code areas using this method, of which 2 had duplicate hits. The final step was to plot everything on folium map using layers. 

### The map

**Please go to the below link to play with the map that is located in the bottom of the notebook.**
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1QIIkUCcDntFXwzs32xIfs57kbbeaKcSn?usp=sharing)

In the folium map four layers were added on top of the base layer.\
Layers included: 
1. HSL public transportation price zones (Zones in blues)
2. 2019 €/m2 prices (Zones from orange to red)
3. Outliers layer from the educational background analysis (Zones in purple)
4. Public transportation terminals (T marks on the map represent terminal, you can click to see info) 

You can turn layers on and off by pressing the layer control button on the top right of the map. 

![map with all stuff](https://github.com/Madfuuu/Coursera_Capstone/blob/main/Colab%20Screenshots/Choropleth%20map%20of%20Helsinki%2C%20with%20prices%2B%20recommendations.PNG?raw=true)

### Conclusion
Based on the analysis and the visual representation given by the folium map, these are the conclusion of this project. \
I would personally use the outliers areas as my first place to look when thinking about investing in property. There is clear correlation with the educational background and price and nearly no outliers in the extremes of the x-axis. There are some conclusions that every reader can make by themselves of if the educational structure correlates to other preferrable or undesirable attributes of housing locations. For example safety, convenience, clenliness etc. and these are definitely a topic that require furter research.\ 
The age structure was not as good of a predictor for house prices due to the more complicated relationship that age has towards house investing.\
In the map one can see that the closer towards Helsinki center and the more nearer towards train lines, the higher the average prices of houses were. This was the main hypothesis I had coming into this study, and it is not surprisingly correct. 

### Further research ideas
Based on my findings, more research on the effecto of educational structure towards other dependend variables would be interesting. Also the map could be recreated with time slider option for viewing data from many years in consecutive sequence and to see which areas have the biggest changes happening. The trends of the variables could also be tracked more carefully and how they affect the housing prices. Does for example sudden increase in inhabitants with higher university degree increase the prices of the area in the same sudden matter. 

# This concludes todays capstone project. Stay tuned for more data science and python related projects.
# Big thank you for being interested! 
