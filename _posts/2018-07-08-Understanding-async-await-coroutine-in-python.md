---
layout: post
title:  "Async and Await in python"
date:   2018-07-08 00:50:25 -0600
---

### Start with Asyncio
Recently I've been hearing a lot buzz about asynchronous. The podcast I followed also mentioned python3's new asyncio couple times. It has been a long standing topic I wanted to discover. What is Async process and how it boost the performance of python programs?

#### Basic
Since I started learning python, I've been hearing all these buzz words related to high performance paradigm but never really understand what on earth they really mean. Here I briefly borrowed some definition from blogposts on Internet that I found really helpful for me.

<strong>Multiprocessing</strong> : spread tasks over processing units (CPUs, or cores). Multiprocessing is well-suited for CPU-bound tasks: loops and mathematical computations.

<strong>Threading</strong> : a concurrent execution model whereby multiple threads take turns executing tasks. One process can contain multiple threads.

<strong>Concurrency</strong> : multiple tasks run in an overlapping manner. (There’s a saying that concurrency does not imply parallelism.)

These short explanation helped clarify the biggest myth I had: essentially, async is not multiprocessing in its core.
Analogies I made for the differences are:

> Multiprocessing is like brain processing multiple information at the same time: eg driving in traffic, you will be measuring the distance between your car and the previous one and also watch out for the break lights.

> Concurrency is like people being able to work on multiple things altogether and switch in between to make tasks planned seamlessly, eg. while cooking, I will be switching between cutting, boiling and stirring regularly, instead of idling and waiting the whole time for water to boil.

So.. Asyncio is a style of concurrent programming, but it is not strictly parallelism. At the heart of async IO are coroutines. A coroutine is a specialized version of a Python generator function. (coroutine: a function that can suspend its execution before reaching return)

Asyncio was introduced in Python 3.5 and the API has been change here and there in different python released versions. Just need to keep that in mind.


### Just some new stuff learnt
Coroutines are something relate to threading. Asynchronous programming has been
on for a while in python. Most explained use case is in HTTP requests.

Python (>= python3.5) use `async` and `await` keyword(decorator) to declare the
function in coroutine manner.

Use the keyword when declaring the function and return. Cannot be used outside function

{% highlight python %}
async def ping_local():  
    return await ping_server('192.168.1.1')
{% endhighlight %}


So the above is the syntax for declaring async function. To actual invoke async function,
we need to start an event loop. Consider this as a generator.

From the internet, what **event loop** can do:

 * Register, execute, and cancel delayed calls (asynchronous functions)
 * Create client and server transports for communication
 * Create subprocesses and transports for communication with another program
 * Delegate function calls to a pool of threads


{% highlight python %}
#!/bin/bash/python3.5
import sys  
import asyncio  
import aiohttp  

loop = asyncio.get_event_loop()  
client = aiohttp.ClientSession(loop=loop)

async def get_json(client, url):
	async with client.get(url) as response:
		assert response.status == 200
		return await response.read()

asyncio.ensure_future(get_json(client, url_2))  
asyncio.ensure_future(get_json(client, url_1))  
loop.run_forever()
{% endhighlight %}
