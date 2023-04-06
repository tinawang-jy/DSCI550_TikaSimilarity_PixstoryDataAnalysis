# DSCI550_TikaSimilarity_PixstoryDataAnalysis

# Introduction

This is a real analysis exercise on the Newest Social Media Dataset in Pixstory platform. Our group, `team 6`,  added 7 types of features to this dataset: sports events, festival lists, hate tags, sarcasm flags, country boundaries, billboard top songs, and movie ranks. We use Tika conduct the similarity analyses and visualize the results.	

# Installation
Please use requirements.txt to install and import required packages.
```
$ pip install -r requirements.txt
```

# Data 

### Pixstory Data
How to Run Code:
* The main data aggregation scritp is designed based on Python3.9.
* The final tsv dataset can get by running the `Team6_DataAggregation` Notebook through `Cell -- Run All`

#### Joining Required Fearures

>1. Sport Events
Parsed from the link [TopendSports](https://www.topendsports.com/events/games/list.htm) and then use re.package to standardized the date of the sport events, use date.time to match the 4 different types of the date of the sport events with the Account Created Date. 

>2. Film Festivals
The data was parsed from [2022 Film Fest Report](https://www.film-festreport.com/home/film-festivals-2022), [2021 Sreen Daily Report](https://www.screendaily.com/news/2021-film-festivals-and-markets-latest-dates-postponements-and-cancellations/5155284.article), [2020 Sreen Daily Report](https://www.screendaily.com/news/2020-film-festivals-and-markets-latest-dates-postponements-and-cancellations/5155284.article). Then apply re.package to standardized the date of the start day of the festival, end day of the festival,  use date.time to compare the date range of the different types of the festival date with the Account Created Date. If the Account Created Date fits in the date range, then the festival features (festival name, start day, end day) will be added.

>3. Hate Speech Flag
Add a flag for hate speech detected from [Glaad](https://www.glaad.org/hate-speech-listing) and [ADL](https://www.adl.org/resources/hate-symbols/search) in order to get the hate label. Then assign hate speech label based on title and narrative.

>4. Scra Flag
Use 'data/GEN-sarc-notsarc.xlsx' to get the columns of the scarc flags and define the function clean_text to standardized the flag. Then count vectorizer and assign the flags based on title, narrative and text.


#### Addtional Dataset from Different MIME Top-Level 

* 3 MIME Top-Level: `appliaction/zipped-shapefile`, `text/html` and `image/jpeg` 

>1. Shapefile -- Esri Worldwide Country Boundary 
> * External shapefiles datasets is provided in the data folder. Make sure you have all necessary files (not just shp!) and Running under Python 3.8+ version. fiona is an dependency from geopandas. If it can not installed properly with geopandas, please install it. manunally. By assign country for post that mentioned country in the title or narrative. 3 new features are : Country, Continent, and geometry (georeference) 

>2. HTML -- BillBoard 100 Year Top Music for 2020, 2021, and 2022
> * Use beautifulsoup to parse [Official Biilboard Award List](https://www.billboard.com/charts/year-end/hot-100-songs/) and store them as lists. Match the dataset narrative column to see whether they mention the song to add new features. 4 new features are : song title, singer, rank, and the rank year

>3. Image --  Boxoffice for 2020, 2021, and 2022
> * Picking up the top most popular box office movies data in the U.S. between 2020 and 2022 of each year. These data are made from image files and converted into CSV file for analysis. Match the data if "movie title" is included in Pixstory data's "Title" or "Narrative". 4 new features are : movie title, movie rank, rank year, movie release date 

# Assignment1 : Tika Similarity Test and Clustering 

### Tika Similarity Test

##### 1. Convert the TSV dataset into JSON using Tika Similarity’s tsv2json tool
You can get data_json folder that contains all json files by running `team6_tsv_to_json` through `Cell -- Run All`

##### 2. Compare Jaccard similarity, edit-distance, and cosine similarity
If you want to compute each similarity test, you should refer to the code below.

> * Jaccard similarity
``` 
$ python jaccard_similarity.py --inputDir /path/to/files —outCSV /path/to/output.csv
```
> * edit-distance
```
$ python edit-value-similarity.py --inputDir /path/to/files --outCSV /path/to/output.csv --accept png pdf gif
```
> * Cosine similarity
```
$ python cosine_similarity.py [-h] --inputDir INPUTDIR --outCSV OUTCSV [--accept [png pdf etc...]]
```

##### 3. Compare and contrast clusters from each test
We generated d3 cluster, d3 circle, and d3 level clusters. If you want to run the code, refer to the code below. 

> * Generate d3 clusters
```
$ python edit-cosine-cluster.py --inputCSV /edit/cosine/jaccard/similarity/scores.csv  --cluster 2
```

> * Generate d3 circle
```
$ python edit-cosine-circle-packing.py --inputCSV /edit/cosine/jaccard/similarity/scores.csv  --cluster 2
```

> * Generate d3 LevelClusters
```
$ python generateLevelCluster.py
```

##### 5. Add some new D3.js visualizations to Tika Similarity
Add some new D3.js visualizations to Tika Similarity. Our team made circle packing, treemap, plot, bracemap. We are added the result and code file with in our submitted file. 

# Assignment2 : Content Analysis on manipulated Pixstory datasets based on Assignment1

## Introduction

This project leverages the richness in the Pixstory dataset by exploring the posts (text) and images (jpg). The content analysis can be devided into two parts. The first part follows the workflows of language identification, language translation, Geo-parsing and toxicity detection. The second part focuses on Image Captioning and Object Recognition.

## Dependencies
A Docker is needed to call Tika servers.
Please use requirements.txt to install and import required packages.
```
$ pip install -r requirements.txt
```

## Text Analysis

### 1. Language Detection using Google's LangDetect and Tika Language Detection
|Language Detection| Google's LangDetect  | Tika Language Detection |
|------------------| ------------- | ------------- |
|Number of Languages Detected| 31 | 53  |
| Top2 Languages  | English, Italian  | English, Italian |

### 2. Language Translation using RTG (Reader Translator Generator)
The command lines to start Tika servers are below.
```
$ IMAGE=tgowda/rtg-model:500toEng-v1
$ docker run --rm -i -p 6060:6060 $IMAGE
$ docker pull apache/tika
$ docker run -d -p 9998:9998 apache/tika:latest
```

### 3. Geo-parsing using GeoTopicParser

### 4. Toxicity Detection using Detoxify

## Image Analysis

### 1.  Image Captioning and Object Recognition using Tika Image Dockers


# About
This is the assignments from DSCI 550 Spring 2023 at USC Viterbi School of Engineering. This research is collaborated by 6 group members

Team members: Andrew Bruneel, Arya Sun, Bongjun Kim, Jiayin Wang, Jingyi Wang, Tongxin Ye
