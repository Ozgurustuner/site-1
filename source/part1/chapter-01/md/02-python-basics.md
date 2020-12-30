---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.6.0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #region deletable=true editable=true -->
# Basic elements of Python

In this section we will introduce some basic programming concepts in Python.
<!-- #endregion -->

<!-- #region deletable=true editable=true -->
## Simple Python math

We will start our Python introduction by learning a bit of the basic operations you can perform. Python can be used as a simple calculator. Let's try it out with some simple math operations such as ``1 + 1`` or ``5 * 7``.  When using a Jupyter Notebook you can press **Shift-Enter** to execute the code cells. 
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false}
1 + 1
```

```python deletable=true editable=true jupyter={"outputs_hidden": false}
5 * 7
```

<!-- #region deletable=true editable=true -->
If you want to edit and re-run some code, simply make changes to the Python cell and press **Shift-Enter** to execute the modified code.
<!-- #endregion -->

<!-- #region deletable=true editable=true -->
### Functions

You can use Python for more advanced math by using a *{term}`function`*. Functions are pieces of code that perform a single action such as printing information to the screen (e.g., the `print()` function). Functions exist for a huge number of operations in Python.

Let's try out a few simple examples using functions to find the sine or square root of a value using the `sin()` and `sqrt()` functions.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false} tags=["raises-exception"]
sin(3)
```

```python deletable=true editable=true jupyter={"outputs_hidden": false} tags=["raises-exception"]
sqrt(4)
```

<!-- #region deletable=true editable=true -->
Wait, what? Python can’t calculate square roots or do basic trigonometry? Of course it can, but we need one more step.
<!-- #endregion -->

<!-- #region deletable=true editable=true -->
### Math operations

The list of basic arithmetic operations that can be done by default in Python are listed in table 1.1.

| Operation      | Symbol | Example syntax | Returned value |
| -------------- | ------ | -------------- | -------------- |
| Addition       | `+`    | `2 + 2`        | `4`            |
| Subtraction    | `-`    | `4 - 2`        | `2`            |
| Multiplication | `*`    | `2 * 3`        | `6`            | 
| Division       | `/`    | `4 / 2`        | `2`            |
| Exponentiation | `**`   | `2**3`         | `8`            |

**Table 1.1**. Basic math operations in Python.

For anything more advanced, we need to load a *{term}`module`* or *{term}`library`*. For math operations, this module is called *math* and it can be loaded by typing `import math`.
<!-- #endregion -->

```python deletable=true editable=true
import math
```

Now that we have access to functions in the math module, we can use it by typing the module name, a period (dot), and the the name of the function we want to use. For example, ``math.sin(3)``.

```python deletable=true editable=true jupyter={"outputs_hidden": false}
math.sin(3)
```

```python deletable=true editable=true jupyter={"outputs_hidden": false}
math.sqrt(4)
```

<!-- #region deletable=true editable=true -->
Let's summarize what we've just seen with modules:

1. A *module* is a group of code items such as functions that are related to one another. Individual modules are often in a group referred to as a *library*.

2. Modules can be loaded using the ``import`` statement. Functions that are part of the module ``modulename`` can then be used by typing ``modulename.functionname()``. For example, ``sin()`` is a function that is part of the ``math`` module, and used by typing ``math.sin()`` with some number between the parentheses.

3. In a Jupyter Notebook the variables you define earlier code cells will be available for use in the cells that follow as long as you have already executed the cells.

Note that modules may also contain constants such as ``math.pi``. Parentheses are not used when calling constant values.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false}
math.pi
```

#### Check your understanding

Use your Python skills to calculate the sine of pi. What value do you expect for this calculation? Did you get the expected result?

```python
# Note that lines starting with "#" are ignored in Python.
# An example solution can be found below.
```

```python deletable=true editable=true jupyter={"outputs_hidden": false} tags=["hide_cell"]
# Solution

math.sin(math.pi)
```

<!-- #region deletable=true editable=true -->
## Combining functions

Functions can also be combined. The ``print()`` function returns values within the parentheses as text on the screen. Let's print the value of the square root of four.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false}
print(math.sqrt(4))
```

You can also combine text with other calculated values using the ``print()`` function. For example, ``print('Two plus two is', 2+2)`` would generate text reading 'Two plus two is 4'. Let's combine the ``print()`` function with the ``math.sqrt()`` function in to produce text that reads `The square root of 4 is 2.0`.

