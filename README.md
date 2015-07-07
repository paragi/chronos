# Chronos - Timer and scheduler
## For node JS or javascript.

### Features
* Extensively improved cron-like syntax (Not compatible)
* Milliseconds resolution
* Recalculate long running timers, to improve accuracy
* No dependencies
* Written for both node JS and browser inclusion
* Time expressions include range, set, timestamp, weekday, yearday 


Chronos are based on the setTimeout function. It is not very precise. At present it seems to have an accuracy within 2 ms in node and 25 ms i most browsers.  However it seems to defer execution time, during process load. In some cases that I preferable, but it makes the timing less precise. 
To increase precision, you would have to rewrite the function with some form of correction to real time.

### Example
To add a timed job every day at noon:

```javascript
chronos.add(”* * * 12”, function(){console.log(“hello - it's noon again”)});
```

## Time expression Syntax
---
The basic syntax is a series of fields specifying the time(s):

 `<year> <month> <day> <hour> <minute> <second> <millisecond> <microsecond> ...`

or a time stamp.

Each field contain wild-cards, ranges, sets, not flags and every flags. Plus some special flags for year days and week days.

The epoch timestamp is seconds since 1970-01-01 UTC with fractions of second as decimal part:

	@<epoch>[.<faction of second>]

##### Field syntax: 	`[!][-]<value>[-<value>]|[,<value>] | /<value> | *`
```
" " : field separator
*   : all values. Flags will be ignored.
!   : not
/   : every (can not be combined with ! and range)
-   : Negative values are counted back from the maximum value
a-b : range. both a and b included.
a,b : set of values

Day field can have the one of the following flags as well
y: day of year
w: day of week 1-7  (1 is Monday)
```
Unspecified minor fields are assumed to have the lowest possible value

## Note: 
- Time expression are in local time where as time stamps are in UTC
- Month and weekday use another offset then the javascript Date function:
- Month 1 is January 
- Week day 1-7 starting with Monday 

 
### Examples:
| Tables        | Are           |
| ------------- |:-------------:|
| Every hour|  * * * *|
| Every day at noon| * * * 12
| Every 3th Hour on work days| * * w1-5 /3
| At a specific epoch time|@1422821601.123  
| At a specific time| 2014 5 13 18 53 7 300 230
| 2th to last day of the month at noon| * * -2 12
| 3th last day of the year| * * y-3
| 3 times an hour during work time| * * w1-5 9-17 0,20,40
| Every morning at 7:30 but not on a weekend| * * !6-7 7 30  
| Every 10 minutes in the day time|  * * * 8-18 /10


## API
---
##### chronos.add(timeExpression, callBack [,parameterToCallBack])

Returns a result object:
```
{
  result: “ok” or null
  error: 	a failure explanation or null
  id:	    integer used to identify the timer
}
```


##### chronos.remove(id)
where id is the value returned from chronos.add

Returns a result object:
```
{
  result: “ok” or null
  error: 	a failure explanation or null
}
```


##### chronos.get([id])
where the optional id is the value returned from chronos.add

Returnes either a chronos timer object if id is given, or an array of all active timer objects.


### Settings
##### chronos.timeResolution (integer)
This is the minimum time resolution for an expression. Minimum value is 1 ms. default is 2 ms.
This should be more the the execution time and delays do to load, of the intepreter. 

##### chronos.maxTimerDelay (integer)
Maximum run time of a setTimeout call. Some javascripts engines cant handle more then 32 bit = 0x7FFFFFF. thats about 28 days. default is 86400000 = 1 day.
When this time have elapsed, the timer event are reevaluated.


## With node JS
---
#### Install
```bash
$ npm install e-chron
```
#### Use
```js
var chronos = require('chronos');

// Add
var res1=chronos.add(”* * * 12”, function(){console.log(“hello wolrd”)});

// Remove
var res2=chronos.remove(res1.id);
```


## With HTML & javascript
---
#### Install
Copy files to folder.

#### Use
```html
<script type="text/JavaScript" src="chronos.js"></script>
<script>
// Add
var res1=chronos.add(”* * * 12”, function(){alert(“hello wolrd”)});

// Remove
var res2=chronos.remove(res1.id);
</script>
```
