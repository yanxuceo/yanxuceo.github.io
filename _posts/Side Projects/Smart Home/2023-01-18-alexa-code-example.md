---
layout: post
title: "[Alexa skill code example] How to use Promise, async and await"
date: 2023-01-18 00:00:00 +0800
categories: [Side Projects, Smart Home]
tags: [Alexa Developers, AVS-Device-SDK, Smart Home, VoiScale]
---

Alexa cookbook on [<span style="color:#3ababa">Github</span>](https://github.com/alexa/alexa-cookbook)

```

function httpGet() {
  return new Promise(((resolve, reject) => {
    var options = {
        host: 'api.icndb.com',
        port: 443,
        path: '/jokes/random',
        method: 'GET',
    };
    
    const request = https.request(options, (response) => {
        response.setEncoding('utf8');
        let returnData = '';

        response.on('data', (chunk) => {
            returnData += chunk;
        });

        response.on('end', () => {
            resolve(JSON.parse(returnData));
        });

        response.on('error', (error) => {
            reject(error);
        });
    });

    request.end();

  }));
}


const GetJokeHandler = {
    canHandle(handlerInput) {
        const request = handlerInput.requestEnvelope.request;
        return request.type === 'IntentRequest'
            && request.intent.name === 'GetJokeIntent';
    },

    async handle(handlerInput) {
        const response = await httpGet();
        console.log(response);

        return handlerInput.responseBuilder
                .speak("Okay. Here is what I got back from my request. " + response.value.joke)
                .reprompt("What would you like?")
                .getResponse();
    },
};


(async function() {
    const lambdaClient = new LambdaClient({ region: "us-west-2" });
    // create JSON object for service call parameters
    const params = {
        FunctionName : "slotPull",
        InvocationType : "RequestResponse",
        LogType : "None"
    };

    // create InvokeCommand command
    const command = new InvokeCommand(params);

    // invoke Lambda function
    try {
        const response = await lambdaClient.send(command);
        console.log(response);
    } catch (err) {
        console.err(err);
    }
})();


function() {
    const lambdaClient = new LambdaClient({ region: "us-west-2" });
    // create JSON object for service call parameters
    const params = {
        FunctionName : "slotPull",
        InvocationType : "RequestResponse",
        LogType : "None"
    };

    // create InvokeCommand command
    const command = new InvokeCommand(params);

    // invoke Lambda function
    try {
        const response = await lambdaClient.send(command);
        console.log(response);
    } catch (err) {
        console.err(err);
    }
}


const https = require('https');

function makeRequest(options) {
  return new Promise((
    
  (resolve, reject) => {
     const request = https.request(options, (response) => {
        let data = '';
        response.on('data', (chunk) => {
           data += chunk;
        });
        response.on('end', () => {
           resolve(JSON.parse(data));
        });
        response.on('error', (error) => {
           reject(error);
        });
     });

     request.on('error', function(error) {
        reject(error);
     });
     
     request.end();
  }
  
  ));
}


function wait(duration) {
// Create and return a new Promise
    return new Promise((resolve, reject) => { // These control the Promise
        // If the argument is invalid, reject the Promise
        if (duration < 0) {
            reject(new Error("Time travel not yet implemented"));
        }
        // Otherwise, wait asynchronously and then resolve the Promise.
        // setTimeout will invoke resolve() with no arguments, which means
        // that the Promise will fulfill with the undefined value.
        setTimeout(resolve, duration);
    });
}

const SearchHandler = {
  canHandle(handlerInput) {
     return handlerInput.requestEnvelope.request.type === 'IntentRequest' &&
        handlerInput.requestEnvelope.request.intent.name === 'SearchIntent';
  },
  handle(handlerInput) {
     const query = handlerInput.requestEnvelope.request.intent.slots.Item.value;
     const location = handlerInput.requestEnvelope.request.intent.slots.Location.value;

     const geocodeOptions = {
        host: 'geocoder.ls.api.here.com',
        path: `/6.2/geocode.json?apiKey=${here.apikey}&searchtext=${location}`,
        method: 'GET'
     };

     const errorMessage = `We didn't find any ${query} in ${location}`;

     return new Promise((resolve, reject) => {
        makeRequest(geocodeOptions).then((geocodeResponse) => {
           const coordinates = geocodeResponse.Response.View[0].Result[0].Location.DisplayPosition;

           const placesOptions = {
             host: 'places.ls.hereapi.com',
             path: `/places/v1/discover/search?at=${coordinates.Latitude},${coordinates.Longitude}&q=${query.replace(/ /g, '+')}&apiKey={YOUR-API_KEY}`,
             method: 'GET'
           };

           makeRequest(placesOptions).then((placeResponse) => {
             const places = placeResponse.results.items;
             const examplePlace = places[0];
             if (examplePlace) {
               const successOutput = `We found a nearby ${query} place near ${location}! ${examplePlace.title} is ${(examplePlace.distance * metersToMiles).toFixed(1)} miles away and is located at ${examplePlace.vicinity}`;
               resolve(handlerInput.responseBuilder.speak(successOutput).getResponse());
             } else {
               resolve(handlerInput.responseBuilder.speak(errorMessage).getResponse());
             }

           }).catch((error) => {
             resolve(handlerInput.responseBuilder.speak(errorMessage).getResponse());
           });

        }).catch((error) => {
           resolve(handlerInput.responseBuilder.speak(errorMessage).getResponse());
        });
     });
  }
};


