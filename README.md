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

This project leverages the richness in the Pixstory dataset by exploring the posts (text) and images (jpeg&png). The content analysis can be devided into two parts. The first part follows the workflows of language identification, language translation, Geo-parsing and toxicity detection. The second part focuses on Image Captioning and Object Recognition.

## Dependencies

A Docker is needed to call Tika servers.
Please use requirements.txt to install and import required packages.
```
$ pip install -r requirements.txt
```

## Text Analysis

### 1. Language Detection using Google's LangDetect and Tika Language Detection
How to Run Code:
* The language identification Notebook script is designed based on Python3.9, all results can be compiled through `Cell -- Run All`

|Language Detection| Google's LangDetect  | Tika Language Detection |
|------------------| ------------- | ------------- |
|Number of Languages Detected| 30 | 53  |
| Top2 Languages  | English, Italian  | English, Italian |


### 2. Language Translation using RTG (Reader Translator Generator)

The command lines to start Tika servers are below.
```
$ IMAGE=tgowda/rtg-model:500toEng-v1
$ docker run --rm -i -p 6060:6060 $IMAGE
$ docker pull apache/tika
$ docker run -d -p 9998:9998 apache/tika:latest
```
#### Use GPU3080 to accelerate translation speed
* 1. When deploying the rtg translation package using docker, pull the latest image v0.7.2-600toEng-v2.0, and deploy the service using the following command:
```
$ IMAGE=tgowda/ v0.7.2-600toEng-v2.0
$ docker run --gpus '"device=0"' --rm -i -p 6060:6060 $IMAGE
```
* 2. When requesting using `http://localhost:6060` or `http://localhost:6060/translate`, an error occurred. After troubleshooting, it was found that the current version of PyTorch in the image is not compatible with the graphics card version. The error message is as follows:
`GeForce RTX 3080 with CUDA capability sm_86 is not compatible with the current PyTorch installation. The current PyTorch install supports CUDA capabilities sm_37 sm_50 sm_60 sm_61 sm_70 sm_75 compute_37.`
* 3. To solve this problem, first enter the container launched in step 1), and then upgrade torch as follows:
Open a terminal and cd to folder 'research' containing geoparsers.

