## Making an app that feeds on contracting data.
The goal of this API is to allow the creation of interactive applications that facilitate research and visualization of data regarding business people and government officials in Latin America, and that is why we will include a step-by-step guide on how to create an application using JavaScript and the client's API [node-qqw] https://github.com/ProjectPODER/node-qqw/).

This tutorial is not an introduction to nodejs, but rather a step sequence and some considerations, in order to be able to have an easily extensible application, based on this API.

### Starting our project, choose your GIF

When we start projects in PODER, we do it choosing a name and an animated GIF to give identity to the project. Even when this will not be published, its purpose is to have a fast and easy way to communicate to the whole team its goal.

For this tutorial we will make a project to download the 100 biggest contracts of 2012, and to tell which companies were benefited the most. We will call it 'baktun'. In 2012, according to some bad interpretations of Mayan mythology, was supposedly the year the world would end, or at least it would usher a new era. Since this is a test project, there's no need for it to make sense, but your project can be meaningful. If you're looking for inspiration, you can check our [research] pages(https://www.quienesquien.wiki/investigaciones) and [tools](https://www.quienesquien.wiki/herramientas).

For our project, we will use the git as a handler of code versions on Ubuntu Linux, so we will create a new folder and start the git repository like this:

```
mkdir baktun
cd baktun
git init
```

Once we start our project,  we have to create our ‘package.json’ file. This will have as dependency the node-qqw library.

package.json:
```
{
  "name": "baktun-test",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "~4.16.1",
    "express-handlebars": "^3.0.2",
    "qqw": "git+https://github.com/ProjectPODER/node-qqw/#v0.2.1",
  }
}

```

Note: the node-qq package is not published yet on the npm repository, so it's necessary to add it via GitHub. We’ll also include the express framework to make easier web page creation.

Then we must create our application's main file, called 'app.js'. In this file, we will include the API and make queries, then we will use the results to generate a web page with a list of the most benefited businesses this year, linked to Quienesquien.wiki to see the full data.

app.js
```
var express = require('express');
var hbs = require('express-handlebars');

var indexRouter = require('./routes/index');


app.engine('.hbs', hbs({
    extname: 'hbs',
    defaultLayout: 'layout',
    layoutsDir: path.join(__dirname, 'views')
));

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'hbs');

app.use('/', indexRouter);

module.exports = app;


```

In our code, we have established that we will use the `routes/index.js` to define the content of our application, and it will be there where we'll make the QuienEsQuien.wiki API call. We have also established that we will use the `views` folder to store our handlebar templates that will be used to generate the HTML that we will send to our users' browsers.

### Creating our main page

In this page, we will show the main history of our site, so we will need to query the QuienEsQuien.wiki API to get the information. We will quickly review the API, but for more in-depth information, please see the section [How to query] of this guide.

The API allows to use several filters, limits, and data sorting. The data can also be restricted to just the fields we need. In this case, we have some criteria:

* Contracts executed during 2012 using date filters.
* Sort the contracts in ascending order using `sort`.
* Limit the search to 100 elements using `limit`.
* (optional) Restrict the contents of the result using `fields`.

Also, don't forget that QuienEsQuien.wiki website features an easy way to make queries to the API from the 'Tools' menu, on the search page. This means that any search we make in QuienEsQuien.wiki will be made through the API, so we can make filters visually, and then copy them.

#### Querying QuienEsQuien.wiki

We must first make a query in QuienEsQuien.wiki in order to get an API URL to base our query on. For this, we will get in the [contracts search engine](https://www.quienesquien.wiki/contratos).

##### Date Filter
On the side bar, we can find the date filters. We will set the first one to January 1st, 2012; and the second one to December 31st, 2012. Upon clicking on the field, a calendar will pop up. It's not necessary to use it as long as the date entered is valid. If you choose to use it, clicking on the year will make a drop-down menu appear, allowing to easily choose year. Don't forget to choose month and day to complete the filter setup.

Click on the 'filter' button on the side bar to finish.

##### Number of results
Even though the API allows us to choose the number of results on each page, the QuienEsQuien.wiki interface has a drop-down on the top bar of the search page, where we can choose 25, 50, or 100 results per page. We will choose 100.

##### Copying the API URL
On the top bar there's the tools menu, which displays several options. Among them, we will see the "Copy URL" button, which will store the queried API URL in our computer's clipboard.
In our case it will look like this:

``` https://api.quienesquien.wiki/v2/contracts?sort=-compiledRelease.total_amount&compiledRelease.contracts.period.startDate=%3E2012-01-01T00%3A00%3A00.000&compiledRelease.contracts.period.endDate=%3C2012-12-31T00%3A00%3A00.000&limit=100
```

Let's take a quick look:

* First, there's the API URL base: `https://api.quienesquien.wiki/v2/`
* Next, entity type: `contracts`
* Then, the sort criteria: `sort=-compiledRelease.total_amount`. note the dash, meaning a minus sign, indicating a descending sorting, and then the field name. This field is the total amount for each contracting process `compiledRelease.total_amount`.
* Next we have the date filters, these will filter contracts which execution had started after the start date and finished before the finish date. Dates are expressed on standard ISO.
* Finally, we have the `limit` field on 100, which specifies the number of contracts we want.

What does the API return?

A list of contracts in the `data`object. Luckily, we won't have to make a system that analyzes the raw API result, but instead we will use an API client.


### The node-qqw API client

node-qqw is a npm package that you can include in your projects, available on [github](https://github.com/ProjectPODER/node-qqw/).

To make an API call from our `baktún` project, what we need to do is to includ the module first.

```
function getApi() {
  let Qqw = require('qqw');
  var client = new Qqw({rest_base: process.env.API_BASE});
```

Then we have to build the filters object we will send:
```
  const params = {
    sort: "-compiledRelease.total_amount",
    "compiledRelease.contracts.period.startDate" ">2012-01-01T00%3A00%3A00.000",
    "compiledRelease.contracts.period.endDate": "<2012-12-31T00%3A00%3A00.000",
    "limit": 100
  }
```
Then we will make the query. There are several ways to make the query, but in this case we are using promises with `await`, even though we could also use `then`.

```
  result = await client.get_promise(collection, params);
  return result;
}
```

Finally, we must create a path and pass the information on to our template:

```
function homePage(res,req) {
  result = getApi();
  res.render("index", {result: result});

}

router.get('/', homePage());
```

And there we go, we have a site that is working, we only need to set up our template and test it.

In `views/index.hbs`:
```
<h1>Baktún</h1>
<h2>Listado de los 100 contratos más caros de 2012.</h2>
{{#if result.data.0.records.length }}
  {{#each result.data.0.records}}
    {{#each this.awards}}
    Contract: Supplier(s): {{this.suppliers}} Tittle: {{this.title}} Value: {{this.value.amount}} {{this.value.currency}}
    {{/each}}
  {{/each}}
{{/if}}
```

To test it, we must execute `npm run start`.

### Find out which provider was the most benefited

We now have all the contracts information, but our goal was to sum all the amounts of each provider and rank them. For this we have to process the contracts after they get to the API and before passing them on to the template.

We need to create a function that adds up amounts, groups them by provider and sends them to the template as another variable.

This tutorial is not yet finished, if you wish to use the API, please email us at info@quienesquien.wiki
