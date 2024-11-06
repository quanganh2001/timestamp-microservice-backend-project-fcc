# Step 1: Creating the date route
```js
app.get("/api/:date?", function (req, res) {
  let date = req.params.date;
  res.json({date: date});
});
```
# Step 2: Returning the unix timestamp
Let's test whether the input date is a number. If our input is a unix string we set the `unixDate` as the input. Otherwise, we build the Date Object and get the unix time
```js
app.get("/api/:date?", function (req, res) {
  let date = req.params.date;
  let unixDate;
  let dateObj;

  // Test whether the input date is a number
  let isUnix = /^\d+$/.test(date);

  // If our input is a unix string we set the unixDate as the input
  if (isUnix) {
    unixDate = parseInt(date);
  }
  // Otherwise we build the Date Object and get the unix time
  else if (!isUnix) {
    dateObj = new Date(date);
    unixDate = dateObj.getTime();
  }

  res.json({unix: unixDate});
});
```
# Step 3: Returning the UTC time string
```js
app.get("/api/:date?", function (req, res) {
  let date = req.params.date;
  let unixDate;
  let dateObj;
  let utcDate;

  // Test whether the input date is a number
  let isUnix = /^\d+$/.test(date);

  // If our input is a unix string we set the unixDate as the input
  if (isUnix) {
    unixDate = parseInt(date);
    dateObj = new Date(unixDate);
    utcDate = dateObj.toUTCString();
  }
  // Otherwise we build the Date Object and get the unix time
  else if (!isUnix) {
    dateObj = new Date(date);
    unixDate = dateObj.getTime();
    utcDate = dateObj.toUTCString();
  }

  res.json({unix: unixDate, utc: utcDate});
});
```
# Step 4: Returning an error for invalid date
```js
if (dateObj.toString() === "Invalid Date") {
  res.json({error: "Invalid Date"});
  return;
}
```
# Step 5: Use today's date if no date specified
Full source code solution:
```js
app.get("/api/:date?", function (req, res) {
  let date = req.params.date;

  let unixDate;
  let dateObj;
  let utcDate;

  // Test whether the input date is a number
  let isUnix = /^\d+$/.test(date);

  // If no date specified, use the current date
  if (!date) {
    dateObj = new Date();
  } 
  // If the date is a Unix Timestamp
  else if (date && isUnix) {
    unixDate = parseInt(date);
    dateObj = new Date(unixDate);
  }
  // If the date is not a unix time stamp
  else if (date && !isUnix) {
    dateObj = new Date(date);
  }

  if (dateObj.toString() === "Invalid Date") {
    res.json({error: "Invalid Date"});
    return;
  }

  unixDate = dateObj.getTime();
  utcDate = dateObj.toUTCString();

  res.json({unix: unixDate, utc: utcDate});
});
```