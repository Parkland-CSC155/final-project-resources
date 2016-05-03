## Paging Through Data
When you are building your HTML pages, you need to break up the data into "pages" of 25 records a piece. This requires building your SQL queries in a way that allows that to happen. On the HTML side, though, how should you create the paging links to navigate through this data?

#### First, Check Your Route(s)
Your HTML page routes on the server should support paging information through the querystring. The following example uses a page at the `/list` URL, but yours could be the home page or another URL you've decided on... 

- `/list` : just the first 25 pages
- `/list?searchText=abcd`: finds the first 25 records matching the search text
- `/list?page=3`: finds records 51-75 in your data
- `/list?page=3&searchText=abcd`: finds records that match the search text AND only returns the 51-75th records of those matches

__If your HTML page's route(s) support the `page` variable, then you can easily create URLs to point to different pages in your data!__

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
