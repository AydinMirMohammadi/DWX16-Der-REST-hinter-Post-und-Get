FORMAT: 1A
HOST: http://polls.apiblueprint.org/

# Customer.APIGateway
As a client programmer, be prepared that any time any error status code or redirect may be responded.

#Group APIGateway

# Customer.APIGateway API Root [/]

APIGateway defintion for a customer demo.

## Retrieve the Entry Point [GET]

+ Response 200 (application/vnd.siren+json)

    + Body

               {
                   "links" : [
                      {
                          "rel": ["CustomerApi"],
                          "href": "/customerapi"
                      }
                  ]
                }

# Group Customer

## Customer Resource [/customerapi]
Access to customers.

## Get CustomerApi meta object [GET]
+ Response 200 (application/vnd.siren+json)
    + Headers

    + Attributes (CustomerApiResponse)

## Route to single Customer [/customerapi/customers/{CustomerId}]
+ Conditional GET

+ Parameters
    + CustomerId: `123` (string, required) - ID of the customer

## Get Customer by ID [GET]
Gets one customer by ID.

+ Request 200

    + Headers

            If-None-Match: etag-hashexample
            Accept: application/vnd.siren+json
            
+ Response 200 (application/vnd.siren+json)

    + Headers
    
            ETag: etag-hashexample
    
    + Attributes (CustomerResponse)


+ Request 304 Not Modified

    + If-None-Match - For this request a version has to be used which is equal to the etag of the requested branch

    + Headers

            If-None-Match: etag-hashexample
            Accept: application/vnd.siren+json


+ Response 304

+ Request 404 Not Found

    Using an Customer ID in the URL that does not exist.

    + Headers

            Content-type: application/vnd.siren+json

+ Response 404

## Queries for Customers [/customerapi/queries{?PageSize,PageOffset,SortBy,Filter}]

The resource supports

* Conditional GET
* Paging: the response is split into pages of data

This route should not be accessed directly, but by posting a query and navigating to the result url.

+ Parameters

    + PageSize: 20 (number, optional) - The number of Customers per page. The maximum value is 100.
        + Default: 20

    + PageOffset: 0 (number, optional) - The number of the Customer which will be the first in the result list.
        + Default: 0

    + SortBy: 0 (string, optional) - A list of propertys by which the result list is sorted. "+" indicates ascending "-" descending
        + Default: 0

    + Filter (string, optional) - Filter collection by Customer properties

## Access a query for Customers [GET]
Gets the list of Customer. Only the Customer that are accessible for the user specified by the authorization header are returned. NOTE: if the result set is empty, an empty collection is returned with a normal 200 HTTP code.

+ Request 200

    + Headers

            Accept: application/vnd.siren+json

+ Response 200 (application/vnd.siren+json)

    + Headers

    + Attributes (CustomerQueryResponse)


## Access to the NewQuery action for Customers [/customerapi/NewQuery]
## Post a new query [POST]
+ Request 201

    + Headers

            Content-type: application/json

    + Body

            {
                  "PageSize": "30",
                  "PageOffset": "2",
                  "SortBy": [
                    "+ZipCode"
                  ],
                  "Filter": [
                    {"Country": "DE"}
                  ]
            }

+ Response 201
    + Header

            Location: http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30&PageOffset=2&SortBy=%5B%22%2BZipCode%22%5D&Filter=%5B%22Country%22%2C%22DE%22%5D

+ Request 400
    + Headers

            Content-type: application/json
+ Response 400
    + Headers

            Content-type: application/problem+json

    + Attributes (Problem)

## Access to the NewCustomer action for Customers [/customerapi/NewCustomer]
## Trigger the creation of a new. [POST]
+ Request 202

    + Headers

            Content-type: application/json

    + Body

            {
                  "Name"="Maria Reinhart"
            }

+ Response 202

+ Request 400
    + Headers

            Content-type: application/json

    + Body

            {
                  "Name"="Maria Reinhart"
            }

+ Response 400
    + Headers

            Content-type: application/problem+json

    + Attributes (Problem)

## The Move action of a Customer [/customerapi/customers/{CustomerId}/Move]

+ Parameters

    + CustomerId: `122` (string, required)

## Trigger Move action on a Customer [POST]
+ Request 202

    + Headers

            Content-type: application/json

    + Body

            {
                  "Street" : "Blaueaue 17",
                  "City" : "Hamburg",
                  "Country" : "DE"
            }


+ Response 202

+ Request 400
    + Headers

            Content-type: application/json

+ Response 400
    + Headers

            Content-type: application/json

    + Attributes (Problem)

# Data Structures

## Problem (object)
+ Title: `This is bad` (string, required)
+ Status: `400` (number, required)
+ Detail: `Things went wrong, the cat did it.` (string, required)

