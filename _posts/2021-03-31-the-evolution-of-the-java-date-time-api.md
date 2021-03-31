---
layout: post
title: "The Evolution of the Java Date Time API"
author: bagrat
categories: [ Programming Languages ]
image: assets/images/the-evolution-of-the-java-date-time-api/0_viIQXek8y4BjhOPN.jpeg
tags: [programming, coding, software development, java, time]
---

Hello guys! Today I want to write about the **time** and how it was and it is currently treated with Java.
Many developers (me first some time ago) don’t have clear ideas when to deal with time, especially before Java 8 was released.
So I would like to retrace the path and the reasons that led from having the old Date class until today with more useful time API and classes.

#### The Ancient Period — Date

<p align="center">
    <img src="{{ site.baseurl }}/assets/images/the-evolution-of-the-java-date-time-api/0_P-ZLgxPRGGOHUeS4.jpeg" width="640">
</p>

When the very first version of Java was released (Java 1.0), a simple **java.util.Date** class was provided to deal with time.
By [definition](https://docs.oracle.com/javase/8/docs/api/java/util/Date.html), this class actually doesn’t represent a
date but a specific instant in time, with millisecond precision.

**Years start from 1900, months are zero-index based** and both of these design decisions brought developers to make many errors when using Date.

For example to get an instance representing the date “10 Jun 2020”, you would have to call this constructor :

```
Date myDate = new Date(120, 5, 10);

//Date myDate = new Date(2020-1900, 6-1, 10);
```

And with just this simple example, you can catch the problems : years were so far from the reality, 
months were zero-index based meanwhile days were not, then there was a lack of coherence.

Another problem is the lack of any kind of control or validation of what is given as input to the constructor. 
In fact, you can call the constructor passing the date “31 Jun 2020” and get an instance with a date that actually is 
“01 Jul 2020” , as follows :

```
Date myDate = new Date(120, 5, 31);

/* you will get the same result of
Date myDate = new Date(120, 6, 1);
*/
```

Finally, the methods to interpret and parse dates was lacking of a simple way of localizing and internationalizing dates.

#### The Middle Ages — Calendar

<p align="center">
    <img src="{{ site.baseurl }}/assets/images/the-evolution-of-the-java-date-time-api/0_W1xUHfhNoqhfEcVu.jpeg" width="640">
</p>

Fortunately, very bad things were suddenly recognized and in order to overcome all these problems, 
in the subsequent version of Java 1.1 the **java.util.Calendar** class was provided. Despite the good intentions, 
though some of the Date limitations were resolved, others came out taking the developers to the next level of making errors.

##### Problems solved
- the year offset is managed by the Calendar class
- many constants were added to deal with days or months

##### Problems remained or introduced
- months are still zero-index based
- Calendar class is mutable then there is a thread-safety problem
- it is still problematic to do some calculations as intervals or differences between dates in a simple manner
- java.text.DateFormat were introduced to parse the string dates but also this is not thread-safe
- managing zoned dates still gives developers many headaches

#### The Modern Era— The new Date Time API

<p align="center">
    <img src="{{ site.baseurl }}/assets/images/the-evolution-of-the-java-date-time-api/0_ZjcYito-OsZE_Ww6.jpeg" width="640">
</p>

With the **JDK 8** was integrated a set of specifications (**JSR 310**) with the following purposes :
- provide an effective API for the developers
- supporting standard time concepts as date, time, instant and time-zone
- immutable implementations for thread-safety

With these goals in mind, there was provided a new package, **java.time**, in which you can find all the Date Time API classes, 
which are all immutable and thread-safe. In the diagram below you can see the core classes to deal with dates and time starting from 
[*Java 8*](https://medium.com/hackernoon/top-5-java-8-courses-to-learn-online-2db57d9dfb8d).

<p align="center">
    <a href="https://www.java67.com/2017/08/top-10-date-time-and-calendar-Java-Interview-Questions.html">
        <img src="{{ site.baseurl }}/assets/images/the-evolution-of-the-java-date-time-api/1_js8vnlPpJomv83V-n2Zk5A.png" width="640">
    </a>
</p>

**LocalDate** : its instance is an immutable object representing a plain date without time of the day and store the date in the YYYY-MM-DD format. 
An instance of this class can be created in many ways, as follows :

```
LocalDate.now(); //get the current date
LocalDate.now(ZoneId.of("Europe/Rome")); //get the current date in a specific zone
LocalDate.of(2020,06,10); //get a date from int values
LocalDate.parse("2020-06-10"); //get a date from string
```

**LocalTime** : it is similar to LocalDate, but it represents only the time of the day without time zone details and stores 
the time in the HH:mm:ss.nanos format. An instance of this class can be created as follows:

```
LocalTime.now();//get the current time
LocalTime.now(ZoneId.of("Europe/Rome"));//get the current time in a specific zone
LocalTime.of(8, 30, 15, 10);//get an instance for 8h 30m 15s 10ns
LocalTime.parse("08:30:15.12345");
```

**LocalDateTime** : this is the combination of the previous two, holding both date and time parts without timezone details. 
The datetime is stored in the YYYY-MM-DDThh:mm:ss and can be created as follows:

```
LocalDateTime.now();//get the current datetime
LocalDateTime.parse("2020-06-10T08:15:30");//get a datetime parsing the string
```

Most of the methods you can use with LocalDate and LocalTime are available also with LocalDateTime class and you can also get a 
LocalDate or a LocalTime instance starting from a LocalDateTime.
**Instant** : it is a specific point in the continuous timeline. It represents the seconds passed since the Epoch time 
1970–01–01T00:00:00Z and internally stores two values :

- a long value representing seconds from the Epoch time
- an int value representing the nanoseconds of seconds

An Instant instance can be created as follows :

```
Instant.now();
Instant.ofEpochSecond(long epochSecond);
```

#### Formatting and Parsing
Now we can format and parse dates and time with the help of two classes : **DateTimeFormatter** (most popular) and **DateTimeFormatterBuilder**. 
The first is the substitute of the old java.text.DateFormat and mostly provides three methods :

- ofPattern(String pattern) : creates a formatter using the specified pattern; it returns a DateTimeFormatter instance.
- format(TemporalAccessor temporal) : formats a date-time object using this formatter; it returns a String.
- parse(CharSequence text) : fully parses the text producing a temporal object; it returns a instance of TemporalAccessor type.

##### Time Intervals

With the old Date and Calendar classes, it was difficult to deal with calculation of intervals between two dates. 
Now Java provides the **TemporalAmount** interface which represents an amount of time and it is implemented by the two classes **Duration** and **Period**.

**Duration** : it is the amount of time in terms of seconds and nanoseconds; it has utility methods to get respective days, 
hours, minutes, millis and nanos; it also provides a “**between**” method to compute duration among two temporal objects.

**Period** : it is the amount of time in terms of years, months and days; it has useful getters and other methods

To calculate the days between two LocalDate you can use the following :

```
dateA.until(dateB, ChronoUnit.DAYS)
```

#### Time Zones
Until Java 7, the java.util.TimeZone class was used together with Calendar but the usage was not simple. With Java 8 we have many classes to deal with time zones as follows :
- ZoneID : defines a unique id for a region-city pair
    — example : Europe/Rome
- ZoneOffset : represents a timezone with an offset from Greenwich/UTC
    — example : +03:00
- ZonedDateTime : represents a date time in the ISO-8601 calendar system with time zone
    — example : 2020–06–10T08:00:15+01:00 Europe/Rome
- OffsetDateTime : represents a date time with an offset from UTC/Greenwich in the ISO-8601 calendar system
    — example : 2020–06–10T08:00:15+01:00
- OffsetTime : represents a time with an offset from UTC/Greenwich in the ISO-8601 calendar system
    — example : 08:00:15+01:00

#### Conclusions
Well, I want to conclude inviting you and your side-by-side developers to deepen the [Java Date Time API](https://docs.oracle.com/javase/8/docs/api/java/time/package-summary.html) 
because at least once in your life it will be useful for sure.I hope you enjoyed this story, you found it useful and you liked the images 
going from looking the sun to having a modern smartwatch. Bye!
