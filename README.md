# aiohttp-ip-rotator
An asynchronous alternative to the requests-ip-rotator  (https://github.com/Ge0rg3/requests-ip-rotator) library based on aiohttp, completely copying its functionality

## Installation
```commandline
pip install git+https://github.com/Karbo123/aiohttp-ip-rotator.git
```

## Example
```python3
from asyncio import gather, get_event_loop
from aiohttp_ip_rotator import RotatingClientSession

aws_access_key_id = "CPWASYAASNXXPVYZCSKN" # an example key
aws_secret_access_key = "zbRdrQ99t8ZAvBe2EK9ILu+Sh+dqm7s8EYqDAaIx" # an example secret key

base_url = "https://www.google.com"
session = RotatingClientSession(base_url, aws_access_key_id, aws_secret_access_key, verbose=True)

async def worker():
    response = await session.get(f"{base_url}/search?query=test")
    print(await response.text())

async def main():
    await gather(*[worker() for _ in range(50)], return_exceptions=True)

loop = get_event_loop()
loop.run_until_complete(session.start())
loop.run_until_complete(main())
loop.run_until_complete(session.close())
```

## Example 2
```python3
from asyncio import get_event_loop
from aiohttp_ip_rotator import RotatingClientSession


async def main():
    session = RotatingClientSession("https://api.ipify.org", "aws access key id", "aws access key secret")
    await session.start()
    for i in range(5):
        response = await session.get("https://api.ipify.org")
        print(f"Your ip: {await response.text()}")
    await session.close()


if __name__ == "__main__":
    get_event_loop().run_until_complete(main())
```
## Example 3
```python3
from asyncio import get_event_loop
from aiohttp_ip_rotator import RotatingClientSession


async def main():
    async with RotatingClientSession(
        "https://api.ipify.org",
        "aws access key id",
        "aws access key secret"
    ) as session:
        for i in range(5):
            response = await session.get("https://api.ipify.org")
            print(f"Your ip: {await response.text()}")


if __name__ == "__main__":
    get_event_loop().run_until_complete(main())
```
