---
layout: post
title:  "Async and Await in python"
date:   2018-07-08 00:50:25 -0600
---
### Just collecting the new stuff learnt
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
