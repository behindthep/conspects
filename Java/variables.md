## Variables

1) Magic numbers

Create vars for numbers used in expression:
```java
var euros = 1000;
var dollars = euros * 1.25; // How would you understand 1.25 when looking at it after a month?

var euroToDollarRate = 1.25;
var dollars = euros * euroToDollarRate;
```


2) Constants

Declare data that never changes:
```java
final var pi = 3.14;
```
