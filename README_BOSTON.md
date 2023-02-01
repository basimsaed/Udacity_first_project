---
jupyter:
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
  language_info:
    codemirror_mode:
      name: ipython
      version: 3
    file_extension: .py
    mimetype: text/x-python
    name: python
    nbconvert_exporter: python
    pygments_lexer: ipython3
    version: 3.8.8
  nbformat: 4
  nbformat_minor: 5
---

<div class="cell markdown">

# Boston AirBnb README

</div>

<div class="cell markdown">

# Busines understanding

</div>

<div class="cell markdown">

In this data set we are going to explore offerings in Boston since
visitors who use AirBnb to travel to there, We have list of stays places
form AirBnb descripes the offers from different hotels or motels or even
public houses the data set countain three diffrent files :

    - calendar : which is countain listing id and the price and availability for that day.
    - listings : which has full descriptions and average review score.
    - reviews : including unique id for each reviewer and detailed comments.

AS for our analysis we are going to use :

    - Calendar
    - listings

To answer and detact the three flowing questions:

    - what is the most busys month in terms of availbility of offeres ?
    - which neighborhood is the most expensive\cheapest throw the offeres we have in Boston?
    - when is the highest average price of offers per night ?

</div>

<div class="cell code" execution_count="1">

``` python
#First importing necessary library for data processing and data visualization
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

</div>

<div class="cell code" execution_count="2">

``` python
#Display settings
pd.set_option('display.max_columns',None)
pd.set_option('display.max_rows',None)
```

</div>

<div class="cell code" execution_count="3">

``` python
#Dataset importing
dfC=pd.read_csv('calendar.csv')
dfL=pd.read_csv('listings.csv')
dfR=pd.read_csv('reviews.csv')
```

</div>

<div class="cell code" execution_count="4">

``` python
#Explore Calendar dataset
dfC.head()
```

<div class="output execute_result" execution_count="4">

       listing_id        date available price
    0    12147973  2017-09-05         f   NaN
    1    12147973  2017-09-04         f   NaN
    2    12147973  2017-09-03         f   NaN
    3    12147973  2017-09-02         f   NaN
    4    12147973  2017-09-01         f   NaN

</div>

</div>

<div class="cell code" execution_count="5">

``` python
print('The number of columns {} and the number of rows are {}'.format(dfC.shape[1],dfC.shape[0]))
```

<div class="output stream stdout">

    The number of columns 4 and the number of rows are 1308890

</div>

</div>

<div class="cell code" execution_count="6">

``` python
dfC.info()
```

<div class="output stream stdout">

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 1308890 entries, 0 to 1308889
    Data columns (total 4 columns):
     #   Column      Non-Null Count    Dtype 
    ---  ------      --------------    ----- 
     0   listing_id  1308890 non-null  int64 
     1   date        1308890 non-null  object
     2   available   1308890 non-null  object
     3   price       643037 non-null   object
    dtypes: int64(1), object(3)
    memory usage: 39.9+ MB

</div>

</div>

<div class="cell markdown">

# The price column need to be change in data type and removing the $ sign from it

</div>

<div class="cell code" execution_count="6">

``` python
#Explore Listings dataset
dfL.head()
```

<div class="output execute_result" execution_count="6">

             id                            listing_url       scrape_id  \
    0  12147973  https://www.airbnb.com/rooms/12147973  20160906204935   
    1   3075044   https://www.airbnb.com/rooms/3075044  20160906204935   
    2      6976      https://www.airbnb.com/rooms/6976  20160906204935   
    3   1436513   https://www.airbnb.com/rooms/1436513  20160906204935   
    4   7651065   https://www.airbnb.com/rooms/7651065  20160906204935   

      last_scraped                                           name  \
    0   2016-09-07                     Sunny Bungalow in the City   
    1   2016-09-07              Charming room in pet friendly apt   
    2   2016-09-07               Mexican Folk Art Haven in Boston   
    3   2016-09-07  Spacious Sunny Bedroom Suite in Historic Home   
    4   2016-09-07                            Come Home to Boston   

                                                 summary  \
    0  Cozy, sunny, family home.  Master bedroom high...   
    1  Charming and quiet room in a second floor 1910...   
    2  Come stay with a friendly, middle-aged guy in ...   
    3  Come experience the comforts of home away from...   
    4  My comfy, clean and relaxing home is one block...   

                                                   space  \
    0  The house has an open and cozy feel at the sam...   
    1  Small but cozy and quite room with a full size...   
    2  Come stay with a friendly, middle-aged guy in ...   
    3  Most places you find in Boston are small howev...   
    4  Clean, attractive, private room, one block fro...   

                                             description experiences_offered  \
    0  Cozy, sunny, family home.  Master bedroom high...                none   
    1  Charming and quiet room in a second floor 1910...                none   
    2  Come stay with a friendly, middle-aged guy in ...                none   
    3  Come experience the comforts of home away from...                none   
    4  My comfy, clean and relaxing home is one block...                none   

                                   neighborhood_overview  \
    0  Roslindale is quiet, convenient and friendly. ...   
    1  The room is in Roslindale, a diverse and prima...   
    2  The LOCATION: Roslindale is a safe and diverse...   
    3  Roslindale is a lovely little neighborhood loc...   
    4  I love the proximity to downtown, the neighbor...   

                                                   notes  \
    0                                                NaN   
    1  If you don't have a US cell phone, you can tex...   
    2  I am in a scenic part of Boston with a couple ...   
    3  Please be mindful of the property as it is old...   
    4  I have one roommate who lives on the lower lev...   

                                                 transit  \
    0  The bus stop is 2 blocks away, and frequent. B...   
    1  Plenty of safe street parking. Bus stops a few...   
    2  PUBLIC TRANSPORTATION: From the house, quick p...   
    3  There are buses that stop right in front of th...   
    4  From Logan Airport  and South Station you have...   

                                                  access  \
    0  You will have access to 2 bedrooms, a living r...   
    1  Apt has one more bedroom (which I use) and lar...   
    2  I am living in the apartment during your stay,...   
    3  The basement has a washer dryer and gym area. ...   
    4  You will have access to the front and side por...   

                                             interaction  \
    0                                                NaN   
    1  If I am at home, I am likely working in my hom...   
    2  ABOUT ME: I'm a laid-back, friendly, unmarried...   
    3  We do live in the house therefore might be som...   
    4  I love my city and really enjoy sharing it wit...   

                                             house_rules  \
    0  Clean up and treat the home the way you'd like...   
    1  Pet friendly but please confirm with me if the...   
    2  I encourage you to use my kitchen, cooking and...   
    3  - The bathroom and house are shared so please ...   
    4  Please no smoking in the house, porch or on th...   

                                           thumbnail_url  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...   
    1  https://a1.muscache.com/im/pictures/39327812/d...   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...   
    3  https://a2.muscache.com/im/pictures/39764190-1...   
    4  https://a1.muscache.com/im/pictures/97154760/8...   

                                              medium_url  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...   
    1  https://a1.muscache.com/im/pictures/39327812/d...   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...   
    3  https://a2.muscache.com/im/pictures/39764190-1...   
    4  https://a1.muscache.com/im/pictures/97154760/8...   

                                             picture_url  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...   
    1  https://a1.muscache.com/im/pictures/39327812/d...   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...   
    3  https://a2.muscache.com/im/pictures/39764190-1...   
    4  https://a1.muscache.com/im/pictures/97154760/8...   

                                          xl_picture_url   host_id  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...  31303940   
    1  https://a1.muscache.com/im/pictures/39327812/d...   2572247   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...     16701   
    3  https://a2.muscache.com/im/pictures/39764190-1...   6031442   
    4  https://a1.muscache.com/im/pictures/97154760/8...  15396970   

                                         host_url host_name  host_since  \
    0  https://www.airbnb.com/users/show/31303940  Virginia  2015-04-15   
    1   https://www.airbnb.com/users/show/2572247    Andrea  2012-06-07   
    2     https://www.airbnb.com/users/show/16701      Phil  2009-05-11   
    3   https://www.airbnb.com/users/show/6031442    Meghna  2013-04-21   
    4  https://www.airbnb.com/users/show/15396970     Linda  2014-05-11   

                              host_location  \
    0  Boston, Massachusetts, United States   
    1  Boston, Massachusetts, United States   
    2  Boston, Massachusetts, United States   
    3  Boston, Massachusetts, United States   
    4  Boston, Massachusetts, United States   

                                              host_about  host_response_time  \
    0  We are country and city connecting in our deck...                 NaN   
    1  I live in Boston and I like to travel and have...      within an hour   
    2  I am a middle-aged, single male with a wide ra...  within a few hours   
    3  My husband and I live on the property.  He’s a...  within a few hours   
    4  I work full time for a public school district....      within an hour   

      host_response_rate host_acceptance_rate host_is_superhost  \
    0                NaN                  NaN                 f   
    1               100%                 100%                 f   
    2               100%                  88%                 t   
    3               100%                  50%                 f   
    4               100%                 100%                 t   

                                      host_thumbnail_url  \
    0  https://a2.muscache.com/im/pictures/5936fef0-b...   
    1  https://a2.muscache.com/im/users/2572247/profi...   
    2  https://a2.muscache.com/im/users/16701/profile...   
    3  https://a2.muscache.com/im/pictures/5d430cde-7...   
    4  https://a0.muscache.com/im/users/15396970/prof...   

                                        host_picture_url host_neighbourhood  \
    0  https://a2.muscache.com/im/pictures/5936fef0-b...         Roslindale   
    1  https://a2.muscache.com/im/users/2572247/profi...         Roslindale   
    2  https://a2.muscache.com/im/users/16701/profile...         Roslindale   
    3  https://a2.muscache.com/im/pictures/5d430cde-7...                NaN   
    4  https://a0.muscache.com/im/users/15396970/prof...         Roslindale   

       host_listings_count  host_total_listings_count  \
    0                    1                          1   
    1                    1                          1   
    2                    1                          1   
    3                    1                          1   
    4                    1                          1   

                                      host_verifications host_has_profile_pic  \
    0          ['email', 'phone', 'facebook', 'reviews']                    t   
    1  ['email', 'phone', 'facebook', 'linkedin', 'am...                    t   
    2             ['email', 'phone', 'reviews', 'jumio']                    t   
    3                      ['email', 'phone', 'reviews']                    t   
    4               ['email', 'phone', 'reviews', 'kba']                    t   

      host_identity_verified                                             street  \
    0                      f      Birch Street, Boston, MA 02131, United States   
    1                      t  Pinehurst Street, Boston, MA 02131, United States   
    2                      t        Ardale St., Boston, MA 02131, United States   
    3                      f                          Boston, MA, United States   
    4                      t    Durnell Avenue, Boston, MA 02131, United States   

      neighbourhood neighbourhood_cleansed  neighbourhood_group_cleansed    city  \
    0    Roslindale             Roslindale                           NaN  Boston   
    1    Roslindale             Roslindale                           NaN  Boston   
    2    Roslindale             Roslindale                           NaN  Boston   
    3           NaN             Roslindale                           NaN  Boston   
    4    Roslindale             Roslindale                           NaN  Boston   

      state zipcode  market smart_location country_code        country   latitude  \
    0    MA   02131  Boston     Boston, MA           US  United States  42.282619   
    1    MA   02131  Boston     Boston, MA           US  United States  42.286241   
    2    MA   02131  Boston     Boston, MA           US  United States  42.292438   
    3    MA     NaN  Boston     Boston, MA           US  United States  42.281106   
    4    MA   02131  Boston     Boston, MA           US  United States  42.284512   

       longitude is_location_exact property_type        room_type  accommodates  \
    0 -71.133068                 t         House  Entire home/apt             4   
    1 -71.134374                 t     Apartment     Private room             2   
    2 -71.135765                 t     Apartment     Private room             2   
    3 -71.121021                 f         House     Private room             4   
    4 -71.136258                 t         House     Private room             2   

       bathrooms  bedrooms  beds  bed_type  \
    0        1.5       2.0   3.0  Real Bed   
    1        1.0       1.0   1.0  Real Bed   
    2        1.0       1.0   1.0  Real Bed   
    3        1.0       1.0   2.0  Real Bed   
    4        1.5       1.0   2.0  Real Bed   

                                               amenities  square_feet    price  \
    0  {TV,"Wireless Internet",Kitchen,"Free Parking ...          NaN  $250.00   
    1  {TV,Internet,"Wireless Internet","Air Conditio...          NaN   $65.00   
    2  {TV,"Cable TV","Wireless Internet","Air Condit...          NaN   $65.00   
    3  {TV,Internet,"Wireless Internet","Air Conditio...          NaN   $75.00   
    4  {Internet,"Wireless Internet","Air Conditionin...          NaN   $79.00   

      weekly_price monthly_price security_deposit cleaning_fee  guests_included  \
    0          NaN           NaN              NaN       $35.00                1   
    1      $400.00           NaN           $95.00       $10.00                0   
    2      $395.00     $1,350.00              NaN          NaN                1   
    3          NaN           NaN          $100.00       $50.00                2   
    4          NaN           NaN              NaN       $15.00                1   

      extra_people  minimum_nights  maximum_nights calendar_updated  \
    0        $0.00               2            1125      2 weeks ago   
    1        $0.00               2              15       a week ago   
    2       $20.00               3              45       5 days ago   
    3       $25.00               1            1125       a week ago   
    4        $0.00               2              31      2 weeks ago   

       has_availability  availability_30  availability_60  availability_90  \
    0               NaN                0                0                0   
    1               NaN               26               54               84   
    2               NaN               19               46               61   
    3               NaN                6               16               26   
    4               NaN               13               34               59   

       availability_365 calendar_last_scraped  number_of_reviews first_review  \
    0                 0            2016-09-06                  0          NaN   
    1               359            2016-09-06                 36   2014-06-01   
    2               319            2016-09-06                 41   2009-07-19   
    3                98            2016-09-06                  1   2016-08-28   
    4               334            2016-09-06                 29   2015-08-18   

      last_review  review_scores_rating  review_scores_accuracy  \
    0         NaN                   NaN                     NaN   
    1  2016-08-13                  94.0                    10.0   
    2  2016-08-05                  98.0                    10.0   
    3  2016-08-28                 100.0                    10.0   
    4  2016-09-01                  99.0                    10.0   

       review_scores_cleanliness  review_scores_checkin  \
    0                        NaN                    NaN   
    1                        9.0                   10.0   
    2                        9.0                   10.0   
    3                       10.0                   10.0   
    4                       10.0                   10.0   

       review_scores_communication  review_scores_location  review_scores_value  \
    0                          NaN                     NaN                  NaN   
    1                         10.0                     9.0                  9.0   
    2                         10.0                     9.0                 10.0   
    3                         10.0                    10.0                 10.0   
    4                         10.0                     9.0                 10.0   

      requires_license  license  jurisdiction_names instant_bookable  \
    0                f      NaN                 NaN                f   
    1                f      NaN                 NaN                t   
    2                f      NaN                 NaN                f   
    3                f      NaN                 NaN                f   
    4                f      NaN                 NaN                f   

      cancellation_policy require_guest_profile_picture  \
    0            moderate                             f   
    1            moderate                             f   
    2            moderate                             t   
    3            moderate                             f   
    4            flexible                             f   

      require_guest_phone_verification  calculated_host_listings_count  \
    0                                f                               1   
    1                                f                               1   
    2                                f                               1   
    3                                f                               1   
    4                                f                               1   

       reviews_per_month  
    0                NaN  
    1               1.30  
    2               0.47  
    3               1.00  
    4               2.25  

</div>

</div>

<div class="cell code" execution_count="8">

``` python
print('The number of columns {} and the number of rows are {}'.format(dfL.shape[1],dfL.shape[0]))
```

<div class="output stream stdout">

    The number of columns 91 and the number of rows are 3585

</div>

</div>

<div class="cell code" execution_count="9" scrolled="true">

``` python
dfL.info()
```

<div class="output stream stdout">

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3585 entries, 0 to 3584
    Data columns (total 91 columns):
     #   Column                            Non-Null Count  Dtype  
    ---  ------                            --------------  -----  
     0   id                                3585 non-null   int64  
     1   listing_url                       3585 non-null   object 
     2   scrape_id                         3585 non-null   int64  
     3   last_scraped                      3585 non-null   object 
     4   name                              3585 non-null   object 
     5   summary                           3442 non-null   object 
     6   space                             2528 non-null   object 
     7   description                       3585 non-null   object 
     8   experiences_offered               3585 non-null   object 
     9   neighborhood_overview             2170 non-null   object 
     10  notes                             1610 non-null   object 
     11  transit                           2295 non-null   object 
     12  access                            2096 non-null   object 
     13  interaction                       2031 non-null   object 
     14  house_rules                       2393 non-null   object 
     15  thumbnail_url                     2986 non-null   object 
     16  medium_url                        2986 non-null   object 
     17  picture_url                       3585 non-null   object 
     18  xl_picture_url                    2986 non-null   object 
     19  host_id                           3585 non-null   int64  
     20  host_url                          3585 non-null   object 
     21  host_name                         3585 non-null   object 
     22  host_since                        3585 non-null   object 
     23  host_location                     3574 non-null   object 
     24  host_about                        2276 non-null   object 
     25  host_response_time                3114 non-null   object 
     26  host_response_rate                3114 non-null   object 
     27  host_acceptance_rate              3114 non-null   object 
     28  host_is_superhost                 3585 non-null   object 
     29  host_thumbnail_url                3585 non-null   object 
     30  host_picture_url                  3585 non-null   object 
     31  host_neighbourhood                3246 non-null   object 
     32  host_listings_count               3585 non-null   int64  
     33  host_total_listings_count         3585 non-null   int64  
     34  host_verifications                3585 non-null   object 
     35  host_has_profile_pic              3585 non-null   object 
     36  host_identity_verified            3585 non-null   object 
     37  street                            3585 non-null   object 
     38  neighbourhood                     3042 non-null   object 
     39  neighbourhood_cleansed            3585 non-null   object 
     40  neighbourhood_group_cleansed      0 non-null      float64
     41  city                              3583 non-null   object 
     42  state                             3585 non-null   object 
     43  zipcode                           3547 non-null   object 
     44  market                            3571 non-null   object 
     45  smart_location                    3585 non-null   object 
     46  country_code                      3585 non-null   object 
     47  country                           3585 non-null   object 
     48  latitude                          3585 non-null   float64
     49  longitude                         3585 non-null   float64
     50  is_location_exact                 3585 non-null   object 
     51  property_type                     3582 non-null   object 
     52  room_type                         3585 non-null   object 
     53  accommodates                      3585 non-null   int64  
     54  bathrooms                         3571 non-null   float64
     55  bedrooms                          3575 non-null   float64
     56  beds                              3576 non-null   float64
     57  bed_type                          3585 non-null   object 
     58  amenities                         3585 non-null   object 
     59  price                             3585 non-null   object 
     60  weekly_price                      892 non-null    object 
     61  monthly_price                     888 non-null    object 
     62  security_deposit                  1342 non-null   object 
     63  cleaning_fee                      2478 non-null   object 
     64  guests_included                   3585 non-null   int64  
     65  extra_people                      3585 non-null   object 
     66  minimum_nights                    3585 non-null   int64  
     67  maximum_nights                    3585 non-null   int64  
     68  calendar_updated                  3585 non-null   object 
     69  availability_30                   3585 non-null   int64  
     70  availability_60                   3585 non-null   int64  
     71  availability_90                   3585 non-null   int64  
     72  availability_365                  3585 non-null   int64  
     73  calendar_last_scraped             3585 non-null   object 
     74  number_of_reviews                 3585 non-null   int64  
     75  first_review                      2829 non-null   object 
     76  last_review                       2829 non-null   object 
     77  review_scores_rating              2772 non-null   float64
     78  review_scores_accuracy            2762 non-null   float64
     79  review_scores_cleanliness         2767 non-null   float64
     80  review_scores_checkin             2765 non-null   float64
     81  review_scores_communication       2767 non-null   float64
     82  review_scores_location            2763 non-null   float64
     83  review_scores_value               2764 non-null   float64
     84  requires_license                  3585 non-null   object 
     85  instant_bookable                  3585 non-null   object 
     86  cancellation_policy               3585 non-null   object 
     87  require_guest_profile_picture     3585 non-null   object 
     88  require_guest_phone_verification  3585 non-null   object 
     89  calculated_host_listings_count    3585 non-null   int64  
     90  reviews_per_month                 2829 non-null   float64
    dtypes: float64(14), int64(15), object(62)
    memory usage: 2.5+ MB

</div>

</div>

<div class="cell markdown">

I will drop has availability, jurisdiction names, license and
square_feet columns as columns are empty column , and square feet barely
has data ( 56 records out of 3585) and other columns like
bathrooms,bedrooms,beds can give the required info.

</div>

<div class="cell code" execution_count="7">

``` python
dfL.drop(columns=['jurisdiction_names','has_availability','jurisdiction_names','license','square_feet'],inplace=True)
```

</div>

<div class="cell code" execution_count="10">

``` python
#Inputting numeric missing value
dfL['bathrooms'].fillna(dfL['bathrooms'].mean(),inplace=True)
dfL['bedrooms'].fillna(dfL['bedrooms'].mean(),inplace=True)
dfL['beds'].fillna(dfL['beds'].mean(),inplace=True)
```

</div>

<div class="cell markdown">

AS for the columns host_response_time,host_response_rate and
host_acceptance_rate will be fill with mode since they are a categorical
variables

</div>

<div class="cell code" execution_count="11">

``` python
#Inputting categorical missing value
dfL['host_response_time'].fillna(dfL['host_response_time'].mode()[0],inplace=True)
dfL['host_response_rate'].fillna(dfL['host_response_rate'].mode()[0],inplace=True)
dfL['host_acceptance_rate'].fillna(dfL['host_acceptance_rate'].mode()[0],inplace=True)
```

</div>

<div class="cell code" execution_count="12" scrolled="true">

``` python
#Explore after inputting
dfL.info()
```

<div class="output stream stdout">

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 3585 entries, 0 to 3584
    Data columns (total 91 columns):
     #   Column                            Non-Null Count  Dtype  
    ---  ------                            --------------  -----  
     0   id                                3585 non-null   int64  
     1   listing_url                       3585 non-null   object 
     2   scrape_id                         3585 non-null   int64  
     3   last_scraped                      3585 non-null   object 
     4   name                              3585 non-null   object 
     5   summary                           3442 non-null   object 
     6   space                             2528 non-null   object 
     7   description                       3585 non-null   object 
     8   experiences_offered               3585 non-null   object 
     9   neighborhood_overview             2170 non-null   object 
     10  notes                             1610 non-null   object 
     11  transit                           2295 non-null   object 
     12  access                            2096 non-null   object 
     13  interaction                       2031 non-null   object 
     14  house_rules                       2393 non-null   object 
     15  thumbnail_url                     2986 non-null   object 
     16  medium_url                        2986 non-null   object 
     17  picture_url                       3585 non-null   object 
     18  xl_picture_url                    2986 non-null   object 
     19  host_id                           3585 non-null   int64  
     20  host_url                          3585 non-null   object 
     21  host_name                         3585 non-null   object 
     22  host_since                        3585 non-null   object 
     23  host_location                     3574 non-null   object 
     24  host_about                        2276 non-null   object 
     25  host_response_time                3585 non-null   object 
     26  host_response_rate                3585 non-null   object 
     27  host_acceptance_rate              3585 non-null   object 
     28  host_is_superhost                 3585 non-null   object 
     29  host_thumbnail_url                3585 non-null   object 
     30  host_picture_url                  3585 non-null   object 
     31  host_neighbourhood                3246 non-null   object 
     32  host_listings_count               3585 non-null   int64  
     33  host_total_listings_count         3585 non-null   int64  
     34  host_verifications                3585 non-null   object 
     35  host_has_profile_pic              3585 non-null   object 
     36  host_identity_verified            3585 non-null   object 
     37  street                            3585 non-null   object 
     38  neighbourhood                     3042 non-null   object 
     39  neighbourhood_cleansed            3585 non-null   object 
     40  neighbourhood_group_cleansed      0 non-null      float64
     41  city                              3583 non-null   object 
     42  state                             3585 non-null   object 
     43  zipcode                           3547 non-null   object 
     44  market                            3571 non-null   object 
     45  smart_location                    3585 non-null   object 
     46  country_code                      3585 non-null   object 
     47  country                           3585 non-null   object 
     48  latitude                          3585 non-null   float64
     49  longitude                         3585 non-null   float64
     50  is_location_exact                 3585 non-null   object 
     51  property_type                     3582 non-null   object 
     52  room_type                         3585 non-null   object 
     53  accommodates                      3585 non-null   int64  
     54  bathrooms                         3585 non-null   float64
     55  bedrooms                          3585 non-null   float64
     56  beds                              3585 non-null   float64
     57  bed_type                          3585 non-null   object 
     58  amenities                         3585 non-null   object 
     59  price                             3585 non-null   object 
     60  weekly_price                      892 non-null    object 
     61  monthly_price                     888 non-null    object 
     62  security_deposit                  1342 non-null   object 
     63  cleaning_fee                      2478 non-null   object 
     64  guests_included                   3585 non-null   int64  
     65  extra_people                      3585 non-null   object 
     66  minimum_nights                    3585 non-null   int64  
     67  maximum_nights                    3585 non-null   int64  
     68  calendar_updated                  3585 non-null   object 
     69  availability_30                   3585 non-null   int64  
     70  availability_60                   3585 non-null   int64  
     71  availability_90                   3585 non-null   int64  
     72  availability_365                  3585 non-null   int64  
     73  calendar_last_scraped             3585 non-null   object 
     74  number_of_reviews                 3585 non-null   int64  
     75  first_review                      2829 non-null   object 
     76  last_review                       2829 non-null   object 
     77  review_scores_rating              2772 non-null   float64
     78  review_scores_accuracy            2762 non-null   float64
     79  review_scores_cleanliness         2767 non-null   float64
     80  review_scores_checkin             2765 non-null   float64
     81  review_scores_communication       2767 non-null   float64
     82  review_scores_location            2763 non-null   float64
     83  review_scores_value               2764 non-null   float64
     84  requires_license                  3585 non-null   object 
     85  instant_bookable                  3585 non-null   object 
     86  cancellation_policy               3585 non-null   object 
     87  require_guest_profile_picture     3585 non-null   object 
     88  require_guest_phone_verification  3585 non-null   object 
     89  calculated_host_listings_count    3585 non-null   int64  
     90  reviews_per_month                 2829 non-null   float64
    dtypes: float64(14), int64(15), object(62)
    memory usage: 2.5+ MB

</div>

</div>

<div class="cell markdown">

handeling DF calendar price by converting data type into float and
removing $ sign from it

</div>

<div class="cell code" execution_count="13">

``` python
def convert_price(df):
    '''
    summary : function will clean the price column by removing the dollar sign
              and convert it to float 
              
    Input: peice column from calendar that it's needed to clean the price in it
    
    Output : df with Cleaned price ( $ ,",") characters are removed
    
     '''
    df['price']=df['price'].map(lambda v: float(v[1:].replace(",","")) if type(v) != float else v)
    return df
