---
layout: page
title: Getting Started
tagline: Documentation
---
{% include JB/setup %}

<style>
	img {
		max-width: inherit;
		border: 1px solid #ccc;
		margin: 1em 0;
	}
</style>

## Introduction<a name="gettingstarted">&nbsp;</a>

This section describes the first steps needed in order to use the API.
If you are new to REST APIs please read the [About REST]({{BASE_PATH}}/rest) section first.

------------------------------------------------------
## Requirements

The only tool needed in this section is a browser with a **REST client Add-on** to access the API. This document doesn't cover the API access through a programming language since everybody has its favourite language.

#### Get a REST client
There are various free clients available please pick one for your favourite browser in the browser Add-on section.

In the following steps *Postman* for Chrome from the [Google Webstore](https://chrome.google.com/webstore/search/rest%20client) will be used.
For Firefox the [REST Client](https://addons.mozilla.org/de/firefox/addon/restclient) is another popular client.

------------------------------------------------------
## Find your gateway

As first step the gateway IP address and port must be found.

This could be achieved by doing a `GET` request to `https://dresden-light.appspot.com/discover`.

<img src="img/0_discover_api.png" alt="discover the gateway"/>

The response body shows that the gateway has the IP address _192.168.192.32_ and the API
is reachable through port _8080_.

`Note` If the above request doesn't work, there are serval other ways to find the gateway IP address as described in the [Discovery]({{BASE_PATH}}/discovery) section.

------------------------------------------------------
## Aquire a API key

Any client which wants to access the API must provide a valid API key otherwise the access will fail.

To aquire a API key send a `POST` request to `/api` as follows.

`Note` The request must contain a JSON object with the required field _devicetype_.

<img src="img/1_aquire_apikey_fail.png" alt="aquire api key failed"/>

`--->` **This didn't work!**

The <span class="label">STATUS</span> says `403 Forbidden`.

The response body provides further information about the raised error in the JSON object.

#### Unlock the gateway

The reason why the request failed is that the gatway was not unlocked. This mechanism is needed to prevent anybody from access the gateway without beeing permitted to do so.

As described in the section [Authorization]({{BASE_PATH}}/authorization) unlock the gateway as follows:

 - in a new browser tab open the the webapp
 - click on `Setting/System` from the top menu
 - click on the `Apps` tab 
 - click on the <a class="btn btn-primary btn-small" href="#">unlock</a> button

 Now the gateway is unlocked for _60 seconds_.

#### Second attemp

Within 60 seconds after unlocking the gateway go back to the REST client and do the aquire API key request as before again. (just click on _Send_ again)

<img src="img/1_aquire_apikey_ok.png" alt="aquire api key success"/>

This time the requests succeded with <span class="label">STATUS</span> 200 OK.

In the response body the new API key is in the field `username`, from now on this key will be used in further API requests.

------------------------------------------------------
## Get a list of all lights

With the API key from the last section it's now possible to access the full API.

To get a list of all available lights do a `GET` request to `/api/<apikey>/lights` as follows.

<img src="img/2_get_all_lights.png" alt="get all lights"/>

In the response 3 lights where returned. There are serval things to note here.

 - the response contains not a list like `[ ]` of lights but a object `{ }` with key/value pairs
 - each light can be accessed by its id `"17"`
 - the light id is a key in the response object and the related value is a further object
 
 `Note` Ids are strings and even if they contain numbers **never** expect them to be "1", "2", "3", ... if the user removes light "2" the list will become "1", "3".

------------------------------------------------------
## Get the details of a light

To get the detail of a light do a `GET` request to `/api/<apikey>/lights/<id>` as follows.

<img src="img/3_get_light_details.png" alt="get light details"/>

## Turn light on/off

To turn a light on/off do a `PUT` request to `/api/<apikey>/lights/<id>/state` as follows.

<img src="img/4_turn_light_on.png" alt="turn light on/off"/>

In the request body set the `on` value to _true_ or _false_ to turn the light on and off. 

------------------------------------------------------
## Dimm the light with transitiontime

Dimming is done the same way as sending on/off by using the `bri` parameter and additionally specify a transitiontime in 1/10 seconds.

The following example dimms the light in 5 seconds down.

<img src="img/5_dimm_light.png" alt="dimm light"/>

------------------------------------------------------
## What's next

To do some more advanced things with this API please refer to the _API endpoints_ documentation on the left side menu.