---
layout: post
title: "[VoiScale User Data Visualization] JavaScript, HTML and AWS Lambda code example"
date: 2023-02-08 00:00:00 +0800
categories: [Side Projects, Smart Food Scale]
tags: [Dynamodb, Smart Kitchen, JavaScript, VoiScale]
---

Create User-friendly Interface with [<span style="color:#3ababa">Chart.js</span>](https://www.chartjs.org/docs/latest/) <br/>
Tutorial from [<span style="color:#3ababa">W3 Schools</span>](https://www.w3schools.com/js/js_graphics_chartjs.asp)


### Code example of pie chart:
```
<!DOCTYPE html>
<html>

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

### Example of Node.js setup a webserver, request DynamoDB data 
index.js 

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

### Example of side-by-side chart display
styles.css
```
*{
    margin: 0;
    padding: 0;
    font-family: sans-serif;
}

.charMenu {
    width: 100vw;
    height: 40px;
    background: #60b5b2;
    color: rgb(242, 14, 14);
}

.chartMenu  p {
    padding: 10px;
    font-size: 20px;
}

.chartCard {
    width: 100vw;
    height: calc(100vh - 40px);
    background: rgba(103, 182, 179, 0.2);
    display: flex;
    align-items: center;
    justify-content: center;
}

.chartBox {
    width: 300px;
    
    padding: 20px;
    border-radius: 20px;
    margin: 20px;
    background: rgb(168, 217, 180);
}
```

index.html
```
<!DOCTYPE html>
<html>

<head>

<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">

<link rel="stylesheet" href="styles.css">
<! -- this is for pie chart -->

</head>


<body>
	<div class="charMenu">
		<p style="text-align:center;line-height:35px;font-family:verdana">VoiScale - We care about your heath!</p>
	</div>

	<div class="chartCard">
		<div class="chartBox">
			<canvas id="myChart"></canvas>
		</div>
		

		<div class="chartBox">
			<canvas id="hisChart"></canvas>
		</div>
	</div>


  	<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  
  	<script>
		const ctx = document.getElementById('myChart');
	
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
   

    <script>
		const cty = document.getElementById('yourChart');

		new Chart(cty, {
			type: 'bar',
			data: {
				labels: ['Red', 'Blue', 'Yellow', 'Green', 'Purple', 'Orange'],
				datasets: [{
					label: '# of Votes',
					data: [12, 19, 3, 5, 2, 3],
					borderWidth: 1
				}]
			},
			options: {
				aspectRatio: 1,
				scales: {
					y: {
						beginAtZero: true
					}
				}
			}
		});
  	</script>


	<script>
		const ctz = document.getElementById('hisChart');

		new Chart(ctz, {
			type: 'line',
			data: {
				labels: ['Mon','Tue','Wed','Thur','Fri','Sat','Sun'],
				datasets: [
					{ 
						data: [190,188,185,190,203,216,208],
						label: "Calories",
						borderColor: "#FD8A8A",
						fill: false
					}
				]
			},
			options: {
				aspectRatio: 1,
				title: {
					display: true,
					text: 'Calories consumption'
				}
			}
		});
	</script>

</body>


</html>
```

<img src="/assets/img/JavaScript/side-by-side-charts_00.PNG" alt="side by side pie chart examples" width="500"/>  <br />
