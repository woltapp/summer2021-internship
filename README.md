# Wolt Summer 2021 Internships

**Preliminary Assignment for Engineering Positions**

Welcome! We are delighted to see you applying. Now is your time to shine. 

## In short
- Select either the frontend **OR** backend version of this assignment. 
- Write some code. When you are ready, leave us a frontend application **[here](https://wolt.com/en/jobs/posting/743999731488093)** or a backend application **[here](https://wolt.com/en/jobs/posting/743999731487668)**. Put your code (as a zip) to Google Drive, Dropbox, OneDrive (or similar) and send us a link in the application form. **Remember to check permissions!** If we cannot access the file, we cannot review your code. 
- That's it! We will get back to you.

## Overview
In 2017 we added a new view to the Wolt App, known as *Discovery*. The view mixes curated and automatically generated content, like banners, articles, videos and lists (e.g. *“Popular restaurants”, “New restaurants”*). Discovery is customized for each user based on their location, personal preferences and order history.

In this assignment you get to follow in the footsteps of Wolt developers and create a Discovery page, although a much simplified version (we don’t want you to spend hundreds of hours on this task 😀).

In the backend version you will generate *new / popular / nearby* restaurant lists from the given data by taking the location of a customer into account. The frontend task is about rendering such content as horizontal carousels. You will also get to use one of our popular open source libraries, [Blurhash](https://blurha.sh/) in the frontend version.

It should take about 4-8 hours to complete this assignment. However, the best way to make your assignment really stand out is to **finish it with care** - the last 10% is often the most important part of any software project.
 
 
## Restaurant-object
Both frontend and backend tasks use restaurant objects which represent **fictive** restaurants in Helsinki.  Each object has a set of fields providing more information about the restaurant, like *name* and *location*.

Example:
```
{
   "blurhash":"UAPp-JsCNbr[UQagn*V^p-bYjIjtL?kSo]bG",
   "location":[
      24.933257,
      60.171263
   ],
   "name":"Charming Cherry House",
   "online": true,
   "launch_date":"2020-09-20",
   "popularity":0.665082352909038
}
```


### Fields:
- **blurhash**: image representation (type: string)
- **location**: Restaurant's location as latitude and longitude coordinates. First element in the list is the longitude (type: a list containing two decimal elements)
- **name**: The name of the restaurant (type: string)
- **launch_date**: the date when the restaurant was added to Wolt app (type: string, ISO 8601 date)
- **online**: if *true*, the restaurant is accepting orders. If *false*, the restaurant is closed (type: boolean)
- **popularity**: the higher the number, the more popular the restaurant is in Wolt app (type: a float between 0-1, where 1 is the - most popular restaurant)

## Backend assignment
*restaurants.json* in the repository contains one hundred restaurants from the Helsinki area. 

Your task is to create an **API endpoint** */discovery* that takes coordinates of the customer as an input and then **returns a page (JSON response)** containing *most popular, newest and nearby* restaurants (based on given coordinates). 

Location of a customer needs to be provided as **request parameters** *lat* (latitude) and *lon* (longitude), e.g. */discovery?lat=60.1709&lon=24.941*. Both parameters accept float values.

An JSON object returned by the */discovery* -endpoint must have the following structure:
```
{
   "sections": [
      {
           "title": "Popular Restaurants",
           "restaurants": [.. add max 10 restaurant objects..]
      },
      {
           "title": "New Restaurants",
           "restaurants": [..add max 10 restaurant objects..]
      },
 	{
           "title": "Nearby Restaurants",
           "restaurants": [.. add max 10 restaurant objects..]
      }

   ]
}
```

For each *restaurants*-list you need to add **maximum 10** restaurant objects. A list can also contain fewer restaurants (or even be empty) if there are not enough objects matching given conditions. A section with an empty *restaurants*-list should be removed from the response.

**So how do you know which restaurants to add to each list?** 

There are two main rules to follow:
- All restaurants returned by the endpoint must be **closer than 1.5 kilometers** from given coordinates, measured as a straight line between coordinates and the location of the restaurant.
- Open restaurants (*online=true*) are **more important** than closed ones. Every list must be first populated with open restaurants, and only adding closed ones if there is still capacity left.

In addition each list has a specific **sorting rule**:
- “Popular Restaurants”: highest *popularity* value first (descending order)
- “New Restaurants”: Newest *launch_date* first (descending). This list has also a special rule: *launch_date* must be no older than 4 months.
- “Nearby Restaurants”: Closest to the given location first (ascending).

Remember to **cap each list to max. 10** best matching restaurants. The same restaurant can obviously be in multiple lists (if it matches given criteria).

See *discovery_page.json* for an example of the format the API should return. **Note: the sample is not based on any particular location on the map, so the data there might not be accurate.**

## Frontend Assignment
 
*discovery_page.json* in the repository has the following structure. 

```
{
   "sections": [
      {
           "title": "Popular Restaurants",
           "restaurants": [...10 restaurant objects...]
      },
      {
           "title": "New Restaurants",
           "restaurants": [...10 restaurant objects...]
      },
 	{
           "title": "Nearby Restaurants",
           "restaurants": [...10 restaurant objects...]
      }

   ]
}
```

**Your task is to render this page on the screen.**

There are four rules for rendering the page:
- Each section (*title* + *restaurants*-list fields) must be shown as a horizontal carousel (with the *title* visible). The user needs to be able to scroll the carousel to both directions.
- Each carousel can have **max. 5** elements visible on screen (at the same time).
- Each restaurant object is rendered with a *title*, *blurhash* and *online* fields.
- Carousels scroll infinitely, so when the user reaches the last element, it starts from the beginning (or vice versa). 

Otherwise you can freely decide everything, from the layout to image sizes.

Instead of real images we are using a technology known as BlurHash. This algorithm for representing images with a few dozen chars was created by Wolt's iOS engineer Dag Ågren in 2017. You can use [react-blurhash](https://github.com/woltapp/react-blurhash) or [Blurhash TypeScript](https://github.com/woltapp/blurhash/tree/master/TypeScript) library when rendering these values (..if you want to save some development time 😉). Blurhash values in the *discovery_page.json* were generated from 480x270 (16:9) images.

The image below is **just an example** of what the output could look like. In this version each carousel has max. 3 restaurants visible at once. 

**Feel free to use your creative superpowers!!!**

[![sample-ui.png](https://i.postimg.cc/sXZHcjTX/sample-ui.png)]

## General Instructions
- Write the assignment using either *TypeScript, ReasonML, Python, Scala, Kotlin* or *Java*.
- The frontend version must use *React* library
- Feel free to use any other 3rd party framework / library, however try to minimize external dependencies.

## A Few Tips
- Everyone in Wolt loves clean code
- Everyone in Wolt also loves unit tests
- Try to figure out what is the essential part in this task. Focus on that first.
- Don't forget README.md with clear instructions how to get the project up and running. The more easier we can test your code, the more happier you will make us 🙂.
- Double check everything before returning the assignment.

## Questions?
Contact severi.kausola@wolt.com
 
 
**Happy coding!**