```

</div>

<div class="cell code" execution_count="14" scrolled="true">

``` python
convert_price(dfC)
dfC.tail(50)
```

<div class="output execute_result" execution_count="14">

             listing_id        date available  price
    1308840    14504422  2016-10-25         t   65.0
    1308841    14504422  2016-10-24         t   65.0
    1308842    14504422  2016-10-23         t   65.0
    1308843    14504422  2016-10-22         t   65.0
    1308844    14504422  2016-10-21         t   65.0
    1308845    14504422  2016-10-20         t   65.0
    1308846    14504422  2016-10-19         t   65.0
    1308847    14504422  2016-10-18         t   65.0
    1308848    14504422  2016-10-17         t   65.0
    1308849    14504422  2016-10-16         t   65.0
    1308850    14504422  2016-10-15         t   65.0
    1308851    14504422  2016-10-14         t   65.0
    1308852    14504422  2016-10-13         t   65.0
    1308853    14504422  2016-10-12         t   65.0
    1308854    14504422  2016-10-11         f    NaN
    1308855    14504422  2016-10-10         f    NaN
    1308856    14504422  2016-10-09         t   65.0
    1308857    14504422  2016-10-08         t   65.0
    1308858    14504422  2016-10-07         t   65.0
    1308859    14504422  2016-10-06         t   65.0
    1308860    14504422  2016-10-05         t   65.0
    1308861    14504422  2016-10-04         t   65.0
    1308862    14504422  2016-10-03         t   65.0
    1308863    14504422  2016-10-02         t   65.0
    1308864    14504422  2016-10-01         t   62.0
    1308865    14504422  2016-09-30         t   62.0
    1308866    14504422  2016-09-29         t   62.0
    1308867    14504422  2016-09-28         f    NaN
    1308868    14504422  2016-09-27         f    NaN
    1308869    14504422  2016-09-26         f    NaN
    1308870    14504422  2016-09-25         t   65.0
    1308871    14504422  2016-09-24         t   62.0
    1308872    14504422  2016-09-23         t   62.0
    1308873    14504422  2016-09-22         t   62.0
    1308874    14504422  2016-09-21         t   62.0
    1308875    14504422  2016-09-20         t   62.0
    1308876    14504422  2016-09-19         t   62.0
    1308877    14504422  2016-09-18         t   62.0
    1308878    14504422  2016-09-17         t   62.0
    1308879    14504422  2016-09-16         t   62.0
    1308880    14504422  2016-09-15         f    NaN
    1308881    14504422  2016-09-14         f    NaN
    1308882    14504422  2016-09-13         f    NaN
    1308883    14504422  2016-09-12         f    NaN
    1308884    14504422  2016-09-11         f    NaN
    1308885    14504422  2016-09-10         f    NaN
    1308886    14504422  2016-09-09         f    NaN
    1308887    14504422  2016-09-08         f    NaN
    1308888    14504422  2016-09-07         f    NaN
    1308889    14504422  2016-09-06         f    NaN

</div>

</div>

<div class="cell markdown">

Now will devide the data into month and years columns for the calender
data frame and replacing the avilabilty columns with 1 for True 0 for
False

</div>

<div class="cell code" execution_count="15">

``` python
#Handling symbols by changing in data and separate date column
dfC['available'].replace({'t':1,'f':0},inplace=True)
dfC['month']=pd.DatetimeIndex(dfC['date']).month
dfC['year']=pd.DatetimeIndex(dfC['date']).year
dfC['Month_Year'] = pd.to_datetime(dfC['date']).dt.to_period('M')

