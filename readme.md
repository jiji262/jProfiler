
The normal way to JavaScript Profiling
---------------------
(from the book "High Performance JavaScript")

Using the Date object, a measurement can be taken at any given point in a script. Before other tools existed, this was a common way to time script execution, and it is still occasionally useful. By default the Date object returns the current time, and subtracting one Date instance from another gives the elapsed time in milliseconds.

For example:

```javascript
var start = new Date(),
    count = 10000,
    i, element, time;

for (i = 0; i < count; i++) {
  element = document.createElement_x('div');
}

time = new Date() - start;
alert('created ' + count + ' in ' + time + 'ms');

start = new Date();
for (i = 0, i < count; i++) {
  element = element.cloneNode(false);
}

time = new Date() - start;
alert('created ' + count + ' in ' + time + 'ms');
```

This type of profiling is cumbersome to implement, as it requires manually instrumenting your own timing code. A Timer object that handles the time calculations and stores the data would be a good next step.

```javascript

Var Timer = {
  _data: {},

  start: function(key) {
    Timer._data[key] = new Date();
  },

  stop: function(key) {
    var time = Timer._data[key];
    if (time) {
      Timer._data[key] = new Date() - time;
    }
  },

  getTime: function(key) {
    return Timer._data[key];
  }
};

Timer.start('createElement_x');
for (i = 0; i < count; i++) {
  element = document.createElement_x('div');
}

Timer.stop('createElement_x');
alert('created ' + count + ' in ' + Timer.getTime('createElement_x');
```
As you can see, this still requires manual instrumentation, but provides a pattern for building a pure JavaScript profiler. 


YUI Profiler
------------------ 
The YUI Profiler (http://developer.yahoo.com/yui/profiler/), contributed by Nicholas Zakas, is a JavaScript profiler written in JavaScript. In addition to timer functionality, it provides interfaces for profiling functions, objects, and constructors, as well as detailed reports of the profile data. It enables profiling across various browsers and data exporting for more robust reporting and analysis.

jProfile
---------------------
Based on the implementation of YUI Profile (V2), we got the tool jProfile, which can be run without YUI library included, and only one file need to be added to page.

Usage
-----------------
To use the Profiler, include the following source files in your web page:

```javascript
<script src="js/jprofiler-min.js" type="text/javascript"></script>
```

Profiling Objects
----------------------
When an object exists with multiple methods to be profiled, it may be faster to call registerObject(), which registers every method found on the object. This can be especially useful in the case of object literals and inheritance done without using prototypes. The first argument is the name of the object (its name in the profiler) while the second argument is the actual object. Each method is registered as objectName.methodName in the profiler. Example:
```javascript
//object
var obj = {
 
    add : function (num1, num2) {
        return num1 + num2;
    },
 
    subtract : function (num1, num2){
        return num1 - num2;
    }    
};
 
//register the object
jProfiler.registerObject("obj", obj);
 
//use the methods
var sum = obj.add(5, 10);
var diff = obj.subtract(20, 12);
var sum2 = obj.add(10, 40);
 
//get the information
jProfile.showReport(); 
```

In this example, an object obj contains two methods, add() and subtract(). Both methods are registered when obj is passed into the registerObject() method. Information about the methods is then returned via getCallCount() by passing in the complete method names of obj.add and obj.subtract.

Also, we can use registerConstructor and registerFunction to do profiling on constructor or specific functions.
Please refer to YUI profile tool documentation: http://developer.yahoo.com/yui/profiler/

The functions in jProfile:
-------------------------
registerFunction

registerConstructor

registerObject

getAverage(name) - returns the average amount of time (in milliseconds) that the function takes to complete.

getCallCount(name) - returns the number of times that the given function was called.

getMax(name) - returns the minimum amount of time (in milliseconds) that the function takes to complete.

getMin(name) - returns the maximum amount of time (in milliseconds) that the function takes to complete.

getFunctionReport(name) - returns an object containing all of the profiling information for the function.

showReport() - show the report in console

unregisterFunction

unregisterConstructor

unregisterObject

clear()


