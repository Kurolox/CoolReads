<!-- Original Link: https://blog.rmotr.com/those-tricky-python-booleans-2100d5df92c -->

<!-- How I learned about this: https://www.reddit.com/r/learnpython/comments/a7veh0/whats_this_i12i2/ec61f4g -->

# Those tricky Python booleans
### Santiago Basulto, Jan 16, 2017

It’s always funny to see the reaction from our students to a coding assignment they have to do during the first week of our Intro to Python course. It’s a super simple coding exercise that reads something like:

>Write a function `what_type()` that receives a Python object and returns:
>
>* “integer” if the object’s type is ***int***
>* “string” if the object’s type is ***str***
>* “boolean” if the object’s type is ***bool***
>* “float” if the object’s type is ***float***

(If you want to try solving it by yourself, stop reading now and take a stab at it: http://learn.rmotr.com/python/introduction-to-programming-with-python/functions-intro/return-the-type-of-value).

This simple assignment makes them think about basic control flow logic and introduce them to the `elif` clause. One of the first things they try is using the `isinstance()` function to get something like:

```python
def which_type(obj):
    if isinstance(obj, int):
        return 'integer'
    elif isinstance(obj, str):
        return 'string'
    elif isinstance(obj, bool):
        return 'boolean'
    elif isinstance(obj, float):
return 'float'
```

If you read that logic carefully, it seems to work just fine. But if you try running it by yourself, you’ll realize it doesn’t work as expected.

The problem is the following line, which has a counter-intuitive result:

```python
if isinstance(True, int):
print("Wait a second 🤔")
```

And that’s because, in Python, **Booleans are a subtype of Integers**, which means that `isinstance(True, int)` returns `True` 😳.

# A little bit of Python History

To understand why booleans are a subtype of integers, we need to go back in time to the year 2002. At this point, the Python language doesn’t have boolean types and the only way to “mimic” boolean logic is by using the numbers `1` and `0` . When it became obvious that Python needed a “boolean type”, Guido proposed PEP 285: “Adding a bool type” (original email introducing it). Remember, this is year 2002; Python didn’t have the popularity that it has today, but there was still a fairly amount of code written (both by users and also for the CPython compiler). So the rationale was: if we make `bool` extend from `int`, we’ll maintain backward compatibility for our users and, equally important, “it’ll ease the implementation enormously”. Here’s the explanation given by Guido in his PEP:

![relevant extract from PEP](https://cdn-images-1.medium.com/max/1600/1*sosKBfRnhqJ9JWyaZYbDZg.png)
(https://www.python.org/dev/peps/pep-0285/)

Seems like there was a good amount of discussion over PEP 285, so Guido decided (after getting a fortune cookie and reading “Strong and bitter words indicate a weak cause”) that it was enough and accepted the proposal that would come to live in Python 2.3 (July 2003); release notes read:

 > *A new built-in type, bool, has been added, as well as built-in names for its two values, True and False. Comparisons and sundry other operations that return a truth value have been changed to return a bool instead. Read PEP 285 for an explanation of why this is backward compatible.*

# But that’s not the end of this story

If we go back to the release notes from Python 2.3.0 we read that `True` and `False` were added as “built-in **names**” for the bool type values. That means that, in Python 2, we can do something like (try it):

```python
True = 0

def is_active():
    return True

if is_active():
    print("This is active")
else:
    print("This is odd :/")
```

We are basically reassigning `True` as `0` (which is of course `False`) 😨. If you think this couldn’t get crazier, go ahead and try doing `True + 1` in your interpreter.

Thankfully, `True` and `False` were made constants in Python 3, which now prevents re-assigning them.

![Error message if you try that in newer python versions](https://cdn-images-1.medium.com/max/1600/1*kty_5Zxb_A2C4jMFd7mMdg.png)

We don’t know if the decision of making booleans subtypes of integers was a good one, but it’s all what we have. So if you ever need to check for both types, integers and booleans, make sure you place your boolean check at the top. The solution to our exercise would be:

Thanks @rosieshimada and Janice Shiu for proofreading this post.