```python deletable=true editable=true jupyter={"outputs_hidden": false}
print('The square root of 4 is', math.sqrt(4))
```

<!-- #region deletable=true editable=true -->
## Variables

A *{term}`variable`* can be used to store values calculated in expressions and used for other calculations.

### Variable assignment

Assigning value to variables is straightforward. To assign a value, you simply type ``variable_name = value``, where ``variable_name`` is the name of the variable you wish to define. Let's define a variable called ``temp_celsius`` and assign it a value of '10.0'. After this, we can print that variable value using the ``print()`` function.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false}
temp_celsius = 10.0
print(temp_celsius)
```

It is also possible to combine text and numbers and even use some math when printing out variable values. The idea is similar to the examples of adding 2+2 or calculating the square root of four from the previous section. Next, we will print out the value of ``temp_celsius`` in degrees Fahrenheit by multiplying ``temp_celsius`` by 9/5 and adding 32. This should be done within the ``print()`` function to produce output that reads 'Temperature in Fahrenheit: 50.0'.

```python deletable=true editable=true jupyter={"outputs_hidden": false}
print('Temperature in Fahrenheit:', 9/5 * temp_celsius + 32)
```

<!-- #region deletable=true editable=true -->
#### Check your understanding

Define a variable and print its value to the screen using the `print()` function. The variable value can be anything you like, and you can even consider defining several variables and printing them out together. Consider using pothole_case_naming for your variable name.
<!-- #endregion -->

```python
# An example solution can be found below.
```

```python deletable=true editable=true tags=["hide_cell"]
# Example solution

my_variable = "Python is cool!"
print(my_variable)
```

<!-- #region deletable=true editable=true -->
### Updating variables

Values stored in variables can also be updated. Let's redefine the value of ``temp_celsius`` to be equal to 15.0 and print its value using the `print()` function.
<!-- #endregion -->

```python deletable=true editable=true
temp_celsius = 15.0
```

```python deletable=true editable=true jupyter={"outputs_hidden": false}
print('temperature in Celsius is now:', temp_celsius)
```

<!-- #region deletable=true editable=true -->
Please note that if you try to run some code that accesses a variable that has not yet been defined you will get a `NameError` message.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false} tags=["raises-exception"]
print('Temperature in Celsius:', 5/9 * (tempFahrenheit - 32))
```

<!-- #region deletable=true editable=true -->
When running the code in a Jupyter Notebook variables get stored in memory only after executing the code cell where the variable is defined. 
<!-- #endregion -->

```python deletable=true editable=true
tempFahrenheit = 9/5 * temp_celsius + 32
```

Now that we have defined `tempFahrenheit`, we can run again the print statement without getting a `NameError`. Let's print out the values of `temp_celsius` and `tempFahrenheit` to check their current values.

```python deletable=true editable=true jupyter={"outputs_hidden": false}
print('temperature in Celsius:', temp_celsius, 'and in Fahrenheit:', tempFahrenheit)
```

The number beside the cell, for example `In [2]`, tells you the order in which the Python cells have been executed. This way you can see a history of the order in which you have run the cells.

<!-- #region deletable=true editable=true -->
### Variable values

Changing the value of a variable does not affect other variable values. Let's redefine ``temp_celsius`` to be equal to 20.0, and print out the values of ``temp_celsius`` and ``tempFahrenheit``.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false}
temp_celsius = 20.0
print('temperature in Celsius is now:', temp_celsius, 'and temperature in Fahrenheit is still:', tempFahrenheit)
```

<!-- #region deletable=true editable=true -->
## Data types

A *{term}`data type`* determines the characteristics of data in a program.
There are 4 basic data types in Python as shown in table 1.2.

| Data type name | Data type            | Example    |
| -------------- | -------------------- | ---------- |
| `int`          | Whole integer values | `4`        |
| `float`        | Decimal values       | `3.1415`   |
| `str`          | Character strings    | `'Hot'`    |
| `bool`         | True/false values    | `True`     |

**Table 1.2**. Basic data types in Python.

The data type can be found using the `type()` function. As you will see, the data types are important because some are not compatible with one another. Let's define a variable ``weatherForecast`` and assign it the value ``'Hot'``. After this, we can check its data type using the ``type()`` function.
<!-- #endregion -->

```python deletable=true editable=true jupyter={"outputs_hidden": false}
weatherForecast = 'Hot'
type(weatherForecast)
```

Let's also check the type of ``tempFahrenheit``. What happens if you try to combine ``tempFahrenheit`` and ``weatherForecast`` in a single math equation such as ``tempFahrenheit = tempFahrenheit + 5.0 * weatherForecast``?

```python deletable=true editable=true jupyter={"outputs_hidden": false} tags=["raises-exception"]
type(tempFahrenheit)
tempFahrenheit = tempFahrenheit + 5.0 * weatherForecast
```

<!-- #region deletable=true editable=true -->
In this case we get at ``TypeError`` because we are trying to execute a math operation with data types that are not compatible. There is no way in Python to multpily decimal values with a character string.
<!-- #endregion -->

<!-- #region deletable=true editable=true -->
#### Check your understanding

As it turns out, it is possible some math with character strings in Python. Define two variables and assign them character string values. What happens if you try to add two character strings together? Can you subtract them? Which other math operations work for character strings?
<!-- #endregion -->

```python
# An example solution can be found in the cell below.
```

```python deletable=true editable=true tags=["hide_cell"]
# Solution

