# CHAPTER 3
# Asyncio Walk-Through
Much of the confusion around asyncio in the community today is due to lack of
understanding of this difference. For instance, the official Python documentation for
asyncio is more appropriate for framework developers than end users. This means
that end-user developers reading those docs quickly become shell-shocked by the
apparent complexity.

My goal is to give you only the most basic understanding of the building blocks of
Asyncio—enough that you should be able to write simple programs with it, and cer‐
tainly enough that you will be able to dive into more complete references.

First up, we have a “quickstart” section that introduces the most important building
blocks for Asyncio applications.

## Quickstart
Yury Selivanov, the author of PEP 492 and all-round major contributor to async
Python, explained in his PyCon 2016 talk “async/await in Python 3.5 and Why It Is
Awesome,” that many of the APIs in the asyncio module are really intended for
framework designers, not end-user developers. In that talk, he emphasized the main
features that end users should care about. These are a small subset of the whole
asyncio API and can be summarized as follows:
	• Starting the asyncio event loop
	• Calling async/await functions
	• Creating a task to be run on the loop
	• Waiting for multiple tasks to complete
	• Closing the loop after all concurrent tasks have completed

The “Hello World” of Asyncio in Python:
```python
# quickstart.py
import asyncio, time

async def main():
	print(f'{time.ctime()} Hello!')
	await asyncio.sleep(1.0)
	print(f'{time.ctime()} Goodbye!')

asyncio.run(main())
```

```python
# quickstart.py
import asyncio, time


async def main():
    print(f'{time.ctime()} Hello!')
    await asyncio.sleep(1.0)
    print(f'{time.ctime()} Goodbye!')

# asyncio.run(main()) 

# Внизу то, что делает под капотом asyncio.run()
loop = asyncio.get_event_loop()
task = loop.create_task(main())
loop.run_until_complete(task)
pending = asyncio.all_tasks(loop=loop)
for task in pending:
    task.cancel()
group = asyncio.gather(*pending, return_exceptions=True)
loop.run_until_complete(group)
loop.close()
# Конец
```
