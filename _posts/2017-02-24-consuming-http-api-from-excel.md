---
layout: post
title: Consuming an http api from excel
---

I've been looking into how to consume an http api from excel. Mainly there are two alternatives:

* Data as json array
* OData

## Data as json array ##

- Pros:
    * Api really easy to develop.
- Cons:
    * No server side filtering.
    * Excel setup a bit harder.


The simple and easy way is just to return all your data from your endpoints as json format inside an array.

{% highlight javascript %}
[
  {
    "Metric": "Service Revenues",
    "Region": "Middle East and Africa",
    "Sub Region": "Africa",
    "Country": "Egypt",
    "Company": "",
    "Service Type": "Service Total",
    "Level": "",
    "Customer": "Total",
    "Mar-15": "1,161.47",
    "Jun-15": "1,393.76",
    "Sep-15": "1,277.61",
    "Dec-15": "1,626.05",
    "2015": "5,458.89",
    "2016": "5,714.91",
    "2017": "5,864.41",
    "2018": "5,999.11",
    "2019": "6,135.24",
    "2020": "6,262.13"
  },
  {
    "Metric": "Active Users",
    "Region": "Middle East and Africa",
    "Sub Region": "Africa",
    "Country": "Ghana",
    "Company": "",
    "Service Type": "Service Total",
    "Level": "",
    "Customer": "Total",
    "Mar-15": "275.33",
    "Jun-15": "330.4",
    "Sep-15": "302.86",
    "Dec-15": "385.46",
    "2015": "1,294.05",
    "2016": "1,368.94",
    "2017": "1,423.63",
    "2018": "1,470.65",
    "2019": "1,519.23",
    "2020": "1,567.26"
  },

  ...
]
{% endhighlight %}

