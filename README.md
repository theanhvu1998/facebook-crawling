# Facebook Crawling with Python

> Demo: https://www.youtube.com/watch?v=Fx0UWOzYsig

## Features:

-   Get information of posts.
-   Filter comments.
-   Not required sign in.
-   Check redirect.
-   Run with Incognito window.
-   Simplify browser to minimize time complexity.
-   Use proxies to prevent from banning with:
    -   Random proxies from [Free Proxy List](https://free-proxy-list.net/) that are just checked and updated every 10 minutes.
    -   [Tor Relays](https://github.com/18520339/facebook-crawling/tree/master/tor) which used in [Tor Browser](https://www.torproject.org/), a network is comprised of thousands of volunteer-run servers.

## Usage:

### I. Install library:

    pip install -r requirement.txt

-   [Helium](https://github.com/mherrmann/selenium-python-helium): a wrapper around [Selenium](https://selenium-python.readthedocs.io/) with more high-level API for web automation.
-   [HTTP Request Randomizer](https://github.com/pgaref/HTTP_Request_Randomizer): used for getting proxies from [Free Proxy List](https://free-proxy-list.net/).

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
        -   ELSE &#10153; Get proxies from [Free Proxy List](https://free-proxy-list.net/).
    -   **HEADLESS**: run with header Browser or not.
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

    -   **SCROLL_DOWN**: number of scroll times for loading more posts.
    -   **FILTER_CMTS_BY**: show comments by `MOST_RELEVANT` / `NEWEST` / `ALL_COMMENTS`.
    -   **VIEW_MORE_CMTS**: number of times for loading more comments.
    -   **VIEW_MORE_REPLIES**: number of times for loading more replies.

### III. Start crawling:

    python crawler.py

-   Sign out Facebook (cause some CSS Selectors will be different as sign in).
-   Note that with some proxies, it might be quite slow or required to sign in.
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

## Testing:

1.  Last test on **Firefox** with **Incognito** mode using [Free Proxy List](https://free-proxy-list.net/):

    ![](https://github.com/18520339/facebook-crawling/blob/master/image/result.png?raw=true)

2.  Running **Proxy Server** with [Free Proxy List](https://free-proxy-list.net/):

```python
from browser import *
page_url = 'http://check.torproject.org'
request_proxy = RequestProxy()
browser_options = BROWSER_OPTIONS.FIREFOX

setup_free_proxy(page_url, request_proxy, browser_options)
# kill_browser()
```

3. Running **Proxy Server** with [Tor Relays](https://github.com/18520339/facebook-crawling/tree/master/tor):

```python
from browser import *
page_url = 'http://check.torproject.org'
tor_path = TOR_PATH.WINDOWS
browser_options = BROWSER_OPTIONS.FIREFOX

setup_tor_proxy(page_url, tor_path, browser_options)
# kill_browser()
```

![](https://github.com/18520339/facebook-crawling/blob/master/image/proxy.png?raw=true)
