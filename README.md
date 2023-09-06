# Date-CSV
[06/09, 7:42 am] Harsh: import React, { useState } from 'react';
import axios from 'axios';

function CsvToJsonConverter() {
  const [jsonData, setJsonData] = useState([]);
  const [selectedFile, setSelectedFile] = useState(null);

  const handleFileChange = (event) => {
    setSelectedFile(event.target.files[0]);
  };

  const handleConvert = () => {
    if (!selectedFile) {
      alert('Please select a CSV file.');
      return;
    }

    const formData = new FormData();
    formData.append('csvFile', selectedFile);

    axios.post('http://localhost:5000/convert', formData)
      .then((response) => {
        setJsonData(response.data);
      })
      .catch((error) => {
        console.error('Error:', error);
      });
  };

  return (
    <div>
      <input type="file" accept=".csv" onChange={handleFileChange} />
      <button onClick={handleConvert}>Convert CSV to JSON</button>
      <pre>{JSON.stringify(jsonData, null, 2)}</pre>
    </div>
  );
}

export default CsvToJsonConverter;
[06/09, 7:42 am] Harsh: const express = require('express');
const csvtojson = require('csvtojson');
const cors = require('cors');
const fs = require('fs');
const app = express();
const port = 5000;

app.use(cors());

app.post('/convert', (req, res) => {
  const csvFilePath = 'your-csv-file.csv';

  csvtojson()
    .fromFile(csvFilePath)
    .then((jsonArrayObj) => {
      res.json(jsonArrayObj);
    });
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
[06/09, 8:31 am] Harsh: function convertDateFormat(inputDate) {
  // Split the input date string into day, month, and year components
  const parts = inputDate.split('/');

  // Ensure day and month have two digits by padding with leading zeros
  const day = parts[0].padStart(2, '0');
  const month = parts[1].padStart(2, '0');
  const year = parts[2];

  // Create a new Date object with the parsed components
  const date = new Date(year, month - 1, day); // Subtract 1 from month to adjust for JavaScript's 0-based months

  // Extract year, month, and day components
  const formattedYear = date.getFullYear();
  const formattedMonth = (date.getMonth() + 1).toString().padStart(2, '0'); // Add leading zero if needed
  const formattedDay = date.getDate().toString().padStart(2, '0'); // Add leading zero if needed

  // Combine components into the "yyyymmdd" format
  const yyyymmdd = `${formattedYear}${formattedMonth}${formattedDay}`;

  return yyyymmdd;
}

// Example usage:
const inputDate = '6/9/2023'; // Single-digit day and month
const convertedDate = convertDateFormat(inputDate);
console.log(convertedDate); // Output: '20230906'
