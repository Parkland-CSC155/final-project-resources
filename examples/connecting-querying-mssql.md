#Connecting to and Querying MS SQL Server
If you've chosen to use Microsoft SQL Server, there are some tips to getting it to work easily in your app. First you should take a look at a `connection string` and get a basic understanding that is essentially a long string of configuration values lumped together that allow you to connect to a database. Most all major database platforms have a standardized way of constructing connection strings. SQL Server is no different. Let's look at one: 

#### The Connection String
`Driver={SQL Server Native Client 11.0};Server=tcp:some-server.database.windows.net,1433;Database=some-db;Uid=bob@some-server;Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;`

Let's break this out into its pieces (each one is delimited by a semi-colon): 
- `Driver={SQL Server Native Client 11.0};` : this tells the client what Driver to use (often not required)
- `Server=tcp:some-server.database.windows.net,1433;`: this tells what the server's host name is along with the port (1433) and protocol (tcp)
- `Database=some-db;`: what database on the server do you want to connect to
- `Uid=bob@some-server;`
- `Pwd={your_password_here};`
- `Encrypt=yes;`: do you want tcp traffic to be encrypted?
- `TrustServerCertificate=no;`
- `Connection Timeout=30;`: what is the connection timeout (in milliseconds) - often is good to bump to 30 full seconds

#### Connecting with the `mssql` NPM Package
In order to connect to a database server with the `mssql` package, you can use a connection string to make the connection

```javascript
router.get("/some-url", function (req, res, next) {

    // the environment variable for the connection string
    var connectionString = process.env.MS_TableConnectionString; 
    sql.connect(connectionString).then(function () {

        var qry = `SELECT  TOP 1 * FROM NutritionData`;

        return new sql.Request().query(qry).then(function(recordset) {
            console.log(recordset);
            
            res.json(record);
            // or you can pass the data to res.render() if you 
            // want it to go to an EJS view
        });

    })
    .catch(function(err){
      console.log(err);
      next(err);  
    });
});
```
However, sometimes folks will get the following error when they try to connect: 
`{"name":"ConnectionError","message":"Unknown driver SQL Server Native Client 11.0!","code":"EDRIVER"}`

In order to fix this, simply remove the `Driver={SQL Server Native Client 11.0};` portion from your connection string.

