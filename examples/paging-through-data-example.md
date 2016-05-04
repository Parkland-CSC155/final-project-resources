## Paging Through Data
When you are building your HTML pages, you need to break up the data into "pages" of 25 records a piece. This requires building your SQL queries in a way that allows that to happen. On the HTML side, though, how should you create the paging links to navigate through this data?

#### First, Check Your Route(s)
Your HTML page routes on the server should support paging information through the querystring. The following example uses a page at the `/list` URL, but yours could be the home page or another URL you've decided on... 

- `/list` : just the first 25 pages
- `/list?searchText=abcd`: finds the first 25 records matching the search text
- `/list?page=3`: finds records 51-75 in your data
- `/list?page=3&searchText=abcd`: finds records that match the search text AND only returns the 51-75th records of those matches

__If your HTML page's route(s) support the `page` variable, then you can easily create URLs to point to different pages in your data!__

#### How to query "pages" of data
When querying a database, we refer to "pages" of data as being slices in a larger set of records that can't fully give to the user at once because the whole set of records is too large. If we have 8000+ records in a database table, we might like to slice that up into 25 record "pages" and allow the user to click through each page of data to find what they are looking for. 

__So how do we write a query to do that?__

Most database platforms have support for this very exact scenario. Each platform is a bit different, but for the most part it ends up being some syntax you add to your query to retrieve one of these slices of the entire recordset (the "page"). First and foremost before you start writing the SQL, you'll need to figure out a way to __SORT__ the data for the query. You can't effectively page through a large set of records until you know what order the slices should be sorted in. 

- SQL Server: [OFFSET and FETCH NEXT](http://www.dofactory.com/sql/order-by-offset-fetch)
- Postgres & MySQL: [LIMIT and OFFSET](http://www.petefreitag.com/item/451.cfm)


#### The Paging Links
So if you want to create paging buttons near a table of data, you can simply do something like the following (assuming you're on page 3): 

```html
<a href="/list?page=2">Prev</a>

<a href="/list">1</a>
<a href="/list?page=2">2</a>
<a href="/list?page=4">4</a>
<a href="/list?page=5">5</a>

<a href="/list?page=4">Next</a>
```
These links will just re-load the current page but with a different "page" of your data. 

#### Bringing this together with EJS
If you want to have dynamic paging buttons within your EJS view, then you should do something like: 
```javascript
// route
app.get("/list", function(req, res){

  var page = req.query.page || 1;
  page = Number(page); // in case it is a string, we need it to be a number

  // database querying logic

  res.render('list', {
    // other data 
    page: page // pass what page we're on to the view
  });
  
});
```
```html
<!-- The EJS View -->

<a href="/list?page=<%= (page - 1) %>">Prev</a>

<a href="/list?<%= (page - 2) %>"><%= (page - 2) %></a>
<a href="/list?<%= (page - 1) %>"><%= (page - 1) %></a>
<a href="/list?<%= (page + 1) %>"><%= (page + 1) %></a>
<a href="/list?<%= (page + 2) %>"><%= (page + 2) %></a>

<a href="/list?<%= (page + 1) %>">Next</a>
```

