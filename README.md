#CSC 155 Final Project Resources and Specs
This page supplements the project requirements document by providing more specific detail, links, helpful examples, etc... to assist you with completing your project.

#SPECS
## JSON-based API
The JSON-based API should handle the following URL patterns: 

- `/api/search/{searchText}?page={pageNumber}` 
returns an array of nutrition items matching the `searchText` (broken into 25 record pages)

- `/api/list?page={pageNumber}`
returns an ordered list of nutrition items broken into 25 record pages

- `/api/{id}` 
returns a specific nutrition item by id

##Web Application/Web Site
- A login page. Users must authenticate in order to gain access to the rest of the site pages
- A list page of nutrition items ordered alphabetically, broken into 25 record pages
  - the page should support searching on the name of items
  - the page should have buttons to allow moving through the pages of data. [See Here for some good guidance](https://gist.github.com/brajeshwar/2802235)
- A page that allows viewing individual nutrition items with their full nutrition information
