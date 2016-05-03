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
  - A suggested URL would be: `/list` (or you can make it the home page too by using the `/` route)
  - users must be authenticated before going to this page
  - the page should support searching on the name of items
  - the page should have buttons to allow moving through the pages of data. [See Here for some good guidance](https://gist.github.com/brajeshwar/2802235)
  - this page should support URLs similar to your JSON-based API, but without the `/api` route prefix
    - `/list?page=3`
    - `/list?page=5`
  - Since you should only be able to access this page after logging in, you should not need an API key with any of the requests
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
## How to Debug Your Azure Web App Instance (or see why its not working...)
Are you running into an issue where your application is working on your machine, but after you deploy it to Azure, it either won't respond or responds with errors? You would probably like to know what errors are happening on the server so that you can fix those, right? Azure offers a [Web App Administration Tool](http://www.jamessturtevant.com/posts/How-to-add-edit-and-remove-files-in-your-azure-webapp-using-the-kudu-service-dashboard/) called `KUDU` to see what's going on under the covers of your website, but before you dive into that, first think through some logical questions: 
- Are you using any absolute file paths in your application that won't work when you upload it to a server?
- Did you upload your entire `npm_modules` folder, or did it only get partially uploaded? If that's taking forever, see below.
- Is your `web.config` file pointing to the correct file that is the "main entry point" for your application? (app.js, index.js, server.js, etc...)

If you've double checked those points, then it's time to use [KUDU](http://www.jamessturtevant.com/posts/How-to-add-edit-and-remove-files-in-your-azure-webapp-using-the-kudu-service-dashboard/). Simply take your application's Azure URL and stick `.scm.` after the subdomain: 
- https://my-project.azurewebsites.net => https://my-project.scm.azurewebsites.net
- Go to "Debug Console > CMD" 
- Go to the "Logs > Application" folder (if you don't have a logs folder, then turn on [Diagnostic Logging](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)
- To view the contents of a file in the folder, simply click the pencil icon to read the contents. 
- Typically a file called something like `logging-errors.txt` will have your most recent node errors at the bottom of the file (just look at the timestamps)
- Usually you can see what the error message is, and then make your fixes accordingly and re-upload. 

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

## How should we work with our Database?
One important part of the project is that you have to connect to and query a separate database server (not your local SQLite database). So how should you do that during the development process? Well you have a few options: 

1. Create your cloud database server and then just work directly against that. 
2. Work locally with a database on your computer, and then have your code switch to the cloud database server after deployment.

#### 1 - Work directly against your cloud database server. 
If you've created an MSSQL, Postgres, or MySQL server to work with in the cloud, then you should have either a `connectionstring` or some credentials to connect to that database. Simply just use that locally. You'll obviously need a network connection to connect to your cloud database, but you'll also need to [tell your cloud database that you are not a hacker](https://azure.microsoft.com/en-us/documentation/articles/sql-database-configure-firewall-settings/)

#### 2 - Work locally with SQLite then switch to MSSQL or some other db platform.
In order to easily switch between a local SQLite database and a production database, you need to use a library that can handle the different ways of interacting with different databases. One such popular library is [Sequelize](http://docs.sequelizejs.com/en/latest/docs/getting-started/). It supports [raw SQL queries](http://docs.sequelizejs.com/en/latest/docs/raw-queries/) and provides a way of flipping between SQLite and MSSQL. 
