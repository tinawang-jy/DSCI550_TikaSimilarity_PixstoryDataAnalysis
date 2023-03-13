# DSCI550_TikaSimilarity_PixstoryDataAnalysis
This repository is designed as a collaborate workspace for DSCI550_SP23_team6. 
Thanks for the instruction from Professor Chris Mattmann and TA Suchith Prathapaneni.

# Introduction

* Research Topic: 
This is a real analysis exercise on the Newest Social Media Dataset in Pixstory platform. Our group adds 7 types of features to this dataset: sports events, festival lists, hate tags, sarcasm, billboard top songs, best book,and country. Then we use Tika to conduct similarity analysis and visualize the results.	

# Installation
Please use requirements.txt to install and import required packages.
```
$ pip install -r requirements.txt
```

# Dependencies & Running Code

How to Run Code:
> The main program is designed based on Python3.9. For scripts requires different working enviornment will mention in the section. 


>Dependency libraries:
>1. beautifulsoup      - 4.11.1
>2. csvkit             - 1.1.0
>3. geopandas          - 0.12.2
>4. glob               - 2.0.7
>5. json               - 0.9.6
>6. matplotlib         - 3.5.1
>7. nltk               - 3.7
>8. numpy              - 1.21.5
>9. opencv-python      - 4.6.0.66
>10. pandas            - 1.4.2
>11. pillow            - 9.0.1
>12. pytesseract       - 0.3.10
>13. python-dateutil   - 2.8.2
>14. regex             - 2022.3.15
>15. requests.         - 2.27.1
>16. requests-file     - 1.5.1
>17. requests-oauthlib - 1.3.0
>18. scikit-image      - 0.19.2
>19. scikit-learn      - 1.1.2
>20. scikit-multilearn - 0.2.0
>21. scipy             - 1.7.3
>22. seaborn           - 0.11.2
>23. tika              - 2.6.0



# Data 

### Pixstory Data

#### Joining Required Fearures
Please run all cells in the 

>1. Sport Events
parse from the link https://www.topendsports.com/events/games/list.htm and then use re.package to standardized the date of the sport events, use date.time to match the 4 different types of the date of the sport events with the Account Created Date. 

>2. Film Festivals
parse from the 3 links: https://www.film-festreport.com/home/film-festivals-2022, https://www.screendaily.com/news/2021-film-festivals-and-markets-latest-dates-postponements-and-cancellations/5155284.article , https://www.screendaily.com/news/2020-film-festivals-and-markets-latest-dates-postponements-and-cancellations/5155284.article.
Then use  re.package to standardized the date of the start day of the festival, end day of the festival,  use date.time to compare the date range of the different types of the festival date with the Account Created Date. If the Account Created Date fits in the date range, then the festival features (festival name, start day, end day) will be added.

>3. Hate Speech Flag
Add a flag for hate speech detected from Glaad and ADL https://www.adl.org/resources/hate-symbols/search in order to get the hate label. Define a function checkString to check wheather there are English characters and digits in narrative and title. Then assign hate speech label based on title and narrative.

>4. Scra Flg
Use 'data/GEN-sarc-notsarc.xlsx' to get the columns of the scarc flags and define the function clean_text to standardized the flag. Then count vectorizer and assign the flags based on title, narrative and text.


## Addtional Dataset from Different MIME Top-Level [Data Aggregation]
3 MIME Top-Level are shapefile,  Text/ HTML and Application/ JSON. Please run all blocks in the 

>1. Shapefile -- Esri Worldwise country boundry 
External shapefiles datasets are in path "data/6-1_World_Countries/World_Countries.shp" and "data/6-1_World_Countries_(Generalized)/World_Countries__Generalized_.shp". Use geopandas package and Make sure you have all necessary files (not just shp!). Assign country for post that mentioned country in the title or narrative. 3 new features are : Country,CONTINENT,geometry(which means detailed location based on the geomap)

>2. HTML -- BillBoard 100 Year Top Music for 2020, 2021, and 2022
Use beautifulsoup to parse the link https://www.billboard.com/charts/year-end/hot-100-songs/ then store into list. Match the dataset narrative column to see whether they mention the song to add new features. 3 new features are : song title, singer, rank

>3. Image --  TOP 20 boxoffice for 2020, 2021, and 2022
Picking up the top 20 most popular box office movies data in the U.S. between 2020 and 2022 of each year. These data are made from image files. Match the data if "movie title" is included in Pixstory data's "Title" or "Narrative". 3 new features are : rank, year, release 

# Assignment1 : Tika Similarity Test and Clustering 
>1. Data Aggregation : convert dataframe to tsv file
```
$ pix_df.to_csv('Team6_DSCI550_HW_BIGDATA_0312.tsv', sep='\t', index=False)
```
>Explanation : 
>Once all of our external data had been added to our table, we had to combine all of our work. This was done with commands being run to combine our csv files into a single .tsv.

>2. Tika Similarity Test

>a. Convert the TSV dataset into JSON using Tika Similarity’s tsv2json tool

```
$ filename = 'Team6_DSCI550_HW_BIGDATA_0312.tsv'
$ df = pd.read_csv(filename, delimiter='\t')
$ df_without_nan = df.dropna(how='all')
$ if not os.path.exists('data'):
$    os.makedirs('data')
$ for i in range(len(df_without_nan)):
$    df.iloc[i].to_json(f'data/{i}.json')
```

> Explanation : 
> reads in a Pandas DataFrame called df, drops any rows that contain only NaN values using the dropna() method, and then saves each row of the resulting DataFrame as a separate JSON file in a new folder called data.

>b. Compare Jaccard similarity, edit-distance, and cosine similarity
> Jaccard similarity
If you want to compute Jaccard similarity, you should write the code below.

``` 
$ 
```
If you want to compute edit-distance, you should write the code below.

> edit-distance
```
$ python edit-value-similarity.py --inputDir /path/to/files --outCSV /path/to/output.csv --accept png pdf gif
```
If you want to compute Cosine similarity, you should write the code below
```
$ python cosine_similarity.py [-h] --inputDir INPUTDIR --outCSV OUTCSV [--accept [png pdf etc...]]
```

>c. Compare and contrast clusters from Jaccard, Cosine Distance, and Edit Similarity


>3. Package data up by combining all of your new JSONs with additional features into a single TSV

# Assignment2 : Add some new D3.js visualizations to Tika Similarity
```
$ python processing.py
```

# Contribution
> Our team has six members, and each contribution is as follows.
> Andrew Bruneel :
> Arya Sun :
> Bongjun Kim : 
> Jiayin Wang :
> Jingyi Wang :
> Tongxin Ye : 
