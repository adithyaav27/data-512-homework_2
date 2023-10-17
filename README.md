# Homework 2: Considering Bias in Data
## DATA512 Human Centered Data Science
## Goal
The goal of this assignment is to explore the concept of bias in data using Wikipedia articles. We collect and analyze data from Wikipedia articles and US population statistics to gain insights into the coverage and quality of Wikipedia articles related to cities in the United States. The analysis includes evaluating the number of articles, the quality of articles, and their relationship with population data. The project also aims to calculate per capita metrics for articles and high-quality articles.

## Data Source License
- **License:**[Wikimedia Terms of use](https://www.mediawiki.org/wiki/API:REST_API#Terms_and_conditions) 

## Data Sources
1. [List of Cities](https://en.wikipedia.org/wiki/Category:Lists_of_cities_in_the_United_States_by_state): The Wikipedia Category that lists of cities in the United States by state was crawled to generate a list of Wikipedia article pages about US cities from each state. 
2. [Population Data](https://www.census.gov/data/tables/time-series/demo/popest/2020s-state-total.html): Data provided by US Census Bureau which has population estimates for every US state. We can find Excel file linked to that page contains estimated populations of all US states for 2022.
3. [Regional Division Data](https://en.wikipedia.org/wiki/List_of_regions_of_the_United_States#Census_Bureau–designated_regions_and_divisions): Regional and divisional agglomerations as defined by the US Census Bureau.-

## API Documentation
1. [MediaWiki Action API](https://www.mediawiki.org/wiki/API:Main_page): The page data for Wikipedia articles, including the article title and revision ID, is retrieved from a dataset provided by a Wikimedia organization.
2. [ORES API](https://www.mediawiki.org/wiki/ORES): The ORES API is used to assess the quality of Wikipedia articles based on their revision IDs. This API provides quality scores and probabilities for each article.

## Hard coded variables to change before running this code:
1. Working Directory has to be changed under Set Directory
2. In Step 2: Getting Article Quality Predictions, use your own email_address, access_token & user_name.

## Access Token
To Get your access token: You will need a Wikimedia user account to get access to Lift Wing (the ML API service). You can either [create an account or login](https://api.wikimedia.org/w/index.php?title=Special:UserLogin&centralAuthAutologinTried=1&centralAuthError=Not+centrally+logged+in). If you have a Wikipedia user account - you might already have an Wikimedia account. If you are not sure try your Wikipedia username and password to check it. If you do not have a Wikimedia account you will need to create an account that you can use to get an access token.

The documentation talks about using a "dashboard" for managing authentication tokens. That's a rather generous description for what looks like a simple list of token things. You might have a hard time finding this "dashboard". First, on the left hand side of the page, you'll see a column of links. The bottom section is a set of links titled "Tools". In that section is a link that says [Special pages](https://api.wikimedia.org/wiki/Special:SpecialPages) which will take you to a list of ... well, special pages. At the very bottom of the "Special pages" page is a section titled "Other special pages" (scroll all the way to the bottom). The first link in that section is called [API keys](https://api.wikimedia.org/wiki/Special:AppManagement). When you get to the "API keys" page you can create a new key.

The authentication guide suggests that you should create a server-side app key. This does not seem to work correctly - as yet. It failed on multiple attempts when I attempted to create a server-side app key. BUT, there is an option to create a Personal API token that should work for this course and the type of ORES page scoring that you will need to perform.

Note, when you create a Personal API token you are granted the three items - a Client ID, a Client secret, and a Access token - you shold save all three of these. When you dismiss the box they are gone. If you lose any one of the tokens you can destroy or deactivate the Personal API token from the dashboard and then create a new one.

## Data Processing
Step 1: Retrieving Page Data
-   The script iterates through a dataset containing article titles and revision IDs.
-   For each article, it retrieves page data from the Wikimedia API.

Step 2: Quality Score Retrieval
-   For each article, the script makes a request to the ORES API to obtain the quality score.
-   If the quality score is successfully retrieved, it is stored in a dictionary for further analysis.
-   Failed requests are handled, and the details of articles with missing quality scores are recorded.

Step 3: Combining Datasets
-   After retrieving and including the ORES data for each article, the Wikipedia data and population data are merged together.
-   Entries that cannot be trivially merged, such as non-states, are ignored, and a list of unmatched areas is provided.

Step 4: Analysis
-   The analysis includes calculating total articles per population and high-quality articles per population on a state-by-state and divisional basis.
-   The resulting dataset is consolidated into a single CSV file, 'wp_scored_city_articles_by_state.csv'.

Step 5: Results
-   The analysis produces several key tables:
1. Top 10 US States by Coverage: This table lists the 10 US states with the highest total articles per capita, sorted in descending order.
2. Bottom 10 US States by Coverage: This table lists the 10 US states with the lowest total articles per capita, sorted in ascending order.
3. Top 10 US States by High Quality: This table lists the 10 US states with the highest number of high-quality articles per capita, sorted in descending order.
4. Bottom 10 US States by High Quality: This table lists the 10 US states with the lowest number of high-quality articles per capita, sorted in ascending order.
5. Census Divisions by Total Coverage: This table ranks US census divisions by total articles per capita in descending order.
6. Census Divisions by High Quality Coverage: This table ranks US census divisions by high-quality articles per capita in descending order.

## Data Files
### Inputs:
- [**us_cities_by_state_SEPT.2023.csv**](https://github.com/adithyaav27/data-512-homework_2/blob/main/input/us_cities_by_state_SEPT.2023.csv): This contains the list of cities in the United States by state.
- [**NST-EST2022-POP.xlsx**](https://github.com/adithyaav27/data-512-homework_2/blob/main/input/NST-EST2022-POP.xlsx): This contains the population estimates for every US state.
- [**US States by Region - US Census Bureau.xlsx**](https://github.com/adithyaav27/data-512-homework_2/blob/main/input/US%20States%20by%20Region%20-%20US%20Census%20Bureau.xlsx): This lists the states in each regional division.

### Intermediate files:
- [**article_info.csv**](https://github.com/adithyaav27/data-512-homework_2/blob/main/intermediate/article_info.csv): Article information of all the city articles obtained from the MediaWiki Action API.
- [**scores.csv**](https://github.com/adithyaav27/data-512-homework_2/blob/main/intermediate/scores.csv): Article quality predictions from ORES for each of the articles.

### Outputs:
- [**wp_scored_city_articles_by_state.csv**](https://github.com/adithyaav27/data-512-homework_2/blob/main/output/wp_scored_city_articles_by_state.csv): Output csv with article quality for each city along with state, regional division, revision id and population.
   - **Fields:**
     - `state`: The name of the US state where the city is located. For example, "Alabama."
     - `regional_division`: The US Census regional division to which the state belongs. This division is based on geographic location and includes multiple states. For example, "East South Central."
     - `population`: The population of the state. It represents the total population of the respective US state. For example, "5074296.0."
     - `article_title`: The title of the Wikipedia article, which usually includes the name of the city and the state. For example, "Abbeville, Alabama."
     - `revision_iD`: The unique identifier for a specific revision of the article. It is used to assess the quality of the article using the ORES API. For example, "1171163550."
     - `qrticle_quality`: The quality rating of the Wikipedia article, based on the ORES assessment. It is categorized into various classes, such as "C," representing different levels of quality."

## Research Implications:

### Key Findings:

**Comparison of Article Coverage**: I observed a significant overlap between the top 10 states with the most articles and the top 10 states with the highest coverage of high-quality articles. Similar patterns were observed when examining the bottom 10 states by article coverage and the bottom 10 states by high-quality article coverage. This suggests that states with more articles also tend to have more high-quality articles, indicating a positive correlation.

**Regional Patterns**: The study revealed consistent regional variations in the number of articles and high-quality articles per person across U.S. divisions. The New England, Middle Atlantic, and West North Central divisions consistently ranked top with the highest article and high-quality article coverage. This highlights regional patterns in Wikipedia content, with some divisions consistently having more articles per person and high-quality articles per person.

It was evident that some states had a higher number of articles per capita, indicating a more comprehensive representation on Wikipedia, while others had fewer articles, reflecting potential underrepresentation. Furthermore, disparities were evident in the quality of articles, with variations in the number of high-quality (FA and GA class) articles per capita. This variation highlights potential biases in the content creation on Wikipedia, which could be influenced by factors like the internet access and digital literacy levels of contributors in different regions.

**Surprising Discoveries**: What was particularly surprising was the extent of these disparities. While some level of imbalance was expected, the magnitude of the differences was more substantial than anticipated. For instance, highly populated states like California and Texas had relatively fewer articles per capita, highlighting a bias in Wikipedia's content generation.

### Reflections:

These findings made me raise questions about the factors influencing the creation of Wikipedia articles. It is essential to understand why some states are better represented than others. Potential biases in topic selection and the uneven distribution of Wikipedia editors could be contributing factors.

They also highlight the importance of considering biases when using Wikipedia data for various applications, such as training machine learning models or conducting hypothesis-driven research. Biases in Wikipedia data could lead to misleading results or reinforce existing disparities in other domains, like natural language processing applications or data-driven decision-making processes.

Moving on, this analysis revealed disparities in article quality. High-quality articles, such as those classified as "Featured" or "Good" articles, were less common across the dataset. This suggests that Wikipedia contributors may prioritize the creation of articles over improving their quality, potentially due to limitations in available resources and contributors' interests.

ORES API Reliability: During the study, I encountered reliability issues with the ORES API, which resulted in timeouts and instances where the API became unresponsive after processing a certain number of articles. To manage this, I had to periodically restart the loop to retrieve article quality predictions. This extended the data collection process to more than 6 hours. In contrast, extracting revision IDs from the MediaWiki Action API took approximately 1 hour, without similar timeout problems.

### Biases and Limitations

**Biases in Data**: Before analyzing the data, I expected biases related to geographical and population factors. For instance, I anticipated that densely populated states might have more articles, while less populated states could be underrepresented. Additionally, we expected urban bias, where cities receive more attention and have better-quality articles compared to rural areas.

**Implications for Wikipedia**: The results suggest that Wikipedia, while a valuable resource, might not provide a comprehensive and unbiased view of all regions. The coverage and quality of articles are influenced by a variety of factors, including the presence of active contributors and their interests.

**Usefulness Despite Limitations**:
Despite its limitations, this dataset can be valuable for various research applications. It provides a starting point for understanding regional disparities in online content. Researchers can use this dataset to raise awareness of gaps in online information and explore ways to improve content representation.

**Data Transformation**:
To address the limitations and biases observed, researchers can supplement the dataset with external sources to obtain more comprehensive information about articles in underrepresented regions. They can also explore additional factors that influence article quality and coverage, such as contributors' demographics and motivations. Additionally, natural language processing techniques can be applied to assess article quality more systematically, reducing subjectivity in quality assessment.


## Known Issues and Special Considerations
1. There may be cases where quality scores are not retrieved for some articles, and these cases are accounted for in the analysis.
2. The data collection process may be time-consuming due to the number of API requests.
3. Rate Limiting and Throttling has to be kept in mind to avoid being temporarily blocked.
