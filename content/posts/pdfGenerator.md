+++
title = 'Pdf Generator tutorial'
date = 2024-05-21T21:07:23+03:00

+++


# Building a PDF Report Generator with Analytics Using the MERN Stack

Ever wanted to create a professional-looking PDF report with embedded analytics? Today, I’ll walk you through how to build a PDF report generator using the MERN stack. We'll use `pdfMake` for generating PDFs and `eCharts` for those sleek analytics charts. We'll fetch data from the backend, create charts server-side, and send the chart options along with data to the frontend for display and filtering.

## What You’ll Need

Before we dive in, make sure you have the following:

- **Node.js**
- **MongoDB**
- **Express.js**
- **React.js**

You’ll also need these libraries:

- `pdfMake`
- `eCharts`
- `axios` (for API requests)
- `express`
- `mongoose`

## Step 1: Setting Up the Backend

### 1.1 Initialize Your Project

First, let's set up our project directory and initialize it with Node.js:

```bash
mkdir pdf-report-generator
cd pdf-report-generator
npm init -y
```

```bash
1.2 Install Dependencies
Now, we need to install the libraries we’ll be using:

npm install express mongoose pdfmake axios echarts-node-canvas
```


1.3 Configure the Server
Create an index.js file for our Express server and set up the basic server configuration:

```javascript
Copy code
const express = require('express');
const mongoose = require('mongoose');
const { createCanvas } = require('echarts-node-canvas');
const pdfMake = require('pdfmake');

const app = express();
const PORT = process.env.PORT || 3000;

mongoose.connect('mongodb://localhost:27017/analyticsDB', { useNewUrlParser: true, useUnifiedTopology: true });

const DataSchema = new mongoose.Schema({
  category: String,
  value: Number
});

const Data = mongoose.model('Data', DataSchema);

app.use(express.json());

// Fetch data and generate chart options
app.get('/api/report', async (req, res) => {
  try {
    const data = await Data.find();

    // Generate chart options
    const chartOptions = {
      title: {
        text: 'Analytics Report'
      },
      tooltip: {},
      xAxis: {
        type: 'category',
        data: data.map(item => item.category)
      },
      yAxis: {
        type: 'value'
      },
      series: [{
        type: 'bar',
        data: data.map(item => item.value)
      }]
    };

    // Render chart as image
    const canvas = createCanvas();
    const echartsInstance = canvas.getEChartsInstance();
    echartsInstance.setOption(chartOptions);
    const chartBuffer = await canvas.renderToBuffer();

    // Create PDF document
    const docDefinition = {
      content: [
        { text: 'Analytics Report', style: 'header' },
        { image: chartBuffer.toString('base64'), width: 500 }
      ],
      styles: {
        header: {
          fontSize: 18,
          bold: true
        }
      }
    };

    const pdfDoc = pdfMake.createPdf(docDefinition);
    const pdfBuffer = await new Promise((resolve, reject) => {
      pdfDoc.getBuffer((buffer) => {
        resolve(buffer);
      });
    });

    res.setHeader('Content-Type', 'application/pdf');
    res.send(pdfBuffer);

  } catch (error) {
    res.status(500).send('Error generating report');
  }
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
1.4 Seed the Database
Let’s add some sample data to our MongoDB database. Create a seed.js file:

javascript

const mongoose = require('mongoose');

const DataSchema = new mongoose.Schema({
  category: String,
  value: Number
});

const Data = mongoose.model('Data', DataSchema);

mongoose.connect('mongodb://localhost:27017/analyticsDB', { useNewUrlParser: true, useUnifiedTopology: true });

const seedData = [
  { category: 'A', value: 30 },
  { category: 'B', value: 80 },
  { category: 'C', value: 45 },
  { category: 'D', value: 60 },
];

Data.insertMany(seedData).then(() => {
  console.log('Data seeded');
  mongoose.connection.close();
});

```
Run the seed script with:

```bash
node seed.js
```
Step 2: Setting Up the Frontend
2.1 Initialize React Project
Let’s create a React project (if you don’t have one already):


```bash
npx create-react-app frontend
cd frontend
```
2.2 Install Dependencies
We’ll use axios for making API requests:

```bash
npm install axios
```
2.3 Create Components
We need a component to fetch and display our report. Create a ReportGenerator.js file in the src/components directory:

```javascript
// src/components/ReportGenerator.js

import React, { useState, useEffect } from 'react';
import axios from 'axios';

const ReportGenerator = () => {
  const [reportData, setReportData] = useState(null);

  useEffect(() => {
    const fetchReport = async () => {
      try {
        const response = await axios.get('/api/report');
        setReportData(response.data);
      } catch (error) {
        console.error('Error fetching report', error);
      }
    };

    fetchReport();
  }, []);

  return (
    <div>
      <h1>Analytics Report</h1>
      {reportData ? (
        <iframe src={`data:application/pdf;base64,${reportData}`} width="600" height="800"></iframe>
      ) : (
        <p>Loading...</p>
      )}
    </div>
  );
};

export default ReportGenerator;
```
2.4 Set Up Proxy
To avoid any CORS issues, set up a proxy in your package.json:

```json
// package.json
"proxy" : "http://localhost:3000"
```
2.5 Use the Component
Now, integrate the ReportGenerator component into your main App.js file:

```javascript
Copy code
// src/App.js

import React from 'react';
import ReportGenerator from './components/ReportGenerator';

function App() {
  return (
    <div className="App">
      <ReportGenerator />
    </div>
  );
}

export default App;
```
Step 3: Running the Application
We’re ready to see it in action! Start your backend server:

```bash
node index.js
```
Then, start the React development server:

```bash
npm start
Open http://localhost:3000 in your browser, and you should see your analytics report.
```
## Wrapping Up
And there you have it! We’ve built a PDF report generator with analytics using the MERN stack. We used pdfMake for generating the PDF and eCharts for creating charts on the backend. The data and chart options are fetched from the backend and displayed on the frontend, giving you a dynamic and interactive report.

Feel free to customize and expand this project to suit your needs. Happy coding!