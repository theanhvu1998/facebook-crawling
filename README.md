# Facebook crawling with random proxy servers

Crawling id, user info, content, date, comments and replies of posts in a Facebook page

> Demo: https://www.youtube.com/watch?v=Fx0UWOzYsig

## Overview:

### I. Features:

1.  Getting information of posts.
2.  Filtering comments.
3.  Not required sign in.
4.  Checking redirect
5.  Running with **Incognito** window.
6.  Simplifying browser to minimize time complexity.
7.  Hiding IP address to prevent from banning by:
    -   Collecting proxies and filtering the slowest ones from:
        -   http://proxyfor.eu/geo.php
        -   http://free-proxy-list.net
        -   http://rebro.weebly.com/proxy-list.html
        -   http://www.samair.ru/proxy/time-01.htm
        -   https://www.sslproxies.org
    -   [Tor Relays](https://github.com/18520339/facebook-crawling/tree/master/tor) which used in [Tor Browser](https://www.torproject.org/), a network is comprised of thousands of volunteer-run servers. 

### II. Weaknesses:

-   Unable to handle some failed responses. Example: **RATE LIMIT EXCEEDED** response (Facebook prevents from loading more), ...
-   Usually failed to collect replies.
-   Quite slow when running with a large number of _loading more_.

### III. Result:

-   To archive large number of comments results:
    -   Load more posts to collect more comments in case failed to view more comments / replies.
    -   Should use browser without headless to detect failed responses.
-   Lastest run on **Firefox** with **Incognito** windows using [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer):

    ![](https://github.com/18520339/facebook-crawling/blob/master/img/result.png?raw=true)

<details>
    <summary><b>Data Field:</b></summary>
    
```json
{
    "url": "",
    "id": "",
    "utime": "",
    "text": "",
    "total_shares": "",
    "total_cmts": "",
    "reactions": [],
    "crawled_cmts": [
        {
            "id": "",
            "utime": "",
            "user_url": "",
            "user_id": "",
            "user_name": "",
            "text": "",
            "replies": [
                {
                    "id": "",
                    "utime": "",
                    "user_url": "",
                    "user_id": "",
                    "user_name": "",
                    "text": ""
                }
            ]
        }
    ]
}
```
</details>

## Usage:

### I. Install libraries:

    pip install -r requirements.txt

-   [Helium](https://github.com/mherrmann/selenium-python-helium): a wrapper around [Selenium](https://selenium-python.readthedocs.io/) with more high-level API for web automation.
-   [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer): used for collecting free proxies.

### II. Customize parameters in [crawler.py](https://github.com/18520339/facebook-crawling/blob/master/crawler.py):

1.  **Running browser**:

    -   **PAGE_URL**: url of Facebook page.
    -   **TOR_PATH**: use proxy with Tor for `WINDOWS` / `MAC` / `LINUX` / `NONE`:
    -   **BROWSER_OPTIONS**: run scripts using `CHROME` / `FIREFOX`.
    -   **PRIVATE**: run with private mode:
        -   Prevent [Selenium](https://selenium-python.readthedocs.io/) detection &#10153; **navigator.driver** must be _undefined_ (check in Dev Tools).
        -   Start browser with **Incognito** / **Private Window**.
    -   **USE_PROXY**: run with proxy or not. If **True** &#10153; check:
        -   IF **TOR_PATH** &ne; `NONE` &#10153; Use **Tor's SOCKS** proxy server.
        -   ELSE &#10153; Randomize proxies with [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer).
    -   **HEADLESS**: run with headless browser or not.
    -   **SPEED_UP**: simplify browser for minizing loading time:

        -   With **Chrome** :

        ```python
        # Disable loading image, CSS, ...
        browser_options.add_experimental_option('prefs', {
            "profile.managed_default_content_settings.images": 2,
            "profile.managed_default_content_settings.stylesheets": 2,
            "profile.managed_default_content_settings.cookies": 2,
            "profile.managed_default_content_settings.geolocation": 2,
            "profile.managed_default_content_settings.media_stream": 2,
            "profile.managed_default_content_settings.plugins": 1,
            "profile.default_content_setting_values.notifications": 2,
        })
        ```

        -   With **Firefox** :

        ```python
        # Disable loading image, CSS, Flash
        browser_options.set_preference('permissions.default.image', 2)
        browser_options.set_preference('permissions.default.stylesheet', 2)
        browser_options.set_preference('dom.ipc.plugins.enabled.libflashplayer.so', 'false')
        ```

2.  **Loading page**:

    -   **SCROLL_DOWN**: number of times to scroll for **view more posts**.
    -   **FILTER_CMTS_BY**: filter comments by `MOST_RELEVANT` / `NEWEST` / `ALL_COMMENTS`.
    -   **VIEW_MORE_CMTS**: number of times to click **view more comments**.
    -   **VIEW_MORE_REPLIES**: number of times to click **view more replies**.

### III. Start running:

    python crawler.py

-   Most of my success tests was on **Firefox** with [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer) proxy server
-   Run at sign out state, cause some CSS Selectors will be different as sign in.
-   With some proxies, it might be quite slow or required to sign in.
-   Each post will be written line by line when completed.

## Test proxy server:

1. With [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer):

```python
from browser import *
page_url = 'http://check.torproject.org'
proxy_server = random.choice(proxies).get_address()
browser_options = BROWSER_OPTIONS.FIREFOX

setup_free_proxy(page_url, proxy_server, browser_options)
# kill_browser()
```

2. With [Tor Relays](https://github.com/18520339/facebook-crawling/tree/master/tor):

```python
from browser import *
page_url = 'http://check.torproject.org'
tor_path = TOR_PATH.WINDOWS
browser_options = BROWSER_OPTIONS.FIREFOX

setup_tor_proxy(page_url, tor_path, browser_options)
# kill_browser()
```

![](https://github.com/18520339/facebook-crawling/blob/master/img/proxy.png?raw=true)
