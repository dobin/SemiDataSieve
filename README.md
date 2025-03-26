# Semi Structured Data Sieve

Web interface to 
Sort and Filter Records of Semi-Structured Data (array of dictionaries, which have several identical keys). 

Available at: [semidatasieve.r00ted.ch](https://semidatasieve.r00ted.ch)

100% pure JavaScript, no dependencies.


## Screenshot

![Screenshot](https://raw.githubusercontent.com/dobin/SemiDataSieve/master/doc/screenshot.png)

![Screenshot](https://raw.githubusercontent.com/dobin/SemiDataSieve/master/doc/screenshot2.png)


## Filter 

* Include keys
* Exclude keys
* Include values
* Exclude values

Excludes take precedence.


## Example

```
[
    { id: 1, timestamp: "12:00:00", level: "INFO", message: "System started" },
    { id: 2, timestamp: "12:05:00", level: "WARNING", message: "High memory usage detected" },
    { id: 3, timestamp: "12:10:00", level: "ERROR", message: "System crash" },
    { id: 4, timestamp: "12:10:00", nolevel: "ERROR", stuff: "0x1234" }
]
```

* With "include key:level", #1, #2, #3 will be shown
* With "exclude key:nolevel", #4 will be hidden
* With "include value:error", #1, #2, #3 will be shown
* With "exclude value:error", #4 will be hidden

Multiple: 
* "include key:level": #1, #2, #3 will be shown
* "exclude value:ERROR": #3 will not be shown, only #1 and #2


