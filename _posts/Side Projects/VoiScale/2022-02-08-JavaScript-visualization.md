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

index.js 
```
const http = require('https');
  
// Setting the configuration for
// the request
const options = {
    hostname: 'your_endpoint.execute-api.eu-central-1.amazonaws.com',
    path: '/items',
    method: 'GET'
};
    
// Sending the request
const req = http.request(options, (res) => {
    let data = ''
     
    res.on('data', (chunk) => {
        data += chunk;
    });
    
    // Ending the response 
    res.on('end', () => {
        console.log('Body:', JSON.parse(data))
    });
       
}).on("error", (err) => {
    console.log("Error: ", err)
}).end()
```