## CustomerApiResponse (object)
+ class (array, required)
    + `CustomerApi`
    + `QueriableList`
+ rel (array, required)
    + `customerapi`
+ properties (object, required)

+ links (array, required)
    + (object, required)
        + rel: `self` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi`(string, required)
    + (object, required)
        + rel: `all` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30`(string, required)

+ actions (array, required)
    + (CustomerApiNewQueryAction)
    + (CustomerApiNewCustomerAction)

## CustomerApiNewQueryAction (object)
+ name: `NewQuery` (string, required)
+ title: `A query for this collection` (string, required)
+ method: `POST`(string, required)
+ href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/NewQuery` (string, required)
+ type: `application/json` (string, required)
+ fields (array, required)
    + (object, optional)
        + name: `PageSize` (string, required)
        + type: `number`(string, required)
        + value: `30`(string, optional)
    + (object, optional)
        + name: `PageOffset` (string, required)
        + type: `number`(string, required)
        + value: `1`(string, optional)
    + (object, optional)
        + name: `SortBy` (string, required)
        + type: `http://private-56854-customerdemo.apiary-mock.com/customerapi/types/CustomerSortProperty[]`(string, required) - this url is only used as unique namespace so far
    + (object, optional)
        + name: `Filter` (string, optional)
        + type: `http://private-56854-customerdemo.apiary-mock.com/customerapi/types/CustomerFilterProperty[]`(string, optional)

#CustomerApiNewCustomerAction
+ name: `NewCustomer` (string, required)
+ title: `Create a new customer` (string, required)
+ method: `POST`(string, required)
+ href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/NewCustomer` (string, required)
+ type: `application/json` (string, required)
+ fields (array, required)
    + (object, optional)
        + name: `Name` (string, required)
        + type: `string`(string, required)
    + (object, optional)
        + name: `Street` (string, required)
        + type: `string`(string, required)
    + (object, optional)
        + name: `City` (string, required)
        + type: `string`(string, required)
    + (object, optional)
        + name: `ZipCode` (string, required)
        + type: `string`(string, required)
    + (object, optional)
        + name: `Country` (string, required)
        + type: `string`(string, required)

#MoveAction
+ name: `Move` (string, required)
+ title: `Move a customer to a new location` (string, required)
+ method: `POST`(string, required)
+ href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/customers/123/Move` (string, required)
+ type: `application/json` (string, required)
+ fields (array, required)
    + (object, optional)
        + name: `Street` (string, required)
        + type: `string`(string, required)
    + (object, optional)
        + name: `City` (string, required)
        + type: `string`(string, required)
    + (object, optional)
        + name: `Country` (string, required)
        + type: `string`(string, required)

##CustomerResponse
+ Include CustomerEntity1
+ actions (array, required)
    + (MoveAction)

##CustomerQueryResponse
+ class (array, required)
    + `CustomerQueryResponse`

+ properties (object, required)

+ links (array, required)
    + (object, required)
        + rel: `self` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30&PageOffset=2`(string, required)
    + (object, required)
        + rel: `first` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30&PageOffset=1`(string, required)
    + (object, required)
        + rel: `next` (array, optional)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30&PageOffset=3`(string, required)
    + (object, required)
        + rel: `pervious` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30&PageOffset=1`(string, required)
    + (object, required)
        + rel: `last` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/queries?PageSize=30&PageOffset=10`(string, required) - assuming 10 pages

+  entities (array, required)
    + (object, required)
        + Include CustomerEntity1

    + (object, required)
        + Include CustomerEntity2

##CustomerEntity1
+ class (array, required)
    + `Customer`
+ rel (array, required)
    + `customerapi-customer`
+ properties (object, required)
    + Id: `123` (string, required)
    + Name: `Frank Herbert` (string, required)
    + Street: `Wüstenallee 7` (string, required)
    + City: `Arakis` (string, required)
    + ZipCode: `77123` (string, required)
    + Country: `DE` (string, required)

+ links (array, required)
    + (object, required)
        + rel: `self` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/customers/123`(string, required)

##CustomerEntity2
+ class (array, required)
    + `Customer`
+ rel (array, required)
    + `customerapi-customer`
+ properties (object, required)
    + Id: `124` (string, required)
    + Name: `Luke Himmelsläufer` (string, required)
    + Street: `Zone 5` (string, required)
    + City: `Karlsruhe` (string, required)
    + ZipCode: `77124` (string, required)
    + Country: `DE` (string, required)

+ links (array, required)
    + (object, required)
        + rel: `self` (array, required)
        + href: `http://private-56854-customerdemo.apiary-mock.com/customerapi/customers/124`(string, required)
