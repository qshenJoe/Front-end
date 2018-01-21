# The Date Type
The `Date` type stores dates as the number of milliseconds that have passed since midnight on January 1, 1970 UTC(Universal Time Code). <br />
To create a date object, use the new operator along with the `Date` constructor:

```javascript
    var now = new Date();
```

To create a date based on another date or time, you must pass in the millisecond representation of the date(the number of milliseconds after midnight, January 1, 1970 UTC).
* `Date.parse` method accepts a string argument representing a date, now supporting the following date formats:
  * month/date/year(such as 6/13/2004)
  * month_name date, year(such as January 12, 2004)
  * day_of_week month_name date year hours:minutes:seconds time_zone (such as Tue May 25 2004 00:00:00 GMT-0700)
  * ISO 8601 extended format YYYY-MM-DDTHH:mm:ss.sssZ(such as 2004-05-25T00:00:00)
  
```javascript
    var someDate = new Date(Date.parse("May 25, 2004"));
    
    // the Date constructor will call Date.parse() behind the scenes if a string is passed in directly
    
    var someDate = new Date("May 25, 2004");  // produces the same result as the previous example
```

* `Date.UTC()`
  * The arguments for `Date.UTC()` are the year, the zero-based month (January is 0), the day of the month(1 - 31) and the hours (0 - 23), minutes, seconds, and milliseconds of the time
  * Of these arguments, only the year and month are required
  * If the day of the month isn't supplied, it's assumed to be 1, while all other omitted arguments are assumed to be 0

```javascript
    // January 1, 2000 at midnight GMT
    var y2k = new Date(Date.UTC(2000, 0));
    
    // January 1, 2000 at midnight in local time
    var y2k = new Date(2000, 0);
    
    // May 5, 2005 at 5:55:55 PM GMT
    var allFives = new Date(Date.UTC(2005, 4, 5, 17, 5, 55, 55));
    
    // May 5, 2005 at 5:55:55 PM local time
    var allFives = new Date(2005, 4, 5, 17, 5, 55, 55);
```

ECMAScript 5 adds `Date.now()`, which returns the millisecond representation of the date and time at which the method is executed.

```javascript
    // get start time
    var start = Date.now();
    
    // call a function
    doSomething();
    
    // get stop time
    var stop = Date.now(),
        result = stop - start;
        
    // for browsers that don't yet support this method:
    
    var start = +new Date();
    doSomething();
    var stop = +new Date(),
        result = stop - start;        
```

## Inherited Methods
As with the other reference types, the `Date` type overrides `toLocaleString()`, `toString()`, and `valueOf()`.
* `toLocaleString()` method returns the date and time in a format appropriate for the locale in which the browser is being run
  * the format includes AM or PM for the time
  * doesn't include any time-zone information
* `toString()` method returns the date and time with time-zone information
  * the time is indicated in 24-hour notation(0-23)
* `valueOf()` method for the `Date` type doesn't return a string at all
  * it is overridden to return the milliseconds representation of the date so that operators will work
  
```javascript
    var date1 = new Date(2007, 0, 1);   // "January 1, 2007"
    var date2 = new Date(2007, 1, 1);   // "February 1, 2007"
    
    alert(date1 < date2);   // true
    alert(date1 > date2);   // false
```
## Date-Formatting Methods
There are several `Date` type methods used specifically to format the date as a string.
* `toDateString` - displays the date's day of the week, month, day of the month, and year in an implementation-specific format
* `toTimeString()` - displays the date's hours, minutes, seconds, and time zone in an implementation-specific format
* `toLocaleDateString()` - displays the date's day of the week, month, day of the month, and year in an implementation-and locale-specific format
* `toLocaleTimeString()` - displays the date's hours, minutes, and seconds in an implementation-specific format
* `toUTCString()` - displays the complete UTC date in an implementation-specific format

```javascript
    var today = new Date();   //
    console.log(today.toDateString());    // Sun Jan 21 2018
    console.log(today.toTimeString());    // 09:53:24 GMT-0500 (EST)
    console.log(today.toLocaleDateString());    // 1/21/2018
    console.log(today.toLocaleTimeString());    // 9:53:24 AM
    console.log(today.toUTCString());   // Sun, 21 Jan 2018 14:53:24 GMT
```

## Date/Time Component Methods

|**METHOD**|**DESCRIPTION**|
|---|---|
|`getTime()`|Returns the milliseconds representation of the date; same as `valueOf()`|
|`setTime(milliseconds)`|Sets the milliseconds representation of the date, thus changing the entire date|
|`getFullYear()`|Returns the four-digit year|
|`getUTCFullYear()`|Returns the four-digit year of the UTC date value|
|`setFullYear(year)`|Sets the year of the date. The year must be given with four digits|
|`setUTCFullYear(year)`|Sets the year of the UTC date. The year must be given with four digits|
|`getMonth()`|Returns the month of the date, where 0 represents January and 11 represents December|
|`getUTCMonth()`|Returns the month of the UTC date, where 0 represents January and 11 represents December|
|`setMonth(month)`|Sets the month of the date, which is any number or greater. Numbers greater than 11 add years|
|`setUTCMonth(month)`|Sets the month of the UTC date, which is any number 0 or greater. Numbers greater than 11 add years|
|`getDate()`|Returns the day of the month(1 through 31) for the date|
|`getUTCDate()`|Returns the day of the month(1 through 31) for the UTC date|
|`setDate(date)`|SetS the day of the month for the date. If the date is greater than the number of days in the month, the month value also gets increased|
|`setUTCDate(date)`|Sets the day of the month for the UTC date. If the date is greater than the number of the days in the month, the month value also gets increased|
|`getDay()`|Returns the date's day of the week as a number(0 for Sunday and 6 for Saturday)|
|`getUTCDay()`|Returns the UTC date's of the week as a number(0 for Sunday and 6 for Saturday)|
|`getHours()`|Returns the date's hours as a number between 0 and 23|
|`getUTCHours()`|Returns the UTC date's hours as a number between 0 and 23|
|`setHours(hours)`|Sets the date's hour. Setting the hours to a number greater than 23 also increments the day of the month|
|`setUTCHours(hours)`|Sets the UTC date's hours. Setting the hours to a number greater than 23 also increments the day of the month|
|`getMinutes()`|Returns the date's minutes as a number between 0 and 59|
|`getUTCMinutes()`|Returns the UTC date's minutes as a number between 0 and 59|
|`setMinutes(minutes)`|Sets the date's minutes. Setting the minutes to a number greater than 59 also increments the hour|
|`setUTCMinutes(minutes)`|Sets the UTC date's minutes. Setting the minutes to a number greater than 59 also increments the hour|
|`getSeconds()`|Returns the date's seconds as a number between 0 and 59|
|`getUTCSeconds()`|Returns the UTC date's seconds as a number between 0 and 59|
|`setSeconds(seconds)`|Sets the date's seconds. Setting the seconds to a number greater than 59 also increments the minutes|
|`setUTCSeconds(seconds)`|Sets the UTC date's seconds. Setting the seconds to a number greater than 59 also increments the minutes|
|`getMilliseconds()`|Returns the date's milliseconds|
|`getUTCMilliseconds()`|Returns the UTC date's milliseconds|
|`setMilliseconds(milliseconds)`|Sets the date's milliseconds|
|`setUTCMilliseconds(milliseconds)`|Sets the UTC date's milliseconds|
|`getTimezoneOffset()`|Returns the number of minutes that the local time zone is offset from UTC|
