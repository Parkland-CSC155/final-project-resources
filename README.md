#CSC 155 Final Project Resources and Specs
This page supplements the project requirements document by providing more specific detail, links, helpful examples, etc... to assist you with completing your project.

#SPECS
## JSON-based API
The JSON-based API should handle the following URL patterns: 

`/api/search/{searchText}?page={pageNumber}` 

returns an array of nutrition items matching the search parameters (broken into 25 record pages)

`/api/list?page={pageNumber}`

returns an ordered list of nutrition items broken into 25 record pages

`/api/{id}` 

returns a specific nutrition item by id

