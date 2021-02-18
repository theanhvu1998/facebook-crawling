# Facebook Crawling with Python

> Demo: https://www.youtube.com/watch?v=Fx0UWOzYsig

## Features:

1.  Getting information of posts.
2.  Filtering comments.
3.  Not required sign in.
4.  Checking redirect and RATE LIMIT EXCEEDED response
5.  Running with Incognito window.
6.  Simplifying Browser to minimize time complexity.
7.  Using proxies to prevent from banning by:
    -   Collecting proxies and filtering the slowest ones from:
        -   http://proxyfor.eu/geo.php
        -   http://free-proxy-list.net
        -   http://rebro.weebly.com/proxy-list.html
        -   http://www.samair.ru/proxy/time-01.htm
        -   https://www.sslproxies.org
    -   [Tor Relays](https://github.com/18520339/facebook-crawling/tree/master/tor) which used in [Tor Browser](https://www.torproject.org/), a network is comprised of thousands of volunteer-run servers.

## Usage:

### I. Install library:

    pip install -r requirement.txt

-   [Helium](https://github.com/mherrmann/selenium-python-helium): a wrapper around [Selenium](https://selenium-python.readthedocs.io/) with more high-level API for web automation.
-   [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer): used for collecting free proxies.

### II. Customize parameters in crawler.py:

1.  **Running Browser**:

    -   **PAGE_URL**: url of Facebook page.
    -   **TOR_PATH**: use proxy with Tor for `WINDOWS` / `MAC` / `LINUX` / `NONE`:
    -   **BROWSER_OPTIONS**: run scripts using `CHROME` / `FIREFOX`.
    -   **PRIVATE**: run with private mode:
        -   Prevent [Selenium](https://selenium-python.readthedocs.io/) detection &#10153; **navigator.driver** must be _undefined_ (check in Dev Tools).
        -   Start Browser with **Incognito** / **Private Window**.
    -   **USE_PROXY**: run with proxy or not. If **True** &#10153; check:
        -   IF **TOR_PATH** &ne; `NONE` &#10153; Use Tor's SOCKS proxy server.
        -   ELSE &#10153; Randomize proxies with [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer).
    -   **HEADLESS**: run with headless Browser or not.
    -   **SPEED_UP**: simplify Browser for minizing loading time:

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

2.  **Loading Page**:

    -   **SCROLL_DOWN**: number of times to scroll for **view more posts**.
    -   **FILTER_CMTS_BY**: filter comments by `MOST_RELEVANT` / `NEWEST` / `ALL_COMMENTS`.
    -   **VIEW_MORE_CMTS**: number of times to click **view more comments**.
    -   **VIEW_MORE_REPLIES**: number of times to click **view more replies**.

    ```js
    // Wait 3s for each click to simulate "human like" behavior
    const clickBtn = btn => setTimeout(() => btn.click(), 3000);
    document.querySelectorAll('{selector}').forEach(btn => clickBtn(btn));
    ```

### III. Start crawling:

    python crawler.py

-   Most of my success tests was on **Firefox** with [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer) proxy server
-   Run at sign out state, cause some CSS Selectors will be different as sign in.
-   With some proxies, it might be quite slow or required to sign in.
-   Each post will be written line by line when completed. Data Field:

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
                    "user_id": "",
                    "user_name": "",
                    "text": ""
                }
            ]
        }
    ]
}
```

## Test Proxy Server:

1.  With [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer):

```python
from browser import *
page_url = 'http://check.torproject.org'
browser_options = BROWSER_OPTIONS.FIREFOX

setup_free_proxy(page_url, browser_options)
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

![](https://github.com/18520339/facebook-crawling/blob/master/test_proxy.png?raw=true)