Then you can just from excel point to that endpoint using [power query](https://support.office.com/en-us/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605).

To do that just open excel and go to *Data -> New Query -> From Other Sources -> From Web*. You will get a form with and input for the url. Specify the url of your endpoint and excel will fetch the data from it and come back with a list of records.

![power query list of records]({{ site.url }}/assets/power-query-1.png)

Now you have to convert this to a table click the *To Table* icon.
After that click in the expand icon to get all the properties of the elements in your json (I recommend deselecting *Use original column names as prefix*).
As a consecuence of that you should see all your data in the table.
Finally if you click the icon "Close & Load" that table will be copied into your spreadsheet.

You should get a table similar to this:

![power query table]({{ site.url }}/assets/power-query-2.png)

*By the way, I am using excel 2016. I'm not sure if this works with earlier versions.*

As you can see there are a few steps involved, but is not that bad.

Once you get your table in the spreadsheet you can filter it, create a pivot table from it or do whatever you will do with excel. 

If you want to refresh the data you can just click the refresh icon next to the query or *Refresh All* inside the *Data* ribbon.

You can also customise your query as much as you want in edit mode. Remove columns, rename them, reorder them, filter..

In summary: It's quite powerful and easy to setup. You just need a json api to point to and follow a few steps.


## OData ##

- Pros:
    - Excel setup really easy.
    - Server side filtering.
- Cons:
    - Api harder to develop.

# What is OData? #

From [wikipedia](https://en.wikipedia.org/wiki/Open_Data_Protocol):
> Open Data Protocol (OData) is an open protocol which allows the creation and consumption of queryable and interoperable RESTful APIs in a simple and standard way.

Basically odata defines a standard way of comunication between client and servers. It is based in [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) princpiles and it adds a query syntax on top. If your api follows the OData standard it means that your clients using odata will know how to get the data, navigate through it, filter it, order it and more.

You can find more info and documentation in the [odata website](http://www.odata.org/).

OData was created by Microsoft, so excel's power query plays quite well with it.

# How to use it from excel? #

In excel go to *Data -> New Query -> From Other Sources -> From Odata Feed* and input the url to your odata endpoint. You can use the example service from odata.org: `http://services.odata.org/TripPinRESTierService`. 
This endpoint uses version 4  of OData, which is the latest at the moment.

Excel will recognise all the resources available and it will give you the option to convert any of them to a table.

![power query odata setup]({{ site.url }}/assets/power-query-odata-1.png)

Once you pick one and click *Load* it will be in your spreadsheet. 

![power query odata table]({{ site.url }}/assets/power-query-odata-2.png)

From now on everything is the same as before. You can filter, sort, create pivot tables, customise your query.. in esence is power query, so you should get the same capabilities.

But there is a big benefit: you can do **server side filtering** from excel (only with OData v4).

If you go to edit the query and filter one of the columns excel will use an odata based filter when calling the backend. 

So for example if we edit the query and filter the column *LastName* for *Bright*.

![power query odata filtering]({{ site.url }}/assets/power-query-odata-3.png)

This is what it gets fired to the backend each time we refresh the view:
	
~~~~
services.odata.org/TripPinRESTierService/People?$filter=LastName%20eq%20'Bright'
~~~~

This is quite usefull when you have a lot of data in your backend and you don't want to pull all of it every time you want to refresh it.

# How much do I need to do in the api? #

The downsite is that developing an odata based http api for excel is a bit more complicated. 

First, all the endpoints have to specify the odata version in the response headers. If not excel doesn't work. E.g. "OData-Version: 4".

Second, you have to define the shape of the data for each of your resources in xml format under /$metadata. For example in our previous example we will have something like this:

**/$metadata**
~~~
<edmx:Edmx>
<edmx:DataServices>
<Schema xmlns="http://docs.oasis-open.org/odata/ns/edm">
    <EntityType Name="row" Namespace="models">
        <Property Name="Metric" Type="Edm.String"/>
        <Property Name="Region" Type="Edm.String"/>
        <Property Name="Sub Region" Type="Edm.String"/>
        <Property Name="Country" Type="Edm.String"/>
        <Property Name="Company" Type="Edm.String"/>
        <Property Name="Service Type" Type="Edm.String"/>
        <Property Name="Level" Type="Edm.String"/>
        <Property Name="Customer" Type="Edm.String"/>
        <Property Name="2015" Type="Edm.String"/>
        <Property Name="2016" Type="Edm.String"/>
        <Property Name="2017" Type="Edm.String"/>
        <Property Name="2018" Type="Edm.String"/>
    <EntityContainer Name="Container">
        <EntitySet Name="rows" EntityType="models.row"/>
    </EntityContainer>
</Schema>
</edmx:DataServices>
</edmx:Edmx>
~~~

From every endpoint we will have to reference that metadata with the property `@odata.context`. 

Next, we will have to define a root resource that is going to point all the available resources. This is know as the *service root* in odata terms. Luckily odata allows xml and json format for everythting other than the metadata, so let's use json:

**/**
~~~
{
    @odata.context: "http://localhost/$metadata",
    value: [
        {
            name: "rows",
            kind: "EntitySet",
            url: "rows"
        }
    ]
}
~~~
Unfortunately the link to the metadata just works with absolute paths, if you use the relative path excel will give you an error.

Finally, we need the actual resource:

**/rows**

~~~
{
    @odata.context: "http://localhost/$metadata#rows",
    value: [    
        {
            "Metric": "Service Revenues",
            "Region": "Middle East and Africa",
            "Sub Region": "Africa",
            "Country": "Egypt",
            "Company": "",
            "Service Type": "Service Total",
            "Level": "",
            "Customer": "Total",
            "Mar-15": "1,161.47",
            "Jun-15": "1,393.76",
            "Sep-15": "1,277.61",
            "Dec-15": "1,626.05",
            "2015": "5,458.89",
            "2016": "5,714.91",
            "2017": "5,864.41",
            "2018": "5,999.11",
            "2019": "6,135.24",
            "2020": "6,262.13"
        }

        ...
        ]
}

~~~
We append the resource name at the end of the link to the metadata with the '#' sing.

The actual data goes into `value`, there is nothing special about it.

There is much more than this in OData, but so far this is just what we need in order to make excel work. I would even say that if we make the backend able to understand the odata filtering syntax that should be enough. 

I would start small and just do this and implement the filtering in the backend. There is sorting in OData, but I don't think you would need it since you can sort in the spreadsheet itself. Or you should be able to navigate to each resource in the list with OData, but you are not going to need that in excel.
Still this would meand that you that you are not following the standard entirely and could lead to unexpected behaviours for your clients. So be aware of that. As an alternative you can also use one the odata libraries available out there.

In summary, odata is quite powerful, is super easy for excel users to use our apis and it more importantly server side filtering comes out of the box. But with great power comes great responsibility, it means that you will need to do extra work in the backend.

# OData with aws lambdas #

If you are using OData with amazon lambdas you will get to the issue that AWS ApiGateway doesn't allow '$' sign in the resource path -  `Resource's path part only allow a-zA-Z0-9._- and curly braces at the beginning and the end.`. So you won't be able to use "$metadata". The workaround to this would be simply to rename it to something like "metadata" and change the response body's "@odata.context" to point to /metadata.

## OData or no Odata ##

I would say that if you have a huge collection of data and if you are already going to implement filtering go for OData. Instead of creating your own filtering syntax (e.g ?filter1='value1'&filter2='value2') you could follow the [Odata standard for filtering](https://msdn.microsoft.com/en-us/library/hh169248(v=nav.90).aspx). Or you could use one of the available libraries.

If you are just not going to expose too much data and you are not going to do server side filtering, then just go for simple plain json endpoint and teach your users how to access to it through excel. It's not that complicated anyway, I bet any excel user could get around it quite easily.

## Other alternatives ##

As far as I know, the other alternative would be to create our own excel document with custom vb scripts that will fetch the data from the api and convert it to the format we need it. Then we would need to mantain and hand this excel document to the customers every time we change the api. Or if we have customer who know how to program could write the vb scripts themselves.

I see this as the worst option from all. The only upside of this approach is that it gives us a lot of flexibility. But the maintainability and deployment of the spreadsheet aspect could be quite painful.

## Other clients: Power BI ##

Power BI is a microsoft tool for creating visual graphs. Both excel's power query and Power BI have some similiarties, but Power BI is better for creating graphs. 
I have been experimenting with Power BI a bit and the data import part is the same as excel's power query. Probably they share the same code. So for Power BI you can import data both from OData or just from a Json based endpoint.
