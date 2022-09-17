---
title: "Calculate diffrence in days between two dates"
date: 2022-9-16T17:16:25+02:00
draft: false
---

## Calculate the difference in days between two dates

To calculate the difference in days between two dates, we will use the `diff()` method of the `DateTime` class.

The `diff()` method returns the difference between two `DateTime` objects as a `DateInterval` object.

The `DateInterval` object contains the difference in years, months, days, hours, minutes, and seconds.

The `days` property of the `DateInterval` object contains the difference in days between two dates.

Let's see an example:

```php
<?php
// Create two DateTime objects
$date1 = new DateTime("2022-09-17");
$date2 = new DateTime("2022-09-20");

// Calculate the difference between two dates
$diff = $date1->diff($date2);

// Display the difference in days
echo $diff->days;
?>
```

The output of the above code is:

```bash
3
```

Here is another example after converting to timestamp:

```php
<?php
$to_date = time();
$from_date = strtotime("2022-09-17");
$diff = $to_date - $from_date;
echo floor($diff / (60 * 60 * 24));
```

![diffrence between two dates](/content/images/diff-in-days-php.png)
