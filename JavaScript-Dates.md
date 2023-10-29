## JavaScript Dates

### Creating dates in JavaScript

We can create a JavaScript date for the current point of time:

    const now = new Date(); // right now
    console.log(now.toString()); // Sun Sep 29 2019 10:42:01 GMT+1000 (Australian Eastern Standard Time)

We can also specify a date in time.

    const particularDate = new Date('January 21 2001 6:25:12');

There are a number of methods for getting parts of JavaScript dates.

    console.log(`Year: ${now.getFullYear()}`); // Year: 2019

    console.log(`Month: ${now.getMonth() + 1}`); // Month: 9

Note that month values a zero based so we have to add 1 to get the correct month value.

    console.log(`Day: ${now.getDay()}`); // Day: 0 (Sunday)
    
    console.log(`Day of the month: ${now.getDate()}`); // Day of the month: 29

    console.log(`Hours: ${now.getHours()}`); // Hours: 10

    console.log(`Minutes: ${now.getMinutes()}`); // Minutes: 42

    console.log(`Seconds: ${now.getSeconds()}`); // Seconds: 1

### Working with Timestamps

A timestamp is nothing more than a number. This number represents the number of milliseconds since January 1st 1970 at midnight. Positive numbers go forward in time from that point. A date in 2018 would represent a positive number. A date in 1969 would represent a negative number.

Timestamps are handy because you only have to hand around a single number.

You can create a timestamp from a date with the following.

    const timestamp = now.getTime();
    const today = new Date(timestamp);

    console.log(`Year: ${today.getFullYear()}`); // Year: 2019

### Working with dates

1. Create 2 dates in the past (use string for date)
2. Get timesamps for both
3. Figure out which is first and print its time (use toString)

.

    const date1 = new Date('2018-02-09 12:25:20');

    const date2 = new Date('2017-08-10 14:20:40');  

    console.log(`date 1: ${date1.toString()}`); // date 1: Fri Feb 09 2018 12:25:20 GMT+1100 (Australian Eastern    Daylight Time)

    console.log(`date 2: ${date2.toString()}`); // date 2: Thu Aug 10 2017 14:20:40 GMT+1000 (Australian Eastern    Standard Time)  

    const date1Timestamp = date1.getTime();
    const date2Timestamp = date2.getTime();

    console.log(`date 1 timestamp: ${date1Timestamp}`); // date 2: Thu Aug 10 2017 14:20:40 GMT+1000 (Australian    Eastern Standard Time)
    console.log(`date 2 timestamp: ${date2Timestamp}`); // date 2 timestamp: 1502338840000  

    if (date1 < date2) {
        console.log(`date 1: ${date1.toString()}`);
    } else {
        console.log(`date 2: ${date2.toString()}`);
    }

    // result:
    date 2: Thu Aug 10 2017 14:20:40 GMT+1000 (Australian Eastern Standard Time)

### Working with Moment.js

Working with JavaScript dates isn't easy. A better way to work with dates is to use a third party library named Moment.js.

[Moment.js website](https://momentjs.com).

##### Installation

    npm install moment --save

or

    <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment-with-locales.min.js"></script>

You can create a moment:

    const nowDate = moment();

Then use moment functions to print the results.

    console.log(nowDate.toString()); // Sun Sep 29 2019 18:16:01 GMT+1000

    console.log(nowDate.weekday()); // 0

    console.log(nowDate.isoWeekday()); // 7

    console.log(nowDate.minutes()); // 21

    console.log(nowDate.hours()); // 18

    console.log(nowDate.get('year')); // 2019

You can create a moment with a date string.

    const date = moment('2019-11-03 12:00:00');
    console.log(date.toString());

Moment has a large number of date formatting options.

    console.log(date.format('MMMM Do, YYYY')); // November 3rd, 2019

    console.log(date.format('Do MMMM YYYY')); // 3rd November 2019

    console.log(date.format('Do of MMMM YYYY')); // 3rd of November 2019

    console.log(date.format('dddd MMMM YYYY')); // Sunday November 2019

    console.log(date.format('DD-MM-YYYY')); // 03-11-2019

    console.log(date.format('DD/MM/YYYY')); // 03/11/2019

    console.log(moment([2016, 07, 22]).format('DD/MM/YYYY')); // 22/08/2016 -- note: month is zero based

    console.log(moment([2016, 07, 22]).fromNow()); // 3 years ago

    nowDate.add(2, 'year').subtract(4, 'months').add(20, 'days'); 

    console.log(nowDate.toString()); // Wed May 12 2021 13:28:25 GMT+1000

    console.log(nowDate.fromNow()); // in 2 years

Create a timestamp.

    const nowTimestamp = nowDate.valueOf();

    console.log(nowTimestamp); // 1620790340567

    console.log(moment(nowTimestamp).toString()); // Wed May 12 2021 13:34:50 GMT+1000

    console.log(moment(nowTimestamp).format('(dddd) Do MMMM, YYYY')); // (Wednesday) 12th May, 2021

Work out how many days to my birthday next year.

    const currentDay = moment();

    const birthDay = moment([2020, 03, 17]);

    console.log(birthDay.format('Do MMMM, YYYY')); // 17th April, 2020

    console.log(birthDay.diff(currentDay).valueOf()); // 20599924849

    console.log(`Days to birthday: ${birthDay.diff(currentDay, 'days')}`); // Days to birthday: 238

Another way to create a timestamp is:

    const myTimestamp = moment().valueOf();
