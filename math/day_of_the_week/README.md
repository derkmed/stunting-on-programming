# Days of the Week

<b>Question:</b>
<b>Given a date, return the corresponding day of the week for that date.
The input is given as three integers representing the day, month and year respectively.
Return the answer as one of the following values {"Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"}.</b>
<b>Note: The given dates are valid dates between the years 1971 and 2100.</b>

```
Input: day = 31, month = 8, year = 2019
Output: "Saturday"
```

Thought Process:
* There are a lot of cool formulas and methodologies for solving this. But, assuming we do not have access to those, what we can try to do is use the current day to our knowledge.
  * Let's assume today is `09/14/2019`. 
    * It is a `Saturday`: We can use this to build an array of days starting from today as a reference i.e. `dayNames = ["Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]`
    * It is `September`: We can use this to build an array of months as referenced by the number of days they hold i.e. `daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]`
  * We can use `12/31/1970` as a reference point for today's date to get the distance between these two dates.
  * We can also use it as a reference point for a target date to get the distance between these two dates.
  * Based on the number of days between these two dates, we now know what day of the week the target day resides on.

```python
class Solution:

  def dayOfTheWeek(self, day: int, month: int, year: int) -> str:
    def hasLeapDay(year):
      return 1 if year % 4 == 0 and (year % 100 != 0 or year % 400 == 0) else 0
		    
    dayNames = ["Saturday", "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]
    daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    
    # days since 31, 12, 1970
    def daysSinceStart(day, month, year):
        numDays = 0
        # handle all the years from 01/01/year
        for y in range(year - 1, 1970, -1):
            numDays += 365 + hasLeapDay(y)
            
        # handle the months from 01/01/year to month/01/year
        numDays += sum(daysInMonth[:month-1])
        if month > 2:    
            numDays += hasLeapDay(year)
            
        # handle the days from month/01/year to month/day/year
        numDays += day 

        return numDays

    knownStart = daysSinceStart(14,9,2019)
    d = daysSinceStart(day, month, year) 
    return dayNames[ (d - knownStart) % 7]

```

Topics = {Math}  
