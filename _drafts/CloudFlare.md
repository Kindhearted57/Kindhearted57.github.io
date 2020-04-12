# CloudFlare
## CDN(Content Delivery Network)
## CloudFlare防护实例
### 五秒盾
### 机器人验证
## 反爬技术
### 使用Python库cloudflare-scrape
[github主页](https://github.com/Anorov/cloudflare-scrape)  
`pip install cfscrape`安装`cloudflare-scrape`  
处理get请求的CloudFlare
```
import cfscrape  
scraper = cfscrape.create_scraper()
web_data = scraper.get("https://youraddresstoscrapehere.com").content
```