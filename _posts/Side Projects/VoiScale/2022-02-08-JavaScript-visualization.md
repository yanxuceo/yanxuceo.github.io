---
layout: post
title: "[VoiScale User Data Visualization] JavaScript, HTML and AWS Lambda code example"
date: 2023-02-08 00:00:00 +0800
categories: [Side Projects, Smart Food Scale]
tags: [Dynamodb, Smart Kitchen, JavaScript, VoiScale]
---

Create User-friendly Interface with [<span style="color:#3ababa">Chart.js</span>](https://www.chartjs.org/docs/latest/) <br/>
Tutorial from [<span style="color:#3ababa">W3 Schools</span>](https://www.w3schools.com/js/js_graphics_chartjs.asp)


code example:
```
<script
src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.9.4/Chart.js">
</script>

<canvas id="myChart" style="width:100%;max-width:500px"></canvas>

<script>
var xValues = ["Calories", "Protein", "Fat", "Carbohydrate", "Fiber"];
var yValues = [55, 49, 44, 24, 15];
var barColors = [
  "#FFCEFE",
  "#FFFFD0",
  "#FF7B54",
  "#86C8BC",
  "#C0DEFF"
];

new Chart("myChart", {
  type: "pie",
  data: {
    labels: xValues,
    datasets: [{
      backgroundColor: barColors,
      data: yValues
    }]
  },
  options: {
    title: {
      display: true,
      text: "Your Food Nutrition Facts"
    }
  }
});
</script>

</html>
```

<img src="/assets/img/JavaScript/pie_chart_nutrition_facts_00.PNG" alt="pie chart food facts" width="500"/>  <br />

index.js example to setup a webserver, request DynamoDB data 
```
const http = require('https');
const http_test = require('http');
  

const host = 'localhost';
const port = 8000;

function httpGet() {
    return new Promise(((resolve, reject) => {
        var options = {
            hostname: 'your_endpoint.execute-api.eu-central-1.amazonaws.com',
            path: '/items',
            method: 'GET'
        };
      
      const request = http.request(options, (response) => {
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
  

var scale_response = async function getVoiScale() {
    const response = await httpGet();
    console.log(response);
}

const requestListener = function (req, res) {
    scale_response();

    res.writeHead(200);
    res.end("My first server!");
};


const server = http_test.createServer(requestListener);
server.listen(port, host, () => {
    console.log(`Server is running on http://${host}:${port}`);
});
```