# insights-api
## Question C

Create an api that takes a string as input and returns a list of text insights from the top 5 google results of the query generated by those keywords. 
Test with the following string « keystone - Circular reference found role inference » . You will need to build a query , search the web (you can scrape google) , select the top 5 results and for each result scrape for the most relevant information on that page (relevant from a user’s perspective based on the original string ) . The response should contain the links to the websites that were scraped with the most relevant information (could be a text insight ,  a code snippet, a patch file, etc.) for each. In the response, feel free to add any other extra information that you think could be relevant.

As a hint, we are not looking for quantity, but rather quality, maintainability, scalability, testability and a code that you can be proud of.

## Approach
* I walked through the problem following PEDAC. My first thoughts concerning the problem are in approach folder (it is an image with my handwriting notes)
* Having my notes in mind and the question requirements, I thought about what stack should I use.

### Stack
#### Which tools to create the API?
* Concerning quality, maintainability, testability I chose Django rest framework. 
* Django modularizes your project in reusable apps that you can use in different projects.
* It follows REST concepts.
* It is a python framework so one can use NLP (Natural Language Processing) libraries right away.
* Concerning the scalability, Django scales (Pinterest and Instagram use Django). We can think about using Redis to cache searches and avoid reading operations on database frequentely. Moreover, we can use Kurbenetes, with Docker containers, which provides autoscaling and load balancer.

#### Which database?
* In order to avoid frequently scraping both google and top 5 pages it is a good idea to store those searches and save time. Thus, instead of googling again we might query for a similar entry on our database first.
* I chose to use a NoSql database (MongoDB).
* It is not necessary to update a search already stored, the top 5 results do not change often.
* Since this application hardly ever updates records and it has a high demand for reading operations, I think a NoSql database fits better.
* We can have different relevant information regarding a page, documents are flexibel concerning its structure.
* Using capped collections the older documents are removed. Imagine how many searches will be done, maybe it is not necessary to store all of them forever. Not for the goal of this API. One can check it in details here https://docs.mongodb.com/manual/core/capped-collections/.

### Steps I took 
#### Scrap google and 5 top pages
* I started coding following TDD approach using the built-in unit test library
* In order to scrap the pages, there are two well-known python libraries: BeautifulSoup and RequestsHTML. I chose RequestsHTML since it uses CSS sintaxe (that I am really familiar with), it has asyncronoues requests that might also save some time decreasing the total time response while scraping the 5 pages.
* Since we have couple web searches available (Google, Bing, etc), I designed a superclass called WebSearcher then I implemented the Google one through inheritance. It is really straitfoward to add more.
* Concerning the top 5 pages, I defined a class called PageInsight responsible for gathering the relevant information.
* Moreover, I thought ahead that one can create your own PageInsight, since there are some well known pages, for example, StackOverFlow, that we might already know what we want there. The most relevant information there should be the answer with more votes and etc.  In summary, we can have a couple specific PageInsight (inheritance) classes related to pages we already know what is relevant.

#### Design the API itself

#### Add Redis

#### Dockerize the API

## Future Features
* Give to the user the possibility of re-doing the search for real, I mean, to scrap the web again


