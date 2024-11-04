# webscraping

## trying out scrapy

### mostly memory notes

trial page: recently played on bbc

---

layout: false

## install

~~~
$ pip install scrapy
~~~

## install

~~~
$ scrapy startproject bbc
New Scrapy project 'bbc', using template directory '/home/olav/.venvs/bbc/lib/python3.9/site-packages/scrapy/templates/project', created in:
    /home/olav/dev/py/webscraping/bbc/bbc

You can start your first spider with:
    cd bbc
    scrapy genspider example example.com
~~~

~~~
$ cd bbc
$ scrapy genspider recent bbc.co.uk
Created spider 'recent' using template 'basic' in module:
  bbc.spiders.recent
~~~

---

## first version

~~~
# bbc/spiders/recent.py
import scrapy


class RecentSpider(scrapy.Spider):
    name = 'recent'
    allowed_domains = ['bbc.co.uk']
    start_urls = ['http://bbc.co.uk/sounds/play/live:bbc_6music']

    def parse(self, response):
        pass
~~~

Interactive testing

~~~
$ scrapy shell http://bbc.co.uk/sounds/play/live:bbc_6music
>>> response
<200 https://www.bbc.co.uk/sounds/play/live:bbc_6music>
>>> response.css('div.sc-c-track-artist')
[]
>>> response.css('div.sc-c-track__artist')
[<Selector xpath="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' sc-c-track__artist ')]" data='<div class="sc-c-track__artist gel-pi...'>, <Selector xpath="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' sc-c-track__artist ')]" data='<div class="sc-c-track__artist gel-pi...'>, <Selector xpath="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' sc-c-track__artist ')]" data='<div class="sc-c-track__artist gel-pi...'>, <Selector xpath="descendant-or-self::div[@class and contains(concat(' ', normalize-space(@class), ' '), ' sc-c-track__artist ')]" data='<div class="sc-c-track__artist gel-pi...'>]
>>> response.css('div.sc-c-track__artist::text').getall()
['The Future Sound of London', 'SAULT', 'Fatoumata Diawara', 'Talking Heads']
>>> response.css('div.sc-c-track__title::text').getall()
['Cascade', 'Fight For Love', 'Nsera (feat. Damon Albarn)', 'Crosseyed And Painless']

~~~

---

Put in spider

~~~
def parse(self, response):
        artists = response.css('div.sc-c-track__artist::text').getall()
        titles = response.css('div.sc-c-track__title::text').getall()
        for artist, title in zip(artists, titles):
            yield {
                'artist': artist,
                'title': title,
            }
~                                
~~~

and crawl to save in json

~~~
$ scrapy crawl recent -O recent.json
~~~

~~~
# recent.json
[
{"artist": "Romy", "title": "Strong (feat. Fred again..)"},
{"artist": "Luther Ingram", "title": "If It's All The Same To You Babe"},
{"artist": "Olan Monk", "title": "Can't Wait"},
{"artist": "Alewya", "title": "Let Go"}
]%             
~~~

---

Use line-json to append several crawols and -o option to append

~~~
$ scarpy crawl recent -o resent.jsonl
~~~
~~~
{"artist": "Olan Monk", "title": "Can't Wait"}
{"artist": "Alewya", "title": "Let Go"}
{"artist": "The Future Sound of London", "title": "Cascade"}
{"artist": "SAULT", "title": "Fight For Love"}
~~~

ok how to handle results to database

piplines genereated

~~~
class BBCPipeline:
      def process_item(self, item, spider):
          return item
~~~

In bbc/settings.py it has to be activated with

~~~
ITEM_PIPELINES = {
      'bbc.pipelines.BBCPipeline': 300,
}
~~~

setting up a db

~~~
from sqlalchemy import create_engine, Column, Integer, String                                         
from sqlalchemy.ext.declarative import declarative_base                                               
from sqlalchemy.orm import sessionmaker                                                               
                                                                                                      
engine = create_engine('sqlite:///recent.db')                                                         
Session = sessionmaker(bind=engine)                                                                   
session = Session()                                                                                   
                                                                                                      
Base = declarative_base()                                                                             
                                                                                                      
                                                                                                      
class Song(Base):                                                                                     
    __tablename__ = 'songs'                                                                           
                                                                                                      
    id = Column(Integer, primary_key=True)                                                            
    title = Column(String)                                                                            
    artist = Column(String)                         

class BBCPipeline:
    def process_item(self, item, spider):
        # adapter = ItemAdapter(item)
        song = Song(**item)
        session.add(song)
        session.commit()
        return item


Base.metadata.create_all(engine)
~~~

---

Still need to handle duplication

robots.txt error: set in settings

OBEY_ROBOTSTXT = False
