# Chronos - Timer and sheduler

work in progress

## For node JS or javascript.

Call the method with a time expression string and a callback function. 

The time expression can produce a complex reoccurring time pattern or a one-time shot, within years, seconds or even milliseconds.
This is not cron. Rather it is an attempt at updating the very successful cron syntax.

Chronos are based on the setTimeout function. It is not very precise. At present it seems to have an accuracy within 25 ms, without significant process load.  However it seems to defer execution time, during process load. In some cases that I preferable, but it makes the timing less precise. 
To increase precision, you would have to rewrite the function with some form of correction to real time.

### To add a timed job every day at noon:

```javascript
chronos.add({timex:”* * * 12”,  action:function(){console.log(“hello wolrd”)}});
```
## Time expression Syntax
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

##Note: 
- Time expression are in local time where as time stamps are in UTC
- Month and weekday use another offset then the javascript Date function:
- Month 1 is January 
- Week day 1-7 starting with Monday 
- In the current implementation, using javascripts settimeout, the accuracy is 
  likely within 25 ms.

 
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


### Functions

####Add job
```
result = chronos.add(<object>);

object:
* timex: <time expression>
* action: <function to execute>
  param: <an object parsed top the function>
  opt:	<options>

* mandatory elements.
```

Returns a result object:
```
result: “ok” or null
error: 	<an failure explanation> or null
id:	<integer used to identify the timer>
```

#### Remove job
`result = chronos.remove(id);`

Id is the value returned with chronos.add


## Use with node JS
#### Install
```bash
$ npm install chronos
```
#### Use
```js
var chronos = require('chronos');

// Add
var res1=chronos.add({timex:”* * * 12”,  action:function(){console.log(“hello wolrd”)}});

// Remove
var res2=chronos.remove(res1.id);
```


## Use in javascript
```html
<script type="text/JavaScript" src="chronos.js"></script>
<script>
// Add
var res1=chronos.add({timex:”* * * 12”,  action:function(){alert(“hello wolrd”)}});

// Remove
var res2=chronos.remove(res1.id);
</script>
```
<<<<<<< HEAD

=======
>>>>>>> 40688aa392e11b1c01766ee72ebba2dc35733d4e
