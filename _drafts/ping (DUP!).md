# ping (DUP!)
今天在复习网络安全 发现了一个问题

`ping -l 66000 baidu.com`

```
64 bytes from 220.181.38.148: icmp_seq=30127 ttl=49 time=139.065 ms
64 bytes from 220.181.38.148: icmp_seq=30124 ttl=49 time=139.104 ms
64 bytes from 220.181.38.148: icmp_seq=30125 ttl=49 time=139.104 ms
64 bytes from 220.181.38.148: icmp_seq=30128 ttl=49 time=139.083 ms
64 bytes from 220.181.38.148: icmp_seq=33620 ttl=49 time=126.597 ms
64 bytes from 220.181.38.148: icmp_seq=30129 ttl=49 time=139.084 ms
64 bytes from 220.181.38.148: icmp_seq=33858 ttl=49 time=125.633 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=33863 ttl=49 time=125.623 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=33854 ttl=49 time=125.655 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=30131 ttl=49 time=139.091 ms
64 bytes from 220.181.38.148: icmp_seq=33857 ttl=49 time=125.653 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=33621 ttl=49 time=126.618 ms
64 bytes from 220.181.38.148: icmp_seq=34434 ttl=49 time=123.715 ms
64 bytes from 220.181.38.148: icmp_seq=34437 ttl=49 time=123.706 ms
64 bytes from 220.181.38.148: icmp_seq=34438 ttl=49 time=123.707 ms
64 bytes from 220.181.38.148: icmp_seq=34440 ttl=49 time=123.703 ms
64 bytes from 220.181.38.148: icmp_seq=34449 ttl=49 time=123.676 ms
64 bytes from 220.181.38.148: icmp_seq=34448 ttl=49 time=123.682 ms
64 bytes from 220.181.38.148: icmp_seq=34446 ttl=49 time=123.692 ms
64 bytes from 220.181.38.148: icmp_seq=34451 ttl=49 time=123.678 ms
64 bytes from 220.181.38.148: icmp_seq=34453 ttl=49 time=123.674 ms
64 bytes from 220.181.38.148: icmp_seq=45835 ttl=49 time=81.020 ms
64 bytes from 220.181.38.148: icmp_seq=45837 ttl=49 time=81.016 ms
64 bytes from 220.181.38.148: icmp_seq=45839 ttl=49 time=81.011 ms
64 bytes from 220.181.38.148: icmp_seq=45841 ttl=49 time=81.008 ms
64 bytes from 220.181.38.148: icmp_seq=45851 ttl=49 time=82.185 ms
64 bytes from 220.181.38.148: icmp_seq=45861 ttl=49 time=82.167 ms
64 bytes from 220.181.38.148: icmp_seq=45863 ttl=49 time=82.162 ms
64 bytes from 220.181.38.148: icmp_seq=45853 ttl=49 time=82.199 ms
64 bytes from 220.181.38.148: icmp_seq=46458 ttl=49 time=123.013 ms
64 bytes from 220.181.38.148: icmp_seq=46460 ttl=49 time=123.038 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46459 ttl=49 time=123.050 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46461 ttl=49 time=123.046 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46465 ttl=49 time=123.036 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46468 ttl=49 time=123.028 ms
64 bytes from 220.181.38.148: icmp_seq=46471 ttl=49 time=123.020 ms
64 bytes from 220.181.38.148: icmp_seq=46470 ttl=49 time=123.027 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46474 ttl=49 time=123.015 ms
64 bytes from 220.181.38.148: icmp_seq=46483 ttl=49 time=122.988 ms
64 bytes from 220.181.38.148: icmp_seq=46482 ttl=49 time=122.994 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46487 ttl=49 time=122.980 ms
64 bytes from 220.181.38.148: icmp_seq=46501 ttl=49 time=122.930 ms
64 bytes from 220.181.38.148: icmp_seq=46512 ttl=49 time=122.887 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=46513 ttl=49 time=122.888 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=50657 ttl=49 time=106.100 ms
64 bytes from 220.181.38.148: icmp_seq=50658 ttl=49 time=106.099 ms
64 bytes from 220.181.38.148: icmp_seq=50656 ttl=49 time=106.109 ms
64 bytes from 220.181.38.148: icmp_seq=50659 ttl=49 time=106.102 ms
64 bytes from 220.181.38.148: icmp_seq=51401 ttl=49 time=103.572 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51400 ttl=49 time=103.578 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51399 ttl=49 time=103.584 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51403 ttl=49 time=103.575 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51402 ttl=49 time=103.580 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51405 ttl=49 time=103.573 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51406 ttl=49 time=103.573 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51404 ttl=49 time=103.582 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51407 ttl=49 time=103.575 ms
64 bytes from 220.181.38.148: icmp_seq=51409 ttl=49 time=103.572 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51415 ttl=49 time=103.554 ms
64 bytes from 220.181.38.148: icmp_seq=51408 ttl=49 time=103.581 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51414 ttl=49 time=103.564 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51410 ttl=49 time=103.580 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51411 ttl=49 time=103.586 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51420 ttl=49 time=103.560 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51423 ttl=49 time=103.553 ms
64 bytes from 220.181.38.148: icmp_seq=51427 ttl=49 time=103.542 ms
64 bytes from 220.181.38.148: icmp_seq=51431 ttl=49 time=103.532 ms
64 bytes from 220.181.38.148: icmp_seq=51429 ttl=49 time=103.542 ms
64 bytes from 220.181.38.148: icmp_seq=51460 ttl=49 time=103.439 ms
64 bytes from 220.181.38.148: icmp_seq=51862 ttl=49 time=101.532 ms
64 bytes from 220.181.38.148: icmp_seq=51867 ttl=49 time=101.503 ms
64 bytes from 220.181.38.148: icmp_seq=51861 ttl=49 time=101.540 ms (DUP!)
64 bytes from 220.181.38.148: icmp_seq=51860 ttl=49 tim
```
但是正常ping没有问题，原因未知