//example number guessing
handle(handlerInput) {
   
   const theNumber = handlerInput.requestEnvelope.request.intent.slots.number.value;
  
   const speakOutput = theNumber ;
   const repromptOutput = " Would you like another fact?";
  
   return new Promise((resolve, reject) => {
       getHttp(URL, theNumber).then(response => {
           speakOutput += " " + response;
           resolve(handlerInput.responseBuilder
               .speak(speakOutput + repromptOutput)
               .reprompt(repromptOutput)
               .getResponse());
       }).catch(error => {
           reject(handlerInput.responseBuilder
               .speak(`I wasn't able to find a fact for ${theNumber}`)
               .getResponse());
       });
   });   
}

const getHttp = function(url, query) {
   return new Promise((resolve, reject) => {
       const request = http.get(`${url}/${query}`, response => {
           response.setEncoding('utf8');
          
           let returnData = '';
           if (response.statusCode < 200 || response.statusCode >= 300) {
               return reject(new Error(`${response.statusCode}: ${response.req.getHeader('host')} ${response.req.path}`));
           }
          
           response.on('data', chunk => {
               returnData += chunk;
           });
          
           response.on('end', () => {
               resolve(returnData);
           });
          
           response.on('error', error => {
               reject(error);
           });
       });
       request.end();
   });
}


//async and await version
async handle(handlerInput) {
    const theNumber = handlerInput.requestEnvelope.request.intent.slots.number.value;

    let speakOutput = theNumber ;
    const repromptOutput = " Would you like another fact?";

    try {
        const response = await getHttp(URL, theNumber);
        speakOutput += " " + response;
    
        handlerInput.responseBuilder
            .speak(speakOutput + repromptOutput)
            .reprompt(repromptOutput)
        
    } catch(error) {
        handlerInput.responseBuilder
            .speak(`I wasn't able to find a fact for ${theNumber}`)
            .reprompt(repromptOutput)
    }

    return handlerInput.responseBuilder
        .getResponse();
}


const getHttp = function(url, query) {
    return new Promise((resolve, reject) => {
        const request = http.get(`${url}/${query}`, response => {
            response.setEncoding('utf8');
        
            let returnData = '';
            if (response.statusCode < 200 || response.statusCode >= 300) {
                return reject(new Error(`${response.statusCode}: ${response.req.getHeader('host')} ${response.req.path}`));
            }
        
            response.on('data', chunk => {
                returnData += chunk;
            });
        
            response.on('end', () => {
                resolve(returnData);
            });
        
            response.on('error', error => {
                reject(error);
            });
        });
        request.end();
    });
}

```