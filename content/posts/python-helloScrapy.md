+++
title = 'Python-Scrapy'
date = 2024-08-24T16:33:42+03:00
draft = false
ShowToc = true
ShowBreadCrumbs = true
+++

[Course Video - (Youtube link)](https://www.youtube.com/watch?v=mBoX_JCKZTE)

[Scrape Free Website](https://books.toscrape.com)

My OS is windows 11, course shots macos. Some points differences. 

Step 0 | Install Enviroments
--|--
write space : visual studio code ide | https://visualstudio.microsoft.com
1 : Install python | https://www.python.org/downloads/
2 : Install pip as package manager | https://pip.pypa.io/en/stable/installation/
3 : Install Virtual Enviroment ~~~~ `python -m pip install --user virtualenv`| https://virtualenv.pypa.io/en/latest/installation.html
4 : Install Scrapy using with pip | Inside the virtual enviroment ~~~~~~~~~~ `pip install scrapy` https://scrapy.org/ 

### Step 1 : Create Virtual Enviroment
```    
python -m venv venv
# python -m venv[command] venv[folder name]
```
### Step 2 : Run & Close VEnv
* Run in linux or macos 
```
source venv/bin/activate
```
* Run in Windows 11
```
.\venv\Scripts\Activate.ps1
```
then terminal should look like this
![run_scrapy](/images/scrapy-01.png "run_scrapy") 
* Close VEnv in Windows 11
```
deactivate
```
### Step 3 : Install Scrapy with pip install inside VEnv
? did you activate your virtual enviroment
```
pip install scrapy
```
Check instillation `scrapy` then terminal must shows scrapy command list
![check_scrapy](/images/scrapy-02.png "check_scrapy")

### Step 4 : Creating A Scrapy Project
* How To Create A Scrapy Project
* Overview of The Scrapy Project (Video)
* Explaining Scrapy Spiders, Items & Item Pipelines (Video)
* Explaining Scrapy Middlewares & Settings (Video)
#### Create New Scrapy Project
```
scrapy startproject bookscraper
# scrapy startproject bookscraper[project name]                   
```
### Step 5 : Build First Scrapy Spider
? did you create scrape project 
* get down spider folder
  - In my case my scrape project name is `bookscraper`
  - ! 2 times bookscraper include
  - my cmd promt is `cd .\bookscraper\bookscraper\spiders\`
* Create Spider
```
scrapy genspider bookspider books.toscrape.com
# scrapy genspider[spider type?] 
# bookspider[spider name] 
# books.toscrape.com[url to target website]
```
![spider_created](/images/scrapy-03.png "spider_created")

* Created `.\bookscraper\bookscraper\spiders\bookspider.py` file

![template_spider](/images/scrapy-03_1.png "template_spider")

### Step 6 : Install & Open Easier access with Shell

* install shell
  * `pip install ipython`
* Open shell
  * `scrapy shell`
* Close shell (Return Powershell)
  * `exit`

# Part 4 (I) : First Scrapes with Shell 
⚠️ VEnv Started ?  Shell started ? 
  * Get data from "https://books.toscrape.com/"
    * `fetch('https://books.toscrape.com/')`
  * Get data form spesified css class
    *  `response.css('article.product_pod').get`

### Set Variable to showed books 

*  `books = response.css('article.product_pod')`
*  get the total number of books appering on a single page
   *  `len(books)` output = `20`

## Access Single Book variables

* access single book variables. Create new varaible and assing in books list
  * `book = books[0]`
### Example : Get Book Title   
![book_title_inspected-view](/images/scrapy-04.png "book_title_inspected-view")           
* inspect site and find where is the title
  * in this case ise h3 header inside a tag
  * get book title
    * `book.css('h3 a::text').get()` output = `A Light in the ...` 
### Example : Get Book Price
![book_price_inspected-view](/images/scrapy-05.png "book_price_inspected-view")
#### Method 1 : Get div and p type
* `book.css('div p::text').get()` output = `£51.77`
#### Method 2 : Get with class names
* `book.css('.product_price .price_color::text').get()` output = `£51.77`
### Example : Get Book page link
* `book.css('h3 a').attrib['href']` output = `catalogue/a-light-in-the-attic_1000/index.html`

# Part 4 (II) : Dive Into Scrape with Spider (Automate)

## Using bookspider.py

```
def parse(self, response):
        books =response.css('article.product_pod')

        for book in books:
            yield{
                'name' : book.css('h3 a::text').get(),
                'price' : book.css('.product_price .price_color::text').get(),
                'url': book.css('h3 a').attrib['href']
            }
```
run this program (run inside powershell, if 'scrapy shell' is open, firstly close it)
```
scrapy crawl bookspider
```
sample output 
```
2024-08-24 20:26:29 [scrapy.core.scraper] DEBUG: Scraped from <200 https://books.toscrape.com/>
{'name': 'A Light in the ...', 'price': '£51.77', 'url': 'catalogue/a-light-in-the-attic_1000/index.html'}
```

## Check Every Page with Recursive bookspider.py

```
def parse(self, response):
        books =response.css('article.product_pod')

        for book in books:
            yield{
                'name' : book.css('h3 a::text').get(),
                'price' : book.css('.product_price .price_color::text').get(),
                'url': book.css('h3 a').attrib['href']
            }

        next_page = response.css('li.next a ::attr(href)').get()
        # If page have next button enter if block
        if next_page is not None:
            # Some pages next button have 'catalogue/' but some buttons havent
            if 'catalogue/' in next_page:
                next_page_url = 'https://books.toscrape.com/' + next_page
            else:
                next_page_url = 'https://books.toscrape.com/catalogue/' + next_page
            yield response.follow(next_page_url, callback= self.parse)
```
if code works well, spider should have made scrape 1000 item  
![recursive_spider_result](/images/scrapy_part4_resultv2.png "recursive_spider_result")

# Part 5 : Build Discovery & Extraction Spider
⚠️ VEnv Started ?  Shell started ?  

Get every book page inside data.
* Now, to be more specific, we must learn selectors > `.xpath()`
  * here is the some documents about `.xpath()`
    * [Brief Intro to XPath](https://docs.scrapy.org/en/latest/intro/tutorial.html?highlight=xpath#xpath-a-brief-intro)
    * [Selectors - XPath and CSS](https://docs.scrapy.org/en/latest/topics/selectors.html#topics-selectors)

## XPath() Examples

### Get Type Of Book

Inspect the target
![inspect_target](/images/scrapy-part5_1.png "inspect_target")
Get info about class and tags
![get_info](/images/scrapy-part5_2.png "get_info")

in our case is `.xpath()` should look like this
```
response.xpath("//ul[@class='breadcrumb']/li[@class='active']/preceding-sibling::li[1]/a/text()").get()
# output : 'Poetry'
```
### Get Book Description (Tagless)
In this case we wanted information about book information. Holded 'p' type but this time 'p' doesnt belong any tag or class
![inspect_target](/images/scrapy-part5_3.png "inspect_target")
We use sibling method and get the info
![get_info](/images/scrapy-part5_4.png "get_info")
in this case xpath code looks like this
```
response.xpath("//div[@id='product_description']/following-sibling::p/text()").get()
# output : "It's hard to imagine a world without A Light in the Attic. This now-classic collection of poetry and drawings from Shel Silverstein celebrates its 20th anniversary with this special edition. Silverstein's humorous and creative verse can amuse the dowdiest of readers. Lemon-faced adults and fidgety kids sit still and read these rhythmic words and laugh and smile and love th It's hard to imagine a world without A Light in the Attic. This now-classic collection of poetry and drawings from Shel Silverstein celebrates its 20th anniversary with this special edition. Silverstein's humorous and creative verse can amuse the dowdiest of readers. Lemon-faced adults and fidgety kids sit still and read these rhythmic words and laugh and smile and love that Silverstein. Need proof of his genius? RockabyeRockabye baby, in the treetopDon't you know a treetopIs no safe place to rock?And who put you up there,And your cradle, too?Baby, I think someone down here'sGot it in for you. Shel, you never sounded so good. ...more"
```

## Get Book Title
Inspect title
![inspect_title](/images/scrapy-part5_9.png "inspect_title")
Will look like
![title_info](/images/scrapy-part5_10.png "title_info")
get title
```
response.css('.product_main h1::text').get()
# output : 'A Light in the Attic'
```
## Get Price
Inspect price
![inspect_price](/images/scrapy-part5_11.png "inspect_price")
Will look like
![price_info](/images/scrapy-part5_12.png "price_info")
get price
```
response.css('p.price_color ::text').get()
# output : '£51.77'
```
## Get Stars Info
Inspect stars
![inspect_stars](/images/scrapy-part5_7.png "inspect_stars")
Will look like
![stars_info](/images/scrapy-part5_8.png "stars_info")
get class name of stars
```
response.css("p.star-rating").attrib['class']
# output : 'star-rating Three'
```
## Get Table Contents
Inspect the table
![inspect_table](/images/scrapy-part5_5.png "inspect_table")
Should look like
![table_tags](/images/scrapy-part5_6.png "table_tags")
Assigne table rows a variable. 
```
table_rows = response.css("table tr")
# table_rows is list

len(table_rows)
# output : 7
```
get info rows
```
table_rows[0].css('td::text').get()
# output : a897fe39b1053632

table_rows[1].css('td::text').get()
# output : 'Books'

table_rows[2].css('td::text').get()
# output : £51.77

```
## Create Spider using examples
whole code look like this
 ```
 import scrapy

class BookspiderSpider(scrapy.Spider):
    name = "bookspider"
    allowed_domains = ["books.toscrape.com"]
    start_urls = ["https://books.toscrape.com"]

    def parse(self, response):
        books =response.css('article.product_pod')
        for book in books:
            relative_url = book.css('h3 a ::attr(href)').get()

            if 'catalogue/' in relative_url:
                book_url = 'https://books.toscrape.com/' + relative_url
            else:
                book_url = 'https://books.toscrape.com/catalogue/' + relative_url
            yield response.follow(book_url, callback= self.parse_book_page)

        next_page = response.css('li.next a ::attr(href)').get()
 
        if next_page is not None:
            if 'catalogue/' in next_page:
                next_page_url = 'https://books.toscrape.com/' + next_page
            else:
                next_page_url = 'https://books.toscrape.com/catalogue/' + next_page
            yield response.follow(next_page_url, callback= self.parse)

    def parse_book_page(self, response):
        table_rows = response.css("table tr")
        yield{
            'url' : response.url,
            'title' : response.css('.product_main h1::text').get(),
            'product_type' : table_rows[1].css('td::text').get(),
            'price_excl_tax' : table_rows[2].css('td::text').get(),
            'price_incl_tax' : table_rows[3].css('td::text').get(),
            'tax' : table_rows[4].css('td::text').get(),
            'availability' : table_rows[5].css('td::text').get(),
            'num_reviews' : table_rows[6].css('td::text').get(),
            'stars' : response.css("p.star-rating").attrib['class'],
            'category' : response.xpath("//ul[@class='breadcrumb']/li[@class='active']/preceding-sibling::li[1]/a/text()").get(),
            'description' : response.xpath("//div[@id='product_description']/following-sibling::p/text()").get(),
            'price' : response.css('p.price_color ::text').get()
        }
 ```
 start spider in powershell and save .csv file format informations

`scrapy crawl bookspider -O bookdata.csv`

 ```
 # example output
 url,title,product_type,price_excl_tax,price_incl_tax,tax,availability,num_reviews,stars,category,description,price
https://books.toscrape.com/catalogue/a-light-in-the-attic_1000/index.html,A Light in the Attic,Books,£51.77,£51.77,£0.00,In stock (22 available),0,star-rating Three,Poetry,"It's hard to imagine a world without A Light in the Attic. This now-classic collection of poetry and drawings from Shel Silverstein celebrates its 20th anniversary with this special edition. Silverstein's humorous and creative verse can amuse the dowdiest of readers. Lemon-faced adults and fidgety kids sit still and read these rhythmic words and laugh and smile and love th It's hard to imagine a world without A Light in the Attic. This now-classic collection of poetry and drawings from Shel Silverstein celebrates its 20th anniversary with this special edition. Silverstein's humorous and creative verse can amuse the dowdiest of readers. Lemon-faced adults and fidgety kids sit still and read these rhythmic words and laugh and smile and love that Silverstein. Need proof of his genius? RockabyeRockabye baby, in the treetopDon't you know a treetopIs no safe place to rock?And who put you up there,And your cradle, too?Baby, I think someone down here'sGot it in for you. Shel, you never sounded so good. ...more",£51.77
 
'item_scraped_count': 1000,
 ```
 start spider in powershell and save .json file format informations

`scrapy crawl bookspider -O bookdata.json`
```
# example output
{"url": "https://books.toscrape.com/catalogue/a-light-in-the-attic_1000/index.html", "title": "A Light in the Attic", "product_type": "Books", "price_excl_tax": "£51.77", "price_incl_tax": "£51.77", "tax": "£0.00", "availability": "In stock (22 available)", "num_reviews": "0", "stars": "star-rating Three", "category": "Poetry", "description": "It's hard to imagine a world without A Light in the Attic. This now-classic collection of poetry and drawings from Shel Silverstein celebrates its 20th anniversary with this special edition. Silverstein's humorous and creative verse can amuse the dowdiest of readers. Lemon-faced adults and fidgety kids sit still and read these rhythmic words and laugh and smile and love th It's hard to imagine a world without A Light in the Attic. This now-classic collection of poetry and drawings from Shel Silverstein celebrates its 20th anniversary with this special edition. Silverstein's humorous and creative verse can amuse the dowdiest of readers. Lemon-faced adults and fidgety kids sit still and read these rhythmic words and laugh and smile and love that Silverstein. Need proof of his genius? RockabyeRockabye baby, in the treetopDon't you know a treetopIs no safe place to rock?And who put you up there,And your cradle, too?Baby, I think someone down here'sGot it in for you. Shel, you never sounded so good. ...more", "price": "£51.77"},

'item_scraped_count': 1000,
```

# Part 6 : Cleaning Data With Item Pipelines

* Scrapy Items ! What is it ?
* Structure Data using with scrapy items
* So Scrapy Pipelines?
* Cleaning Data with Item Pipelines