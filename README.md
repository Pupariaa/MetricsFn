# Metricsfn

## Introduction

`metricsfn` is a lightweight JavaScript library designed to facilitate the collection of metrics and performance data in an application.

## Installation

Install the library via npm:

```bash
npm install metricsfn
```



## Usage
```JS const Metrics = require('metrics-library');

// Create an instance of Metrics
const metrics = new Metrics();

// Initialize Metrics
metrics.Init();

// Add an ID
metrics.ID(123);

// Add data
metrics.UserID('user123')
    .TimeSamp()
    .ParseErrors();

// Retrieve data
const data = metrics.env();
console.log(data);
```
## Example 
```JS
const axios = require('axios')
const Metrics = require('metricsfn')
const metrics = new Metrics();

async function makeRequest(url, method = 'GET', data = null) {
  metrics.Init();
  const id = 123
  metrics.ID(id);
  metrics.ID(id).TimeSamp()

  const datas = {};
  const metricsDatas = {};

  try {
    metrics.TimerStart(id);
    const response = await axios({
      method,
      url,
      data,
    });

    metrics.ID(id).FetchTimeExec(metrics.TimerStop()).Success()
    datas = response.data
    metricsDatas = metrics.ID(id).env()

    return {datas, metricsDatas};

  } catch (error) {
    metrics.ID(id).FetchTimeExec(metrics.TimerStop()).FetchError().Aborted()
    metricsDatas = metrics.ID(id).env()

    return {datas, metricsDatas};

  }

}

const apiUrl = 'https://jsonplaceholder.typicode.com/posts/1';
makeRequest(apiUrl, 'GET');
```

## Available Methods

### `Init()`

Initializes the Metrics library.

### `ID(id: string)`

Adds a new ID to the data table.

- `id`: Unique identifier.

### `UserID(data: string)`

Adds the user ID to the data.

- `data`: User ID.

### `TimeSamp()`

Adds a timestamp to the data.

### `ParseErrors()`

Increments the parse errors count in the data.

### `SyntaxErrors()`

Increments the syntax errors count in the data.

### `FilterTimeExec(timesamp: string)`

Adds filter execution time to the data.

- `timesamp`: Timestamp of filter execution time.

### `GenerateTimeExec(timesamp: string)`

Adds generation execution time to the data.

- `timesamp`: Timestamp of generation execution time.

### `FetchTimeExec(timesamp: string)`

Adds fetch execution time to the data.

- `timesamp`: Timestamp of fetch execution time.

### `Aborted()`

Indicates in the data that the operation was aborted.

### `Exited()`

Indicates in the data that the operation was exited.

### `Success()`

Indicates in the data that the operation was successful.

### `TimerStart(key: string)`

Starts a timer with the specified key.

- `key`: Timer key.

### `TimerStop(key: string)`

Stops the timer and returns the elapsed time.

- `key`: Timer key.

### `TimerClear(key: string)`

Stops and clears the timer associated with the specified key.

- `key`: Timer key.

### `env()`

Gets the complete data array.
