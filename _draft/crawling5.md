### Crawling & Scraping 실습 - Crawling 편

crawling 실습

http://example.webscraping.com/places/default/index

~~~python
urls = []
seen = []
urls.append(url)

while urls:
    seed = urls.pop(0)
    seen.append(seed) #재방문 회피
    dom = BeautifulSoup(download(seed).text, 'html.parser')
   
    for _ in [_['href'] for _ in dom.select('a')
             if _.has_attr('href') 
              and _['href'].startswith('/')]: #내부 링크(href='#~') 회피
        newUrl = urljoin(seed, urlparse(_)[2])
        
        if newUrl not in seen and newUrl not in urls:
            urls.append(newUrl)
            
       #parse newUrl, do works with it
    
    print(len(urls),len(seen))
~~~



- href 중 중복되는 link는 제거 --> seen 리스트에 있으면 pass
- 찾은 link 내 존재하는 link를 다시 url 리스트에 add
- url 리스트가 empty될때까지 link 가져오기

