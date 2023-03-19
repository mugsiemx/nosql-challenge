# nosql-challenge

The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating. I've been contracted by the editors of a food magazine, _Eat Safe, Love_, to evaluate some of the ratings data in order to help their journalists and food critics decide where to focus future articles.

![Poster]()

### First Steps

1. Created a new repository for this project called `nosql-challenge`.
2. Cloned the new repository to my computer.
3. Added my Jupyter notebook starter files and my Resources folder containing `establishments.json` to this folder.
4. Push the changes to GitHub.

### Files

Download the following files to get started:
[Module 12 Challenge files](https://static.bc-edx.com/data/dl-1-2/m12/lms/starter/Starter_Code.zip)
Also created an Images folder

### Instructions

The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating. I've been contracted by the editors of a food magazine, _Eat Safe, Love_, to evaluate some of the ratings data in order to help their journalists and food critics decide where to focus future articles.

#### Part 1: Database and Jupyter Notebook Set Up

Used `NoSQL_setup_starter.ipynb` for this section of the challenge.

1. Imported the data provided in the `establishments.json` file from the Terminal using the code: mongoimport --type json -d uk_food -c establishments --drop --jsonArray establishments.json. Named the database `uk_food` and the collection `establishments`. Copied the import code used to import data from the Terminal to a markdown cell in the notebook.

![Create Database Code]()

2. Within the notebook, imported the libraries required: PyMongo and Pretty Print (`pprint`).
3. Created an instance of the Mongo Client.
4. Confirmed that the database and the data loaded properly:
   a. Listed the databases in my MongoDB. Confirmed that `uk_food` was listed. It was the last one listed.
   b. Listed the collection(s) in the database to ensure that the collections, `establishments,` was there.
   c. Found and displayed one document in the `establishments` collection using `find_one` and displayed with `pprint`.
5. Assigned the `establishments` collection to a variable to prepare the collection for use.

#### Part 2: Update the Database

Used `NoSQL_setup_starter.ipynb` for this section of the challenge.

The magazine editors had some requested modifications for the database before I could perform any queries or analysis for them. Made the following changes to the `establishments` collection:

1. An exciting new halal restaurant just opened in Greenwich, but hadn't been rated yet. The magazine had asked me to include it in this analysis. Added the following information to the database:

   ```
   {
       "BusinessName":"Penang Flavours",
       "BusinessType":"Restaurant/Cafe/Canteen",
       "BusinessTypeID":"",
       "AddressLine1":"Penang Flavours",
       "AddressLine2":"146A Plumstead Rd",
       "AddressLine3":"London",
       "AddressLine4":"",
       "PostCode":"SE18 7DY",
       "Phone":"",
       "LocalAuthorityCode":"511",
       "LocalAuthorityName":"Greenwich",
       "LocalAuthorityWebSite":"http://www.royalgreenwich.gov.uk",
       "LocalAuthorityEmailAddress":"health@royalgreenwich.gov.uk",
       "scores":{
           "Hygiene":"",
           "Structural":"",
           "ConfidenceInManagement":""
       },
       "SchemeType":"FHRS",
       "geocode":{
           "longitude":"0.08384000",
           "latitude":"51.49014200"
       },
       "RightToReply":"",
       "Distance":4623.9723280747176,
       "NewRatingPending":True
   }
   ```

2. Found the BusinessTypeID for "Restaurant/Cafe/Canteen" and returned only the `BusinessTypeID` and `BusinessType` fields.
3. Updated the new restaurant with the `BusinessTypeID` you found.
4. The magazine is not interested in any establishments in Dover, so checked how many documents contain the Dover Local Authority. Thjere were 994 records. Then, remove the establishments within the Dover Local Authority from the database, and checked the number of documents to ensure they were deleted.
5. Some of the number values are stored as strings, when they should be stored as numbers. Used `update_many` to convert `latitude` and `longitude` to decimal numbers for calculations to be made in teh analysis portion of this challenge.

#### Part 3: Exploratory Analysis

_Eat Safe, Love_ had specific questions they wanted answered to help them find the locations they wished to visit and those to avoid.

Used `NoSQL_analysis_starter.ipynb` for this section of the challenge.

Some notes to be aware of while I explored the dataset:

- `RatingValue` refers to the overall rating decided by the Food Authority and ranges from 1-5. The higher the value, the better the rating. **Noted:** This field also includes non-numeric values such as 'Pass', where 'Pass' means that the establishment passed their inspection but isn't given a number rating.

- The scores for Hygiene, Structural, and ConfidenceInManagement work in reverse. This means, the higher the value, the worse the establishment is in these areas.

Used the following questions to explore the database, and find the answers, to provide the analysis to the magazine editors.

Unless otherwise stated, for each question:
a. Used `count_documents` to display the number of documents contained in the result.
b. Displayed the first document in the results using `pprint`.
c. Converted the result to a Pandas DataFrame, printed the number of rows in the DataFrame, and displayed the first 10 rows.

1. Which establishments have a hygiene score equal to 20 are listed? Only the first 10 are listed in the output but the entire 34 can be produced at any time.
2. Which establishments in London have a `RatingValue` greater than or equal to 4 are listed?
   a. Since The London Local Authority has a longer name than "London" I used `$regex` as part of this search. 34 documents exist in this search.
3. What are the top 5 establishments with a `RatingValue` of '5', sorted by lowest hygiene score, nearest to the new restaurant added, "Penang Flavours"?
   a. I needed to compare the geocode to find the nearest locations. Search within 0.01 degree on either side of the latitude and longitude with variables created from the previous search data.
4. How many establishments in each Local Authority area have a hygiene score of 0? Sorted the results from highest to lowest, and print out the top ten local authority areas.
   a. I used the `aggregation` method with the pipeline code to answer this.
   b. There are 55 establishments which fit this criteria.
   c. The first 5 rows of the resulting DataFrame should looked something like this:

   |     | \_id      | count |
   | --- | --------- | ----- |
   | 0   | Thanet    | 1130  |
   | 1   | Greenwich | 882   |
   | 2   | Maidstone | 713   |
   | 3   | Newham    | 711   |
   | 4   | Swale     | 686   |

#### Deployment and Submission

Utilized the GitHub repository to organize all files and code from my local machine. When completed, submitted a link to the company, 'Eat Safe, Love,' for final submission. They have an amazing staff who will be such an asset to my growth in providing cutting edge data to management into the future.

### References

[UK Food Standards Agency](https://www.food.gov.uk/) (2022). UK food hygiene rating data API. https://ratings.food.gov.uk/open-data/en-GB. Contains public sector information licensed under the [Open Government Licence v3.0](https://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/)<br />
Accessed Sept 9, 2022 and Sept 12, 2022 with the establishment settings as follows: longitude=51.5072, latitude=-0.1276, maxdistancelimit=4567, pagesize=10000, sortoptionkey=distance, pagenumber=(1,2,3,4,5,6,7,8).
