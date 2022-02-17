---
date: "2022-02-17T00:00:00+01:00"
draft: false
linktitle: Functions in Python
menu:
  example_tech:
    parent: Python Tips 
    weight: 1
title: Functions in Python
image:
  caption: ""
  focal_point: ""
toc: true
type: docs
weight: 1
---

## Docstrings 

```{python}
def function(arg_1, arg_2=42):
""" Description of function

Args: 
   arg_1(str): description of argument one
   arg_2(int, optional): 
   
Returns:
   bool: description of return value
   
Raises:
   ValueError: include errors that function intentionally raises (if applicable)
   
Note: any notes here
"""
```

## Immutable vs Mutable

### Immutable
- int
- float
- bool
- string
- bytes
- tuple
- frozenset
- None

### Mutable
- list
- dict
- set
- bytearray
- objects
- functions


## Context Managers (function-based)

Creating a context manager, basic structure:
1. Define a function
2. (optional) Add any setup code your context needs
3. Use the 'yield' keyword
4. (optional) Add any teardown code your context needs
5. Add the `@contextlib.contextmanager` **decorator**.

### Yield keyword

```{python}
@contextlib.contextmanager
def my_context():
    # Add any set up code you need
    yield
    # Add any teardown code you need
```

### Open a file
Generic structure and example using `open()`

```{python}
# generic structure
with <context-manager>(<args>) as <variable-name>:
    # Run code here
    # This code is running 'inside the context'
# This code runs after the context is removed

with open('my_file.txt') as my_file:
    text = my_file.read()
    length = len(text)
    
print('The file is {} characters long'.format(length))
```

### Read file

```{python}
@contextlib.context.manager
def open_read_only(filename):
    """description: open a file in read-only mode
    
    args: filename(str): location of file to read
    
    yields: file object
    """
    read_only_file = open(filename, mode='r')
    # Yield read_only_file so it can be assigned to my_file
    yield read_only_file
    # close read_only_file
    read_only_file.close()
    
with open_read_only('my_file.txt') as my_file:
    print(my_file.read())
```

### Count words

```{python}
with open("alice.txt") as file:
    text = file.read()
    
n = 0
for word in text.split():
    if word.lower() in ['cat', 'cats']:
        n += 1

print('Lewis Carroll used the word "cat" {} times'.format(n))

```
### Set a timer

Time how long a function takes to run

```{python}
image = get_image_from_instagram()

# time how long Numpy takes to run
with timer():
    print('Numpy version')
    process_with_numpy(image)

# timer with context manager
@contextlib.contextmanager
def timer():
    """description: time the execution of a context block
    
    Yields: None
    """
    start = time.time()
    # send control back to the context block
    yield
    end = time.time()
    print('Elapsed: {:.2f}s'.format(end - start))
    
with timer():
    print('This should take approx 0.25 seconds')
    time.sleep(0.25)
```



### Setup and teardown (db connection example)

```{python}
@contextlib.contextmanager
def database(url):
    # set up database connection
    db = postgres.connect(url)
    
    yield db
    
    # tear down database connection
    db.disconnect()

url = 'http://domain.name/data'
with database(url) as my_db:
    course_list = my_db.execute(
    'SELECT * FROM courses'
    )
```

### Using a decorator

## Advanced Topics

### Nested Contexts

Use nested loop with a context manager. This is legal in python.

```{python}
def copy(src, dst):
    """copy the content of one file to another
    
    args: 
        src (str): file name of file to be copied
        dst (str): where to write the new file
    """
    # open both files
    with open(src) as f_src:
        with open(dst) as f_dst:
            # read and write each line, one at a time
            for line in f_src:
                f_dst.write(line)
```

### Handling errors

Use:
- try
- except
- finally

```{python}
def get_printer(ip):
    p = connect_to_printer(ip)
    
    try:
        yield
    finally:
        p.disconnect()
        print('disconnected from printer')
        
doc = {'text': 'This is a test.'}

with get_printer('10.0.34.111') as printer:
    printer.print_page(doc['txt'])
```

### Patterns

- Open Close
- Lock Release
- Change Reset
- Enter Exit
- Start Stop
- Setup Teardown
- Connect Disconnect

## Decorators

Decorators use:
- functions as objects
- nested functions
- nonlocal scope
- closures

### Lists and dictionaries of functions

- store function in a dictionary

```{python}
dict_of_functions = {
    'funct1': open,
    'funct2': print
}

dict_of_functions['funct2']('I am printing with the value of a dictionary')

```

- pass function into another function

## Scope

### global

Use **global** key word to update a variable from inside the function:

```{python}
call_count = 0

def my_function():
    # use a keyword that lets us update call_count
    global call_count
    call_count += 1
    
    print("You've called my_function() {} times!".format(
        call_count
    ))

for _ in range(20):
    my_function()
```

### nonlocal

Use **nonlocal** keyword to update a variable in nested function.


```{python}
def read_files():
    file_contents = None
    
    def save_contents(filename):
        # Add a keyword that lets us modify file_contents
        nonlocal file_contents
        if file_contents is None:
            file_contents = []
        with open(filename) as fin:
            file_contents.append(fin.read())
            
    for filename in ['bitcoin.txt', 'sovindividual.txt' ,'smartcontracts.txt']:
        save_contents(filename)
    
    return file_contents
    
print('\n'.join(read_files()))
```

## Closures

A closure is python's way of attaching a **nonlocal** variable to a return function so that the function can operate even when called outside of parent scope.

```{python}
def parent(arg_1, arg_2):
    value = 22
    my_dict = {'chocolate': 'yummy'}
    
    def child():
        print(2 * value)
        print(my_dict['chocolate'])
        print(arg_1 + arg_2)
        
    return child
    
new_function = parent(3, 4)

print([cell.cell_contents for cell in new_function.__closure__])
```

### deletion


```{python}
```

### overwriting


```{python}
```


