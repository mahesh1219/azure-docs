---
title: "Quickstart: Send a search request to the REST API using Ruby - Bing Entity Search"
titleSuffix: Azure Cognitive Services
description: Use this quickstart to send a request to the Bing Entity Search REST API using Ruby, and receive a JSON response.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: mode-api
---

# Quickstart: Send a search request to the Bing Entity Search REST API using Ruby

> [!WARNING]
> Bing Search APIs are moving from Cognitive Services to Bing Search Services. Starting **October 30, 2020**, any new instances of Bing Search need to be provisioned following the process documented [here](/bing/search-apis/bing-web-search/create-bing-search-service-resource).
> Bing Search APIs provisioned using Cognitive Services will be supported for the next three years or until the end of your Enterprise Agreement, whichever happens first.
> For migration instructions, see [Bing Search Services](/bing/search-apis/bing-web-search/create-bing-search-service-resource).

Use this quickstart to make your first call to the Bing Entity Search API and view the JSON response. This simple Ruby application sends a news search query to the API, and displays the response. The source code for this application is available on [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/ruby/Search/BingEntitySearchv7.rb).

Although this application is written in Ruby, the API is a RESTful Web service compatible with most programming languages.

## Prerequisites

* [Ruby 2.4](https://www.ruby-lang.org/en/downloads/) or later.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## Create and initialize the application

1. In your favorite IDE or code editor, create a news Ruby file and import the following packages:

    ```ruby
    require 'net/https'
    require 'cgi'
    require 'json'
    ```

2. Create variables for your API endpoint, News search URL, your subscription key, and search query. You can use the global endpoint in the following code, or use the [custom subdomain](../../../cognitive-services/cognitive-services-custom-subdomains.md) endpoint displayed in the Azure portal for your resource.
    
    ```ruby
    host = 'https://api.bing.microsoft.com'
    path = '/v7.0/search'
    
    mkt = 'en-US'
    query = 'italian restaurants near me'
    ```

## Format and make an API request

1. Create the parameters string for your request by appending your market variable to the `?mkt=` parameter. Encode your query and append it to the `&q=` parameter. Combine your API host, path, and the parameters for your request, and cast them as a URI object.

    ```ruby
    params = '?mkt=' + mkt + '&q=' + CGI.escape(query)
    uri = URI (host + path + params)
    ```

2. Use the variables from the last step to create the request. Add your subscription key to the `Ocp-Apim-Subscription-Key` header.

    ```ruby
    request = Net::HTTP::Get.new(uri)
    request['Ocp-Apim-Subscription-Key'] = subscriptionKey
    ```

3. Send the request, and print the response.

    ```ruby
    response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
        http.request (request)
    end

    puts JSON::pretty_generate (JSON (response.body))
    ```

## Example JSON response

A successful response is returned in JSON, as shown in the following example: 

```json
{
  "_type": "SearchResponse",
  "queryContext": {
    "originalQuery": "italian restaurant near me",
    "askUserForLocation": true
  },
  "places": {
    "value": [
      {
        "_type": "LocalBusiness",
        "webSearchUrl": "https://www.bing.com/search?q=sinful+bakery&filters=local...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "https://www.contoso.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98112",
          "addressCountry": "US",
          "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
      },

      . . .
      {
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Pickles+and+Preserves...",
        "name": "Munson's Pickles and Preserves Farm",
        "url": "https://www.princi.com/",
        "entityPresentationInfo": {
          "entityScenario": "ListItem",
          "entityTypeHints": [
            "Place",
            "LocalBusiness",
            "Restaurant"
          ]
        },
        "address": {
          "addressLocality": "Seattle",
          "addressRegion": "WA",
          "postalCode": "98101",
          "addressCountry": "US",
          "neighborhood": "Capitol Hill"
        },
        "telephone": "(800) 555-1212"
      },
      
      . . .
    ]
  }
}
```

## Next steps

> [!div class="nextstepaction"]
> [Build a single-page web app](../tutorial-bing-entities-search-single-page-app.md)

* [What is the Bing Entity Search API?](../overview.md)
* [Bing Entity Search API reference](/rest/api/cognitiveservices-bingsearch/bing-entities-api-v7-reference).
