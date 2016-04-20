#CSC 155 Final Project Resources and Specs
This page supplements the project requirements document by providing more specific detail, links, helpful examples, etc... to assist you with completing your project.

#SPECS
## JSON-based API
The JSON-based API should handle the following URL patterns: 

- `/api/search/{searchText}?page={pageNumber}&apiKey={apiKey}` 
returns an array of nutrition items matching the `searchText` (broken into 25 record pages)

- `/api/list?page={pageNumber}&apiKey={apiKey}`
returns an ordered list of nutrition items broken into 25 record pages

- `/api/{id}&apiKey={apiKey}` 
returns a specific nutrition item by id

- each request to an API URL should be inspected to contain a valid `apiKey`. It is up to you how you determine what a 'valid' `apiKey` is
  - your server should return a `401` status if no `apiKey` is present or the `apiKey` is invalid

## Web Application/Web Site
- A login page. Users must authenticate in order to gain access to the rest of the site pages. A great url for this would be `/login`
- A logout page. Something like `/logout`
- A list page of nutrition items ordered alphabetically, broken into 25 record pages.
  - A suggested URL would be: `/list`
  - users must be authenticated before going to this page
  - the page should support searching on the name of items
  - the page should have buttons to allow moving through the pages of data. [See Here for some good guidance](https://gist.github.com/brajeshwar/2802235)
- A page that allows viewing individual nutrition items with their full nutrition information
  - A suggested URL would be: `/details/{id}`
  - users must be authenticated before going to this page

## Calculator Area
The calculator area will effectively be another page in the website, but it will have additional functionality than most of the other pages, specifically in that it will: 
- Allow a user to search and select multiple nutrition items and have the page display the totaled nutrition facts
- Allow a user to select multiples of portion sizes and have the page display the totaled nutrition information
- A user must be authenticated to see the page
- a good example of functionality can be found [Here](http://www.myfitnesspal.com/recipe/calculator)
- A suggested URL for this area would be: `/calculator`
 
## Datasets and Databases
The instructor will upload datasets for a variety of database platoforms (typically in the form of a backup or an export). If you need a specific format, please reach out
- you will likely need additional tables in your database to support user authentication and or other features you decide to build. Do not hesitate to create those additional tables

# Tips and Resources
## Deploying to Azure Taking Forever?
Tired of waiting on your internet connection to upload the huge `node_modules` folder everytime you want to make a deployment. Here are a few tricks to speed it up. 
#### Run NPM Install on the Server __AFTER__ you copy your code
- This one requires use of the `DebugConsole` on Azure. Use FTP to upload all of your files _EXCEPT_ your `node_modules` folder. (thus cutting down your overall upload time significantly)
- Go to your `*.scm.azurewebsites.net` URL (see [Here](https://blogs.msdn.microsoft.com/cdndevs/2015/04/01/the-kudu-debug-console-azure-websites-best-kept-secret/) for more info)
- Go to `<your-site>.scm.azurewebsites.net/DebugConsole` and navigate to the `site\wwwroot` folder
- Run `npm install --production` on the command line in the browser window. This will install all the packages in your package.json for your app.

#### Use Git for Deployments
Another option for deployments is actually just using Git - [See HERE](https://azure.microsoft.com/en-us/documentation/articles/web-sites-publish-source-control/#Step4)
- When you do a Git deployment, Azure will automatically run `npm install` after each deployment so you don't have to!

## How does a team work together on a single code-base?
- The [GitHub Flow](https://guides.github.com/introduction/flow/) process is a proven system for multiple individuals to work together on a single codebase
- Here's some info on [Pull Requests](https://help.github.com/articles/using-pull-requests/) and [Branches](https://help.github.com/articles/creating-and-deleting-branches-within-your-repository/) if you would like to better understand them
- Heres some more [Info on collaborating in GitHub](https://help.github.com/categories/collaborating-on-projects-using-issues-and-pull-requests/)