dfC.head()
```

<div class="output execute_result" execution_count="15">

       listing_id        date  available  price  month  year Month_Year
    0    12147973  2017-09-05          0    NaN      9  2017    2017-09
    1    12147973  2017-09-04          0    NaN      9  2017    2017-09
    2    12147973  2017-09-03          0    NaN      9  2017    2017-09
    3    12147973  2017-09-02          0    NaN      9  2017    2017-09
    4    12147973  2017-09-01          0    NaN      9  2017    2017-09

</div>

</div>

<div class="cell code" execution_count="16" scrolled="true">

``` python
convert_price(dfL)
dfL.head()
```

<div class="output execute_result" execution_count="16">

             id                            listing_url       scrape_id  \
    0  12147973  https://www.airbnb.com/rooms/12147973  20160906204935   
    1   3075044   https://www.airbnb.com/rooms/3075044  20160906204935   
    2      6976      https://www.airbnb.com/rooms/6976  20160906204935   
    3   1436513   https://www.airbnb.com/rooms/1436513  20160906204935   
    4   7651065   https://www.airbnb.com/rooms/7651065  20160906204935   

      last_scraped                                           name  \
    0   2016-09-07                     Sunny Bungalow in the City   
    1   2016-09-07              Charming room in pet friendly apt   
    2   2016-09-07               Mexican Folk Art Haven in Boston   
    3   2016-09-07  Spacious Sunny Bedroom Suite in Historic Home   
    4   2016-09-07                            Come Home to Boston   

                                                 summary  \
    0  Cozy, sunny, family home.  Master bedroom high...   
    1  Charming and quiet room in a second floor 1910...   
    2  Come stay with a friendly, middle-aged guy in ...   
    3  Come experience the comforts of home away from...   
    4  My comfy, clean and relaxing home is one block...   

                                                   space  \
    0  The house has an open and cozy feel at the sam...   
    1  Small but cozy and quite room with a full size...   
    2  Come stay with a friendly, middle-aged guy in ...   
    3  Most places you find in Boston are small howev...   
    4  Clean, attractive, private room, one block fro...   

                                             description experiences_offered  \
    0  Cozy, sunny, family home.  Master bedroom high...                none   
    1  Charming and quiet room in a second floor 1910...                none   
    2  Come stay with a friendly, middle-aged guy in ...                none   
    3  Come experience the comforts of home away from...                none   
    4  My comfy, clean and relaxing home is one block...                none   

                                   neighborhood_overview  \
    0  Roslindale is quiet, convenient and friendly. ...   
    1  The room is in Roslindale, a diverse and prima...   
    2  The LOCATION: Roslindale is a safe and diverse...   
    3  Roslindale is a lovely little neighborhood loc...   
    4  I love the proximity to downtown, the neighbor...   

                                                   notes  \
    0                                                NaN   
    1  If you don't have a US cell phone, you can tex...   
    2  I am in a scenic part of Boston with a couple ...   
    3  Please be mindful of the property as it is old...   
    4  I have one roommate who lives on the lower lev...   

                                                 transit  \
    0  The bus stop is 2 blocks away, and frequent. B...   
    1  Plenty of safe street parking. Bus stops a few...   
    2  PUBLIC TRANSPORTATION: From the house, quick p...   
    3  There are buses that stop right in front of th...   
    4  From Logan Airport  and South Station you have...   

                                                  access  \
    0  You will have access to 2 bedrooms, a living r...   
    1  Apt has one more bedroom (which I use) and lar...   
    2  I am living in the apartment during your stay,...   
    3  The basement has a washer dryer and gym area. ...   
    4  You will have access to the front and side por...   

                                             interaction  \
    0                                                NaN   
    1  If I am at home, I am likely working in my hom...   
    2  ABOUT ME: I'm a laid-back, friendly, unmarried...   
    3  We do live in the house therefore might be som...   
    4  I love my city and really enjoy sharing it wit...   

                                             house_rules  \
    0  Clean up and treat the home the way you'd like...   
    1  Pet friendly but please confirm with me if the...   
    2  I encourage you to use my kitchen, cooking and...   
    3  - The bathroom and house are shared so please ...   
    4  Please no smoking in the house, porch or on th...   

                                           thumbnail_url  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...   
    1  https://a1.muscache.com/im/pictures/39327812/d...   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...   
    3  https://a2.muscache.com/im/pictures/39764190-1...   
    4  https://a1.muscache.com/im/pictures/97154760/8...   

                                              medium_url  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...   
    1  https://a1.muscache.com/im/pictures/39327812/d...   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...   
    3  https://a2.muscache.com/im/pictures/39764190-1...   
    4  https://a1.muscache.com/im/pictures/97154760/8...   

                                             picture_url  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...   
    1  https://a1.muscache.com/im/pictures/39327812/d...   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...   
    3  https://a2.muscache.com/im/pictures/39764190-1...   
    4  https://a1.muscache.com/im/pictures/97154760/8...   

                                          xl_picture_url   host_id  \
    0  https://a2.muscache.com/im/pictures/c0842db1-e...  31303940   
    1  https://a1.muscache.com/im/pictures/39327812/d...   2572247   
    2  https://a2.muscache.com/im/pictures/6ae8335d-9...     16701   
    3  https://a2.muscache.com/im/pictures/39764190-1...   6031442   
    4  https://a1.muscache.com/im/pictures/97154760/8...  15396970   

                                         host_url host_name  host_since  \
    0  https://www.airbnb.com/users/show/31303940  Virginia  2015-04-15   
    1   https://www.airbnb.com/users/show/2572247    Andrea  2012-06-07   
    2     https://www.airbnb.com/users/show/16701      Phil  2009-05-11   
    3   https://www.airbnb.com/users/show/6031442    Meghna  2013-04-21   
    4  https://www.airbnb.com/users/show/15396970     Linda  2014-05-11   

                              host_location  \
    0  Boston, Massachusetts, United States   
    1  Boston, Massachusetts, United States   
    2  Boston, Massachusetts, United States   
    3  Boston, Massachusetts, United States   
    4  Boston, Massachusetts, United States   

                                              host_about  host_response_time  \
    0  We are country and city connecting in our deck...      within an hour   
    1  I live in Boston and I like to travel and have...      within an hour   
    2  I am a middle-aged, single male with a wide ra...  within a few hours   
    3  My husband and I live on the property.  He’s a...  within a few hours   
    4  I work full time for a public school district....      within an hour   

      host_response_rate host_acceptance_rate host_is_superhost  \
    0               100%                 100%                 f   
    1               100%                 100%                 f   
    2               100%                  88%                 t   
    3               100%                  50%                 f   
    4               100%                 100%                 t   

                                      host_thumbnail_url  \
    0  https://a2.muscache.com/im/pictures/5936fef0-b...   
    1  https://a2.muscache.com/im/users/2572247/profi...   
    2  https://a2.muscache.com/im/users/16701/profile...   
    3  https://a2.muscache.com/im/pictures/5d430cde-7...   
    4  https://a0.muscache.com/im/users/15396970/prof...   

                                        host_picture_url host_neighbourhood  \
    0  https://a2.muscache.com/im/pictures/5936fef0-b...         Roslindale   
    1  https://a2.muscache.com/im/users/2572247/profi...         Roslindale   
    2  https://a2.muscache.com/im/users/16701/profile...         Roslindale   
    3  https://a2.muscache.com/im/pictures/5d430cde-7...                NaN   
    4  https://a0.muscache.com/im/users/15396970/prof...         Roslindale   

       host_listings_count  host_total_listings_count  \
    0                    1                          1   
    1                    1                          1   
    2                    1                          1   
    3                    1                          1   
    4                    1                          1   

                                      host_verifications host_has_profile_pic  \
    0          ['email', 'phone', 'facebook', 'reviews']                    t   
    1  ['email', 'phone', 'facebook', 'linkedin', 'am...                    t   
    2             ['email', 'phone', 'reviews', 'jumio']                    t   
    3                      ['email', 'phone', 'reviews']                    t   
    4               ['email', 'phone', 'reviews', 'kba']                    t   

      host_identity_verified                                             street  \
    0                      f      Birch Street, Boston, MA 02131, United States   
    1                      t  Pinehurst Street, Boston, MA 02131, United States   
    2                      t        Ardale St., Boston, MA 02131, United States   
    3                      f                          Boston, MA, United States   
    4                      t    Durnell Avenue, Boston, MA 02131, United States   

      neighbourhood neighbourhood_cleansed  neighbourhood_group_cleansed    city  \
    0    Roslindale             Roslindale                           NaN  Boston   
    1    Roslindale             Roslindale                           NaN  Boston   
    2    Roslindale             Roslindale                           NaN  Boston   
    3           NaN             Roslindale                           NaN  Boston   
    4    Roslindale             Roslindale                           NaN  Boston   

      state zipcode  market smart_location country_code        country   latitude  \
    0    MA   02131  Boston     Boston, MA           US  United States  42.282619   
    1    MA   02131  Boston     Boston, MA           US  United States  42.286241   
    2    MA   02131  Boston     Boston, MA           US  United States  42.292438   
    3    MA     NaN  Boston     Boston, MA           US  United States  42.281106   
    4    MA   02131  Boston     Boston, MA           US  United States  42.284512   

       longitude is_location_exact property_type        room_type  accommodates  \
    0 -71.133068                 t         House  Entire home/apt             4   
    1 -71.134374                 t     Apartment     Private room             2   
    2 -71.135765                 t     Apartment     Private room             2   
    3 -71.121021                 f         House     Private room             4   
    4 -71.136258                 t         House     Private room             2   

       bathrooms  bedrooms  beds  bed_type  \
    0        1.5       2.0   3.0  Real Bed   
    1        1.0       1.0   1.0  Real Bed   
    2        1.0       1.0   1.0  Real Bed   
    3        1.0       1.0   2.0  Real Bed   
    4        1.5       1.0   2.0  Real Bed   

                                               amenities  price weekly_price  \
    0  {TV,"Wireless Internet",Kitchen,"Free Parking ...  250.0          NaN   
    1  {TV,Internet,"Wireless Internet","Air Conditio...   65.0      $400.00   
    2  {TV,"Cable TV","Wireless Internet","Air Condit...   65.0      $395.00   
    3  {TV,Internet,"Wireless Internet","Air Conditio...   75.0          NaN   
    4  {Internet,"Wireless Internet","Air Conditionin...   79.0          NaN   

      monthly_price security_deposit cleaning_fee  guests_included extra_people  \
    0           NaN              NaN       $35.00                1        $0.00   
    1           NaN           $95.00       $10.00                0        $0.00   
    2     $1,350.00              NaN          NaN                1       $20.00   
    3           NaN          $100.00       $50.00                2       $25.00   
    4           NaN              NaN       $15.00                1        $0.00   

       minimum_nights  maximum_nights calendar_updated  availability_30  \
    0               2            1125      2 weeks ago                0   
    1               2              15       a week ago               26   
    2               3              45       5 days ago               19   
    3               1            1125       a week ago                6   
    4               2              31      2 weeks ago               13   

       availability_60  availability_90  availability_365 calendar_last_scraped  \
    0                0                0                 0            2016-09-06   
    1               54               84               359            2016-09-06   
    2               46               61               319            2016-09-06   
    3               16               26                98            2016-09-06   
    4               34               59               334            2016-09-06   

       number_of_reviews first_review last_review  review_scores_rating  \
    0                  0          NaN         NaN                   NaN   
    1                 36   2014-06-01  2016-08-13                  94.0   
    2                 41   2009-07-19  2016-08-05                  98.0   
    3                  1   2016-08-28  2016-08-28                 100.0   
    4                 29   2015-08-18  2016-09-01                  99.0   

       review_scores_accuracy  review_scores_cleanliness  review_scores_checkin  \
    0                     NaN                        NaN                    NaN   
    1                    10.0                        9.0                   10.0   
    2                    10.0                        9.0                   10.0   
    3                    10.0                       10.0                   10.0   
    4                    10.0                       10.0                   10.0   

       review_scores_communication  review_scores_location  review_scores_value  \
    0                          NaN                     NaN                  NaN   
    1                         10.0                     9.0                  9.0   
    2                         10.0                     9.0                 10.0   
    3                         10.0                    10.0                 10.0   
    4                         10.0                     9.0                 10.0   

      requires_license instant_bookable cancellation_policy  \
    0                f                f            moderate   
    1                f                t            moderate   
    2                f                f            moderate   
    3                f                f            moderate   
    4                f                f            flexible   

      require_guest_profile_picture require_guest_phone_verification  \
    0                             f                                f   
    1                             f                                f   
    2                             t                                f   
    3                             f                                f   
    4                             f                                f   

       calculated_host_listings_count  reviews_per_month  
    0                               1                NaN  
    1                               1               1.30  
    2                               1               0.47  
    3                               1               1.00  
    4                               1               2.25  

</div>

</div>

<div class="cell markdown">

# Data modeling and insight

</div>

<div class="cell markdown">

# what is the most busys month in terms of availbility of offeres ?

</div>

<div class="cell code" execution_count="17">

``` python
availibe=dfC.groupby('month')['available'].mean().reset_index().rename(columns={'available':'avg_availability'})

