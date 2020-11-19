---
title: "Ether crawler"
key: 2020-03-31-LDA-error
tags: Ethereum
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---

# crawler connection problem

## etherscan crawler

	
    I've written this crawler to collect some contracts information from [etherscan](https://etherscan.io).
However, a few months later, I can't access this website anymore. At first I thought that's because I've got too much information from this website, so I'm banned from this website. However, even if my ip has adjusted, I still can't access this website through my crawler.  
Later I found out that this is not some kind of connection problem. At the beginning this website is available without proxy, but now I have to access it with proxy. So that's the problem. There are a lot of differences with or without a proxy.  
Here is my code at the beginning:

```
url = 'https://etherscan.io/address/'+address[i]+'#code'    
strhtml = requests.get(url).text
```

    address here is a list that stores all of the addresses I wanted to collect information from.
And the error is as follows:

```
Traceback (most recent call last):
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connection.py", line 157, in _new_conn
    (self._dns_host, self.port), self.timeout, **extra_kw
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/util/connection.py", line 84, in create_connection
    raise err
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/util/connection.py", line 74, in create_connection
    sock.connect(sa)
TimeoutError: [Errno 60] Operation timed out

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connectionpool.py", line 672, in urlopen
    chunked=chunked,
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connectionpool.py", line 376, in _make_request
    self._validate_conn(conn)
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connectionpool.py", line 994, in _validate_conn
    conn.connect()
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connection.py", line 300, in connect
    conn = self._new_conn()
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connection.py", line 169, in _new_conn
    self, "Failed to establish a new connection: %s" % e
urllib3.exceptions.NewConnectionError: <urllib3.connection.VerifiedHTTPSConnection object at 0x107a3e490>: Failed to establish a new connection: [Errno 60] Operation timed out

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/requests/adapters.py", line 449, in send
    timeout=timeout
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/connectionpool.py", line 720, in urlopen
    method, url, error=e, _pool=self, _stacktrace=sys.exc_info()[2]
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/urllib3/util/retry.py", line 436, in increment
    raise MaxRetryError(_pool, url, error or ResponseError(cause))
urllib3.exceptions.MaxRetryError: HTTPSConnectionPool(host='etherscan.io', port=443): Max retries exceeded with url: /address/0x43335787c62aaaa9194a8c00c82e034094240340 (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x107a3e490>: Failed to establish a new connection: [Errno 60] Operation timed out'))

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/jiru/Desktop/Desktop/假充值漏洞/ethereum_so_far/憨批正则版脚本.py", line 117, in <module>
    strhtml = s.get(url).text
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/requests/sessions.py", line 543, in get
    return self.request('GET', url, **kwargs)
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/requests/sessions.py", line 530, in request
    resp = self.send(prep, **send_kwargs)
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/requests/sessions.py", line 643, in send
    r = adapter.send(request, **kwargs)
  File "/Users/jiru/fake_deposit/lib/python3.7/site-packages/requests/adapters.py", line 516, in send
    raise ConnectionError(e, request=request)
requests.exceptions.ConnectionError: HTTPSConnectionPool(host='etherscan.io', port=443): Max retries exceeded with url: /address/0x43335787c62aaaa9194a8c00c82e034094240340 (Caused by NewConnectionError('<urllib3.connection.VerifiedHTTPSConnection object at 0x107a3e490>: Failed to establish a new connection: [Errno 60] Operation timed out'))

```
    Here are some ways to solve this problem on the internet, and most of them can be concluded to the following ways:
[Some answers from stackoverflow](https://stackoverflow.com/questions/23013220/max-retries-exceeded-with-url-in-requests)

* To disable SSL verification.
* Close the redundant connections.

    These ways might solve the problem in another situation. As for mine, I need to solve the proxy problem.  
    I follow [this site](https://segmentfault.com/q/1010000008986220) to deal with this problem.  
    At first I write proxies like this:

```
    proxies = {
    'https': 'https://127.0.0.1:1080',  
    'http': 'http://127.0.0.1:1080'. 
    }
```
    However, it still won't work. But I later found out that because I'm using shadowsocks---which uses socks protocol. So I should write like this in my code:

```    
import request

proxies = {
    'http': 'socks5://127.0.0.1:10808',
    'https': 'socks5://127.0.0.1:10808'
}
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'
}
url = 'https://etherscan.io//address/0x43335787c62aaaa9194a8c00c82e034094240340#code'
resp = requests.get(url, proxies=proxies, headers=headers).text
print(resp)s
```

    This time and finally, it worked !!!!!!!!!!!!

    
## The differences between VPN and Proxy
    [VPN vs Proxy:BIG Difference!](https://www.youtube.com/watch?v=Z8LSy66fklk)  
    VPN are more secure than proxy. There are different protocol that proxy will use: HTTP(some may not allow HTTPS) HTTPS(encrypted, only support web traffic)SOCKS(support any types of traffic)
    
## Why I always have to tick off the sock proxy in order to use proxy properly?
    I use shadowsocks as proxy.  
    Sometimes, with the proxy I'm unable to access the internet, so here is the method I follow and works everytime, but I never wonder why.
    ![way to deal with unable problem]()
      
    I just tick off the socks proxy button everytime I got into trouble. But now I think that must be the port that is not correct. It's different from the App on my laptop(that's why here 127.0.0.1 is used). So this setting is basically meaningless, it's used to set some parameters. It's like this:  
    
    ```
    a = 1         
    a = 2
    ```
      
    And the setting process in preference is like the second line of the above code. It can cover the settings in the shadowsocks app.
## Missing dependencies for SOCKS
`pip install pysocks`

  
    