first_variable = "Python"
second_variable = " is cool!"

print(first_variable + second_variable)
print(5 * first_variable)
print(first_variable - second_variable)
```

## Data types revisited

We learned a bit about variables and their values earlier in this section. We now continue with some variables related to [Finnish Meteorological Institute (FMI) observation stations](http://en.ilmatieteenlaitos.fi/observation-stations)[^FMI_stations]. For each station, a number of pieces of information are given, including the name of the station, an FMI station ID number (FMISID), its latitude, its longitude, and the station type. We can store this information and some additional information for a given station in Python as follows:

```python
station_name = 'Helsinki Kaivopuisto'
```

```python
station_id = 132310
```

```python
station_lat = 60.15
```

```python
station_lon = 24.96
```

```python
station_type = 'Mareographs'
```

Here we have 5 values assigned to variables related to a single observation station. Each variable has a unique name and they can store different types of data.


### Reminder: Data types and their compatibility

We can explore the different types of data stored in variables using the `type()` function.
Let's check the data types of the variables `station_name`, `station_id`, and `station_lat`.

```python
type(station_name)
```

```python
type(station_id)
```

```python
type(station_lat)
```

As expected, we see that the `station_name` is a character string, the `station_id` is an integer, and the `station_lat` is a floating point number. Being aware of the data type of variables is important because some are not compatible with one another. Let's see what happens if we try to sum up the variables `station_name` and `station_id`.

```python tags=["raises-exception"]
station_name + station_id
```

Here we get a `TypeError` because Python does not know to combine a string of characters (`station_name`) with an integer value (`station_id`).


### Converting data from one type to another

It is not the case that things like the `station_name` and `station_id` cannot be combined at all, but in order to combine a character string with a number we need to perform a *data type conversion* to make them compatible. Let's convert `station_id` to a character string using the `str()` function. We can store the converted variable as `station_id_str`.

```python
station_id_str = str(station_id)
```

We can confirm the type has changed by checking the type of `station_id_str`, or by checking the output of a code cell with the variable.

```python
type(station_id_str)
```

```python
station_id_str
```

As you can see, `str()` converts a numerical value into a character string with the same numbers as before. Similar to using `str()` to convert numbers to character strings, `int()` can be used to convert strings or floating point numbers to integers and `float()` can be used to convert strings or integers to floating point numbers.


### Combining text and numbers

Although most mathematical operations operate on numerical values, a common way to combine character strings is using the addition operator `+`. Let's create a text string in the variable `station_name_and_id` that is the combination of the `station_name` and `station_id` variables. Once we define `station_name_and_id`, we can print it to the screen to see the result.

```python
station_name_and_id = station_name + ": " + str(station_id)
```

```python
print(station_name_and_id)
```

Note that here we are converting `station_id` to a character string using the `str()` function within the assignment to the variable `station_name_and_id`. Alternatively, we could have simply added `station_name` and `station_id_str`.


## Lists and indices

Above we have seen a bit of data related to one of several FMI observation stations in the Helsinki area. Rather than having individual variables for each of those stations, we can store many related values in a *collection*. The simplest type of collection in Python is a *{term}`list`*.


### Creating a list

Let’s first create a list of selected `station_name` values and print it to the screen.

```python
station_names = ['Helsinki Harmaja', 'Helsinki Kaisaniemi', 'Helsinki Kaivopuisto', 'Helsinki Kumpula']
```

```python
print(station_names)
```

We can also check the type of the `station_names` list using the `type()` function.

```python
type(station_names)
```

Here we have a list of 4 `station_name` values in a list called `station_names`. As you can see, the `type()` function recognizes this as a list. Lists can be created using the square brackets `[` and `]`, with commas separating the values in the list.


### Index values

To access an individual value in the list we need to use an {term}`index (taulukko)` value. An index value is a number that refers to a given position in the list. Let’s check out the first value in our list as an example by printing out `station_names[1]`:

```python
print(station_names[1])
```

Wait, what? This is the second value in the list we’ve created, what is wrong? As it turns out, Python (and many other programming languages) start values stored in collections with the index value `0`. Thus, to get the value for the first item in the list, we must use index `0`. Let's print out the value at index `0` of `station_names`.

```python
print(station_names[0])
```

OK, that makes sense, but it may take some getting used to...

<!-- #region -->
### A useful analog - Bill the vending machine

As it turns out, index values are extremely useful, common in many programming languages, yet often a point of confusion for new programmers. Thus, we need to have a trick for remembering what an index value is and how they are used. For this, we need to be introduced to Bill (Figure 1.8).


![**Figure 1.8**. Bill, the vending machine.](../img/bill-the-vending-machine.png)


As you can see, Bill is a vending machine that contains 6 items. Like Python lists, the list of items available from Bill starts at 0 and increases in increments of 1.

The way Bill works is that you insert your money, then select the location of the item you wish to receive. In an analogy to Python, we could say Bill is simply a list of food items and the buttons you push to get them are the index values. For example, if you would like to buy a taco from Bill, you would push button `3`. If we had a Python list called `Bill`, an equivalent operation could simply be

```python
print(Bill[3])
Taco
```
<!-- #endregion -->

### Number of items in a list

We can find the length of a list using the `len()` function.

```python
len(station_names)
```

Just as expected, there are 4 values in our list and `len(station_names)` returns a value of `4`.


### Index value tips

If we know the length of the list, we can now use it to find the value of the last item in the list, right? What happens if you print the value from the `station_names` list at index `4`, the value of the length of the list?

```python tags=["raises-exception"]
print(station_names[4])
```

An `IndexError`? That’s right, since our list starts with index `0` and has 4 values, the index of the last item in the list is `len(station_names) - 1`. That isn’t ideal, but fortunately there’s a nice trick in Python to find the last item in a list. Let's first print the `station_names` list to remind us of the values that are in it.

```python
print(station_names)
```

To find the value at the end of the list, we can print the value at index `-1`. To go further up the list in reverse, we can simply use larger negative numbers, such as index `-4`.

```python
print(station_names[-1])
```

```python
print(station_names[-4])
```

Yes, in Python you can go backwards through lists by using negative index values. Index `-1` gives the last value in the list and index `-len(station_names)` would give the first. Of course, you still need to keep the index values within their ranges. What happens if you check the value at index `-5`?

```python tags=["raises-exception"]
print(station_names[-5])
```

### Modifying list values

Another nice feature of lists is that they are *mutable*, meaning that the values in a list that has been defined can be modified. Consider a list of the observation station types corresponding to the station names in the `station_names` list.

```python
station_types = ['Weather stations', 'Weather stations', 'Weather stations', 'Weather stations']
print(station_types)
```

Let's change the value for `station_types[2]` to be `'Mareographs'` and print out the `station_types` list again.

```python
station_types[2] = 'Mareographs'
print(station_types)
```

### Data types in lists

Lists can also store more than one type of data. Let’s consider that in addition to having a list of each station name, FMISID, latitude, etc. we would like to have a list of all of the values for station ‘Helsinki Kaivopuisto’.

```python
station_hel_kaivo = [station_name, station_id, station_lat, station_lon, station_type]
print(station_hel_kaivo)
```

Here we have one list with 3 different types of data in it. We can confirm this using the `type()` function. Let's check the type of `station_hel_kaivo` and the types of the values at indices `0-2`.

```python
type(station_hel_kaivo)
```

```python
type(station_hel_kaivo[0])    # The station name
```

```python
type(station_hel_kaivo[1])    # The FMISID
```

```python
type(station_hel_kaivo[2])    # The station latitude
```

### Adding and removing values from lists

Finally, we can add and remove values from lists to change their lengths. Let’s consider that we no longer want to include the first value in the `station_names` list. Since we haven't see that list in a bit, let's first print it to the screen.

```python
print(station_names)
```

`del` allows values in lists to be removed. It can also be used to delete values from memory in Python. To remove the first value from the `station_names` list, we can simply type `del station_names[0]`. If you then print out the `station_names` list, you should see the first value has been removed.

```python
del station_names[0]
```

```python
print(station_names)
```

If we would instead like to add a few samples to the `station_names` list, we can type `station_names.append('List item to add')`, where `'List item to add'` would be the text that would be added to the list in this example. Let's add two values to our list: `'Helsinki lighthouse'` and `'Helsinki Malmi airfield'` and check the list contents after this.

```python
station_names.append('Helsinki lighthouse')
station_names.append('Helsinki Malmi airfield')
```

```python
print(station_names)
```

As you can see, we add values one at a time using `station_names.append()`. `list.append()` is called a method in Python, which is a function that works for a given data type (a list in this case).


### Appending to an integer? Not so fast...

Let’s consider our list `station_names`. As we know, we already have data in the list `station_names`, and we can modify that data using built-in methods such as `station_names.append()`. In this case, the method `append()` is something that exists for lists, but not for other data types. It is intuitive that you might like to add (or append) things to a list, but perhaps it does not make sense to append to other data types. Let's create a variable `station_name_length` that we can use to store the length of the list `station_names`. We can then print the value of `station_name_length` to confirm the length is correct.

```python
station_name_length = len(station_names)
```

```python
print(station_name_length)
```

If we check the data type of `station_name_length`, we can see it is an integer value, as expected.

```python
type(station_name_length)
```

Let's see what happens if we try to append the value `1` to `station_name_length`.

```python tags=["raises-exception"]
station_name_length.append(1)
```

Here we get an `AttributeError` because there is no method built in to the `int` data type to append to `int` data. While `append()` makes sense for `list` data, it is not sensible for `int` data, which is the reason no such method exists for `int` data.


### Some other useful list methods

With lists we can do a number of useful things, such as count the number of times a value occurs in a list or where it occurs. The `list.count()` method can be used to find the number of instances of an item in a list. For instance, we can check to see how many times `'Helsinki Kumpula'` occurs in our list `station_names` by typing `station_names.count('Helsinki Kumpula')`. 

```python
station_names.count('Helsinki Kumpula')    
```

Similarly, we can use the `list.index()` method to find the index value of a given item in a list. Let's find the index of `'Helsinki Kumpula'` in the `station_names` list.

```python
station_names.index('Helsinki Kumpula')    
```

The good news here is that our selected station name is only in the list once. Should we need to modify it for some reason, we also now know where it is in the list (index `2`).

There are two other common methods for lists that we need to see. 


### Reversing a list

The `list.reverse()` method can be used to reverse the order of items in a list. Let's reverse our `station_names` list and then print the results.

```python
station_names.reverse()
```

```python
print(station_names)
```

Yay, it works! A common mistake when reversing lists is to do something like `station_names = station_names.reverse()`. **Do not do this!** When reversing lists with `.reverse()` the `None` value is returned (this is why there is no screen ouput when running `station_names.reverse()`). If you then assign the output of `station_names.reverse()` to `station_names` you will reverse the list, but then overwrite its contents with the returned value `None`. This means you’ve deleted the contents of your list.


### Sorting a list

The `list.sort()` method works in a similary way as reversing a list. Let's sort our `station_names` list and print its contents.

```python
station_names.sort()   # Notice no output here...
```

```python
print(station_names)
```

As you can see, the list has been sorted alphabetically using the `list.sort()` method, but there is no screen output when this occurs. Again, if you were to assign that output to `station_names` the list would get sorted, but the contents would then be assigned `None`. As you may have noticed, `Helsinki Malmi airfield` comes before `Helsinki lighthouse` in the sorted list. This is because alphabetical sorting in Python places capital letters before lowercase letters.


## Footnotes

[^swc]: <http://software-carpentry.org>
[^jupyter]: <https://jupyter.org/>
[^jupyterlab]: <http://jupyterlab.readthedocs.io/en/stable/>
[^markdown]: <https://en.wikipedia.org/wiki/Markdown>
[^FMI_stations]: <http://en.ilmatieteenlaitos.fi/observation-stations>