x=availibe['month']
y=availibe['avg_availability']

#availibility.plot()
plt.figure(figsize=(15,8))
plt.title('Average Availability over Month')

sns.barplot(data=availibe,x='month',y='avg_availability',color='c',palette=None)
availibe.sort_values(by='avg_availability',ascending=False)
```

<div class="output execute_result" execution_count="17">

        month  avg_availability
    0       1          0.568348
    1       2          0.565792
    11     12          0.548702
    10     11          0.547388
    7       8          0.499802
    2       3          0.496384
    6       7          0.494423
    5       6          0.490156
    4       5          0.482162
    3       4          0.477617
    9      10          0.416899
    8       9          0.310448

</div>

<div class="output display_data">

![](fabc4b59148aad4637b22681943de1b98b8630fd.png)

</div>

</div>

<div class="cell markdown">

through this diagram we can find out that the most busy months is from
November until February we can assumem that becuse of holiday's

</div>

<div class="cell markdown">

# which neighborhood is the most expensive\\cheapest throw the offeres we have in Boston?

</div>

<div class="cell code" execution_count="18">

``` python
most_exp=dfL.groupby('neighbourhood')['price'].mean().sort_values(ascending=False).reset_index().rename(columns={'price':'avg_price'})

x=most_exp['neighbourhood'].head(5)
y=most_exp['avg_price'].head(5)

