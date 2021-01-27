# Python Coding Guidelines
Topics Covered
- [String Formatting](#string-formatting)
- 
## String Formatting
`Takeaway: Use f-strings (if using Python 3.6 or above)`

There are 3 ways to format strings in Python (ordered from least recommended to most recommended):
- [using `%`](#%-strings) (not recommended)
- [using `.format()`](#.format()-strings) (somewhat recommended depending on circumstances; has one advantage over f-strings)
- [using `f-strings`](#f-string) (most recommended)


### `%`-strings
```python
# code block
format_type = "%"
print("%s-string" % format_type)
# output
"%-string"
```
The % operator is the original way strings were formatted in Python, but Python has since moved away from it and has even discouraged use in some cases. For example, the [Python docs](https://docs.python.org/3.8/library/stdtypes.html#printf-style-string-formatting) for printf-style string formatting includes the following warning:
> The formatting operations described here exhibit a variety of quirks that lead to a number of common errors (such as failing to display tuples and dictionaries correctly). Using the newer formatted string literals, the str.format() interface, or template strings may help avoid these errors. Each of these alternatives provides their own trade-offs and benefits of simplicity, flexibility, and/or extensibility.

Python docs warning aside, un-intuitive syntax and strained readability are other reasons %-strings aren't the best.

Take a look at the following Python code and it's rather un-intuitive %-string syntax:
```python
# code block
scientist = "Daniel Lieberman"
duration = 20.8
percent = 50
full_string = "According to %s, moderate exercise (like walking) for only %d minutes a day lowers mortality rates by %i percent." % (scientist, duration, percent)
print(full_string)

# output
"According to Daniel Lieberman, moderate exercise (like walking) for only 20 minutes a day lowers mortality rates by 50 percent"
```
You'll notice that there's a variety of letters that can come after the % symbol, which indicate the type being displayed. The s in %s seems like a intuitive letter to represent the word "string", so you might think that the d in %d is used to represent decimals. Unfortunately, it actually indicates an integer, and you can see the result that the .8 from duration is missing from the output string. There are also a number of other syntatical letters (like %f and %i) that require some prior knowledge to use properly.

Syntax aside, if a string gets a bit long, it immediately becomes much harder to read/associate which variables goes where, as you can see in the above example.

In short, there's really no reason to be using the %-string formatt unless you're using a version of Python before Python 2.6.

### `.format()` strings
```python
# code block
format_type = ".format()"
string = "{}-string".format(format_type)
print(string)

# output
".format()-string"
```
Python 2.6 introduced another way of string formatting, using the .format() method. With this new method, you no longer had to know the type of that which you are trying to print, making things much easier. The replacement token uses curly braces (`{}`) instead of the percent sign (`%`) followed by a letter:
```python
#  old way (%-string)
>>> "According to %s, moderate exercise (like walking) for only %d minutes a day lowers mortality rates by %i percent." % (scientist, duration, percent)

# newer way (.format())
>>> "According to {}, moderate exercise (like walking) for only {} minutes a day lowers mortality rates by {} percent.".format(scientist, duration, percent)

# newer way (.format() with numbers to indicate variable indices)
>>> "According to {2}, moderate exercise (like walking) for only {1} minutes a day lowers mortality rates by {0} percent.".format(percent, scientist, duration)
```
While just using curly braces alone solves the problem of potentially having to worry about variable types, it doesn't quite solve problem of readability that %-strings had. That's where a cool trick with dictionaries comes in. You can store variables in a dictionary and then pass them into a string using the `**` operator, which is mighty handy when you have a lot of values to pass into a string:

```python
# code block
fi_numbers = {'goal': 'financial independence/retirement',
              'x-axis': 'Percentage of Income Saved',
              'y-axis': 'Years to Financial Independence',
              'graph': 'exponential decay',
              'increase_percentage': 5,
              'low_savings_rate': 10,
              'low_savings_years': 51,
              'low_savings_improvement': 8,
              'high_savings_rate': 65,
              'high_savings_years': 10.5,
              'high_savings_improvement': 2
              }
fi_prompt = "Your time to reach {goal} depends on only one factor: Your {x-axis}. And the good news is, it turns out that the graph of your {x-axis} to {y-axis} is {graph}. This means the same increase at the lower end of the savings spectrum creates a much larger impact than at the higher end. For example, saving just {increase_percentage}% more on an initial {low_savings_rate}% will allow 'retirement' to happen {low_savings_improvement} years earlier, while that same increase at {high_savings_rate}% will only shave off 2 years. That said, a higher savings rate is still desirable (saving at {low_savings_rate}% will take 51 years while {high_savings_rate}% will only take {high_savings_years}), if tempered by diminishing returns."
fi_string = fi_prompt.format(**fi_numbers)

# output
"Your time to reach financial independence/retirement depends on only one factor: Your Percentage of Income Saved. And the good news is, it turns out that the graph of your Percentage of Income Saved to Years to Financial Independence is exponential decay. This means the same increase at the lower end of the savings spectrum creates a much larger impact than at the higher end. For example, saving just 5% more on an initial 10% will allow 'retirement' to happen 8 years earlier, while that same increase at 65% will only shave off 2 years. That said, a higher savings rate is still desirable (saving at 10% will take 51 years while 65% will only take 10.5), if tempered by diminishing returns."
```

### `f`-strings
```python
# code block
format_type = "f"
string = f"{format_type}-string"
print(string)

# output
"f-string"
```