![3080](https://github.com/Tilalala/DSCI550_TikaSimilarity_PixstoryDataAnalysis/blob/main/3080.png)

* 4. After the upgrade is complete, save it as a new image from the container, as follows:
`docker commit d5944567401a tree/ v0.7.2-600toEng-v2.0-3080`
* 5. Then, redeploy the rtg service on the new image:
`IMAGE= tree/ v0.7.2-600toEng-v2.0-3080`
`docker run --gpus '"device=0"' --rm -i -p 6060:6060 $IMAGE`
This way, rtg service can be used on 3080. The translation speed is tested to be about 3-4 times faster than using CPU.
#### Use multithreading to speed up translation

* Using multithreading in Python is relatively simple, as follows:
`thread.start_new_thread ( function, args[, kwargs] )`

* Parameter explanation:
>1. function - the thread function
>2. args - the arguments passed to the thread function, which must be a tuple
>3. kwargs - optional parameters

* Using multithreading can start multiple translation tasks at the same time, and synchronize the processing of translated text in blocks. It is only necessary to ensure that each thread writes data to different documents separately when writing, in order to ensure thread safety.

* Tests using this method showed that the translation speed is comparable to that of single-threaded processing. The reason for this is that the current bottleneck in translation speed is the model processing speed (CPU or GPU) rather than the limit of request concurrency. Therefore, the acceleration effect is not significant.

### 3. Geo-parsing using GeoTopicParser
```
$ cd research
$ cd lucene-geo-gazetter
$ export PATH=$PWD/src/main/bin:$PATH
$ lucene-geo-gazetter -server
```
Open a new terminal tab
```
$ cd ..
$ ./geotopic-server
```
`Geographical Information' column saved null geoparsing result to 'Null' in string format.`


### 4. Toxicity Detection using Detoxify

A set of open-source models from Detoxify library is utilized to detect and evaluate the level of toxicity in posts in 7 aspects: toxicity, severe_toxicity, obscene, identity_attack, insult, threat, and sexual_explicit. Based on the average toxicity scores and corresponding standard deviations of the pixtory dataset, we can conclude that pixtory lives up to its claim of being a clean social media with about 75% of its narratives getting scores under 0.01 in terms of all kinds of toxic expressions.

![image](https://user-images.githubusercontent.com/111823275/230514951-509aaa50-651a-48ff-9b63-21aedc01b1ca.png)

## Image Analysis

### 1.  Image Captioning using Tika Image Dockers

Do not build the docker environment, instead automatically pull it by running:

```
$ docker run -it -p 8764:8764 uscdatascience/im2txt-rest-tika
```

Overall, the model was precise in generating captions and ran fairly quickly on the images in the dataset. The inaccuracies that are seen with the generated captions are likely due to the wide variety of images within the Pixstory dataset, and the limited vocabulary of the image captioning model to be able to cover the entire breadth of these images. 

### 2. Object Recognition using Tika Image Dockers

Delete all containers and images on your docker environment from image captioning (to ensure it pulls the necessary environment):

```
$ docker run -it -p 8764:8764 uscdatascience/inception-rest-tika
```

The detected objects were very off-base at times from what the actual pictured objects, but under some conditions there exists consistency between the detected object and automatically-generated caption. The identified objects are not usually described in the caption, but sometimes there was overlap and both models performed very well. 

# Assignment3 : Data visualization on multimodal Pixstory dataset

## D3.js Visualization 
* The command lines to call the server which can visualize our 5 ds.js html file are below.
```
$ cd /Users/mac/D3js
$ python -m http.server 8000
```
* Open the browser and then direct to the url:
```
http://localhost:8000/
```
* Open our 5 visualization html files

1. entertainment-calendar
open the `calendar` folder under the `http://localhost:8000/`

then open the `entertainment-calendar` folder under it, and then click on the `calendar.html`

2. hazard_location_bubble
open the `hazard_location_bubble` folder under the `http://localhost:8000/`

then click on the `hazard_location_BubbleChart.html`

3. interest_age bar
open the `interest_age` folder under the `http://localhost:8000/`

then open the `interest_age` folder under it, and then click on the `index.html`

4. language_location_count_map

open the `language_location_count_map` folder under the `http://localhost:8000/`

then then click on the `origin_language_locationmention_map.html`

5.Time_Age_Post Interactive Line

open the `age_time_postdata` folder under the `http://localhost:8000/`

then click on the `agetime.html`

* Create visualization webpage throught GitHub

>1. Create a new Github Repository with all D3 visualization file in the root directory 
>2. Write `index.html` for webpage design and place it in the `master` directory
>3. Build up the Page deployment under repository setting: set the Page Sources to `master` directory 

[Click Here](https://tinawang-jy.github.io/Pixstroy-D3-Visualization/) for the Visualization Site

## Data Ingestion with Elasticsearch

We have created 5 JSON files through the process of D3.js Visualization.

(age_analysis_jsondata, hazard_score&location.json, language&location.json, Region_Interest.json, sports_event_analysis_jsondata.json)

To ingest these 5 data files into Elasticsearch, we need to perform the following steps for each of the data files:

1. Download and install Elasticsearch

Elasticsearch v8.7.0 can be downloaded and installed as follows

```
curl -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.7.0-darwin-x86_64.tar.gz
curl https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.7.0-darwin-x86_64.tar.gz.sha512 | shasum -a 512 -c - 
tar -xzf elasticsearch-8.7.0-darwin-x86_64.tar.gz
cd elasticsearch-8.7.0/ 
```
2. Read data from a JSON file and assign a unique ID to each document

3. Convert data to Elasticsearch bulk format and send the formatted data to Elasticsearch

```
curl -XPOST 'http://localhost:9200/_bulk' -H 'Content-Type: application/json' -d @/path/to/bulk_data_file
```

## MEMEX GeoParser

* The command lines to pull MEMEX GeoParser are below.
```
$ git clone https://github.com/nasa-jpl-memex/GeoParser.git
$ cd /Users/mac/Documents/GitHub/GeoParser/Docker
$ docker pull nasajplmemex/geo-parser
$ docker-compose up -d
```

* The command lines to upload data to MEMEX GeoParser are below.
Assuming that you have checked out GeoParser to $geodata, then:
```
$ cd /geodata
$ ./create-core.sh
$ ./add-fields.sh
```

* loadgeodata.ipynb Run all

* Display in local server
1. Open http://localhost:8000 in browser.
2. Set indexed engine path 'http://localhost:8983/solr/geodata/' and add index.
3. Click on 'Geoparse', wait for a while then click 'View'.

# About
This is the assignments from DSCI 550 Spring 2023 at USC Viterbi School of Engineering. This research is collaborated by 6 group members

Team members: Andrew Bruneel, Arya Sun, Bongjun Kim, Jiayin Wang, Jingyi Wang, Tongxin Ye