plt.figure(figsize=(15,8))
plt.title("hightest neighbourhood in terms of Price for the night")
sns.barplot(data=most_exp,x=x,y=y,color='c',palette=None)
most_exp.head(5)
```

<div class="output execute_result" execution_count="18">

            neighbourhood   avg_price
    0      Harvard Square  359.000000
    1  Financial District  283.692308
    2   Downtown Crossing  273.500000
    3    Leather District  245.875000
    4            Back Bay  245.457045

</div>

<div class="output display_data">

![](3d3d0467aecdb51c4c0d4c885d6ad6a35110ed15.png)

</div>

</div>

<div class="cell code" execution_count="19">

``` python
most_chep =dfL.groupby('neighbourhood')['price'].mean().sort_values(ascending=True).reset_index().rename(columns={'price':'avg_price'})

x=most_chep['neighbourhood'].head(5)
y=most_chep['avg_price'].head(5)

plt.figure(figsize=(15,8))
plt.title("The most chepest neighbourhood in terms of Price per night")
sns.barplot(data=most_chep,x=x,y=y,color='c',palette=None)
most_chep.head(5)
```

<div class="output execute_result" execution_count="19">

       neighbourhood  avg_price
    0  Chestnut Hill  70.750000
    1       Mattapan  72.000000
    2     Somerville  93.076923
    3      Hyde Park  93.680000
    4     Dorchester  97.451282

</div>

<div class="output display_data">

![](0233b9a7538ddca991ca3f7222b36fa6de7a7012.png)

</div>

</div>

<div class="cell markdown">

So from the two figure up we can see that 'Harvard Square' is the most
expensive neighbourhood as for the chepest neighbourhood is 'Chestnut
Hill' in Boston

</div>

<div class="cell markdown">

# when is the highest average price of offers per night ?

</div>

<div class="cell code" execution_count="20">

``` python
avg_pr=dfC.groupby('month')['price'].mean().reset_index().rename(columns={'price':'avg_price'})

x=avg_pr['month']
y=avg_pr['avg_price']

plt.figure(figsize=(15,8))
plt.title("Average Prices per Month")
sns.barplot(data=avg_pr,x='month',y='avg_price',color='c',palette=None)
avg_pr.sort_values(by='avg_price',ascending=False)
```

<div class="output execute_result" execution_count="20">

        month   avg_price
    8       9  237.047727
    9      10  233.416248
    7       8  203.330142
    10     11  202.924416
    6       7  202.486309
    3       4  197.252890
    5       6  196.535302
    4       5  193.712295
    11     12  192.601915
    0       1  182.799671
    2       3  181.818742
    1       2  180.961028

</div>

<div class="output display_data">

![](05eab309c705e4dec567f2bc7be0a5a261118a0d.png)

</div>

</div>

<div class="cell markdown">

From the diagram we can say that on september and october has the most
highest average from other month

</div>

<div class="cell markdown">

# Conclusion

</div>

<div class="cell markdown">

From the analaysis we did before we can say that Boston is a good place
for busines and has more than one option to stay based on the toreest
budeget and the neighbourhood for me i don't advice going there on
Holiday's

</div>

<div class="cell code">

``` python
```

</div>
