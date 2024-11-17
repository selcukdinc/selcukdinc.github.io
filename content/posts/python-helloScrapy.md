+++
title = 'Python-Scrapy'
date = 2024-08-24T16:33:42+03:00
draft = false
ShowToc = true
ShowBreadCrumbs = true
+++

[Course Video - (Youtube link)](https://www.youtube.com/watch?v=mBoX_JCKZTE)

[Scrape Free Website](https://books.toscrape.com)

My OS is windows 11 home, course shots macos.Inside the code some points differences. 

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
  * We define what exactcly need to us 
* Structure Data using with scrapy items
* So Scrapy Pipelines?
* Cleaning Data with Item Pipelines

In this section just arrange `items.py` and `piplines.py`. Combine part 5 to this files


`Items.py` set what we uses items, if somewhere in spider file, program throw an error

```
import scrapy
import scrapy.item


class BookscraperItem(scrapy.Item):
    # define the fields for your item here like:
    name = scrapy.Field()
    pass


def serialize_price(value):
    return f'$ {str(value)}'



class BookItem(scrapy.Item):
    url = scrapy.Field()
    title = scrapy.Field()
    upc = scrapy.Field()
    product_type = scrapy.Field()
    price = scrapy.Field()
    price_excl_tax = scrapy.Field()
    price_incl_tax = scrapy.Field()
    tax = scrapy.Field()
    availability = scrapy.Field()
    num_reviews = scrapy.Field()
    stars = scrapy.Field()
    category = scrapy.Field()
    description = scrapy.Field()
``` 

`bookspider.py` before we created new class, now each class member set value using part-5 technics

```
import scrapy
from bookscraper.items import BookItem

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
       
        book_item = BookItem()
       
        book_item['url'] = response.url,
        book_item['title'] = response.css('.product_main h1::text').get(),
        book_item['upc'] = table_rows[0].css("td ::text").get(),
        book_item['product_type'] = table_rows[1].css('td::text').get(),
        book_item['price'] = response.css('p.price_color ::text').get(),
        book_item['price_excl_tax'] = table_rows[2].css('td::text').get(),
        book_item['price_incl_tax'] = table_rows[3].css('td::text').get(),
        book_item['tax'] = table_rows[4].css('td::text').get(),
        book_item['availability'] = table_rows[5].css('td::text').get(),
        book_item['num_reviews'] = table_rows[6].css('td::text').get(),
        book_item['stars'] = response.css("p.star-rating").attrib['class'],
        book_item['category'] = response.xpath("//ul[@class='breadcrumb']/li[@class='active']/preceding-sibling::li[1]/a/text()").get(),
        book_item['description'] = response.xpath("//div[@id='product_description']/following-sibling::p/text()").get(),
        
       
        yield book_item
```
`pipelines.py` Values getting ok but needed to rework. Cleaning proccess is here. 
```
from itemadapter import ItemAdapter


class BookscraperPipeline:
    def process_item(self, item, spider):

        adapter = ItemAdapter(item)

        # Strip all whitespaces from strings
        field_names = adapter.field_names()
        for field_name in field_names:
            if field_name != 'description':
                value = adapter.get(field_name)
                adapter[field_name] = value[0].strip()

        # Category & Product Type --> switch to lowercase
        lowercase_keys = ['category', 'product_type']
        for lowercase_key in lowercase_keys:
            value = adapter.get(lowercase_key)
            adapter[lowercase_key] = value.lower()

        # Price --> convert to float
        price_keys = ['price', 'price_excl_tax', 'price_incl_tax', 'tax']
        for price_key in price_keys:
            value = adapter.get(price_key)
            value = value.replace('£', '')
            if value == '':
                value = 0
            adapter[price_key] = float(value)

        # Availability --> extract number of books in stock
        availability_string = adapter.get('availability')
        split_string_array = availability_string.split('(')
        if len(split_string_array) < 2:
            adapter['availability'] = 0
        else:
            availability_array = split_string_array[1].split(' ')
            adapter['availability'] = int(availability_array[0])


        # Reviews --> convert string to number
        num_reviews_string = adapter.get('num_reviews')
        adapter['num_reviews'] = int(num_reviews_string)


        # Stars --> convert text no number
        stars_string = adapter.get('stars')
        split_stars_array = stars_string.split(' ')
        stars_text_value = split_stars_array[1].lower()
        if stars_text_value == "zero":
            adapter['stars'] = 0
        elif stars_text_value == "one":
            adapter['stars'] = 1
        elif stars_text_value == "two":
            adapter['stars'] = 2
        elif stars_text_value == "three":
            adapter['stars'] = 3
        elif stars_text_value == "four":
            adapter['stars'] = 4
        elif stars_text_value == "five":
            adapter['stars'] = 5

        return item
```
Most important thing is `settings.py` inside activate "pipeline" lines
```
ITEM_PIPELINES = {
   "bookscraper.pipelines.BookscraperPipeline": 300,
}
```
and now run spider and save the data
```
# file saves from terminal location
scrapy crawl bookspider -O cleandata.json

# Output (changed to more readable): 
{
  "url": "https://books.toscrape.com/catalogue/set-me-free_988/index.html", 
  "title": "Set Me Free", 
  "upc": "ce6396b0f23f6ecc", 
  "product_type": "books", 
  "price": 17.46, 
  "price_excl_tax": 17.46, 
  "price_incl_tax": 17.46, 
  "tax": 0.0, 
  "availability": 19, 
  "num_reviews": 0, 
  "stars": 5, 
  "category": "young adult", 
  "description": ["Aaron Ledbetter’s future had been planned out for him since before he 
  was born. Each year, the Ledbetter family vacation on Tybee Island gave Aaron a chance to 
  briefly free himself from his family’s expectations. When he meets Jonas “Lucky” Luckett, 
  a caricature artist in town with the traveling carnival, he must choose between the life
   that’s been mapped out for him, and Aaron Ledbetter’s future had been planned out for 
   him since before he was born. Each year, the Ledbetter family vacation on Tybee Island
    gave Aaron a chance to briefly free himself from his family’s expectations. When he 
    meets Jonas “Lucky” Luckett, a caricature artist in town with the traveling carnival,
    he must choose between the life that’s been mapped out for him, and the chance at 
    true love. ...more"]
}
```

# Part 7 : Saving Data To Files & Databases
* Saving Data Via The Command Line
* Saving Data Via Feed Settings
* Saving Data Into Databases

## Saving Data Via The Command Line
`scrapy crawl bookspider -O bookdata.csv`

Diffirence between parameter `-O` and `-o`
* `-O` : Overwrite data
* `-o` : keep write data


## Saving Data Via Feed Settings

### Method 1 : Write Inside The `settings.py`
```
FEEDS = {
    'booksdata.json': {'format': 'json'},
    'booksdata.csv': {'format':'csv'}
}
```

### Method 2 : Write Directly `bookspider.py`
```
custom_settings = {
        'FEEDS': {
            'booksdata.json': {'format': 'json', 'overwrite': True}    
        }
    }
```

## Saving Data Into Database

We using MySQL. [Direct Download Page](https://dev.mysql.com/downloads/mysql/)

Dont forget, add path 

system variables > Paths > MySQL bin `C:\Program Files\MySQL\MySQL Server 9.0\bin` 

MySQL Database | Code Blocks
--|--
test mysql | `mysql --version`
access mysql root with password | `mysql -uroot -p`
exit mysql | `exit;`
install mysql connector in python | `pip install mysql mysql-connector-python`
list databases | `show databases;`
create database | `create database books;`
change database | `use books;`
look tables | `show tables;`
get all data in database | `select * from books;`
remove table | `drop table books;`

final version of `pipelines.py` 
```
from itemadapter import ItemAdapter


class BookscraperPipeline:
    def process_item(self, item, spider):

        adapter = ItemAdapter(item)

        # Strip all whitespaces from strings
        field_names = adapter.field_names()
        for field_name in field_names:
            if field_name != 'description':
                value = adapter.get(field_name)
                adapter[field_name] = value[0].strip()

        # Category & Product Type --> switch to lowercase
        lowercase_keys = ['category', 'product_type']
        for lowercase_key in lowercase_keys:
            value = adapter.get(lowercase_key)
            adapter[lowercase_key] = value.lower()

        # Price --> convert to float
        price_keys = ['price', 'price_excl_tax', 'price_incl_tax', 'tax']
        for price_key in price_keys:
            value = adapter.get(price_key)
            value = value.replace('£', '')
            if value == '':
                value = 0
            adapter[price_key] = float(value)

        # Availability --> extract number of books in stock
        availability_string = adapter.get('availability')
        split_string_array = availability_string.split('(')
        if len(split_string_array) < 2:
            adapter['availability'] = 0
        else:
            availability_array = split_string_array[1].split(' ')
            adapter['availability'] = int(availability_array[0])


        # Reviews --> convert string to number
        num_reviews_string = adapter.get('num_reviews')
        adapter['num_reviews'] = int(num_reviews_string)


        # Stars --> convert text no number
        stars_string = adapter.get('stars')
        split_stars_array = stars_string.split(' ')
        stars_text_value = split_stars_array[1].lower()
        if stars_text_value == "zero":
            adapter['stars'] = 0
        elif stars_text_value == "one":
            adapter['stars'] = 1
        elif stars_text_value == "two":
            adapter['stars'] = 2
        elif stars_text_value == "three":
            adapter['stars'] = 3
        elif stars_text_value == "four":
            adapter['stars'] = 4
        elif stars_text_value == "five":
            adapter['stars'] = 5

        return item
    

import mysql.connector

class SaveToMySQLPipeline:

    def __init__(self):
        file = open("C:\secrets\MySQL_Secret.txt", 'r')
        password = file.read()
        self.conn = mysql.connector.connect(
            host = 'localhost',
            user = 'root',
            password = password, # add your password here if you have one set
            database = 'books'
        )

        # Create cursor, used to execute commands
        self.cur = self.conn.cursor()

        # Create books table if none exists
        self.cur.execute("""
            CREATE TABLE IF NOT EXISTS books(
                        id int NOT NULL auto_increment,
                        url VARCHAR(255),
                        title text,
                        upc VARCHAR(255),
                        product_type VARCHAR(255),
                        price_excl_tax DECIMAL,
                        price_incl_tax DECIMAL,
                        tax DECIMAL,
                        price DECIMAL,
                        availability INTEGER,
                        num_reviews INTEGER,
                        stars INTEGER,
                        category VARCHAR(255),
                        description text,
                        PRIMARY KEY (id)
                    )
                    """)
    def process_item(self, item, spider):

        # Define insert statement
        self.cur.execute(""" insert into books (
                url,
                title,
                upc,
                product_type,
                price_excl_tax,
                price_incl_tax,
                tax,
                price,
                availability,
                num_reviews,
                stars,
                category,
                description
                ) values (
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s,
                %s )""", (
            item["url"],
            item["title"],
            item["upc"],
            item["product_type"],
            item["price_excl_tax"],
            item["price_incl_tax"],
            item["tax"],
            item["price"],
            item["availability"],
            item["num_reviews"],
            item["stars"],
            item["category"],
            str(item["description"][0])
        ))
        # Execute insert of data into database
        self.conn.commit()
        return item
        
    def close_spider(self, spider):
        # Close sursor & conneciton to database
        self.cur.close()
        self.conn.close()

```
`settings.py` add second line and give a bigger number. Reasons is work order, smaller pipeline run firstly.
```
# settings.py
ITEM_PIPELINES = {
    "bookscraper.pipelines.BookscraperPipeline": 300,
    "bookscraper.pipelines.SaveToMySQLPipeline": 400,
}
```
# Part 8 :Fake User-Agents & Browser Headers
## Why We Get Blocked When Web Scraping
Some web services want to public datas only reach real person And block unpersonally activity. Here is the key: How many people see few seconds in 1000 book ? Answer is "NoNe people" 
## Explaining & Using User Agents To ByPass Getting Blocked
But what if we keep want to data? Well... in this case we need know to how work this things.

- my computer (none of changes) : Hi! Can i look book first of the list...
- server : oh its real person, ok bro take it look!
- mc (0.0002 sec. after): Its amazing! can i look second of book?
- s : "This is Doesnt Coverage Real Person Time" Sorry you dont look a person, and i dont want bots or spiders. Get out, here is my site!
- mc : "404" or "i am human test" 

Lets change scenario

We need to this time "different computer spec list" and this is called in headers inside client data and we change to what we say it is... For example in our hands 3 different client computer data, like win11, macos and linux pc specs.
- mc (win11) : first book please
- s : ok, code : 200
- mc (linux, 0.0002 sec. after) : second book please
- s : "hmm its different client request" ok, code : 200
- mc (macos, 0.0002 sec. after) : third book please
- s : "hmm its different client request" ok, code : 200

it is simplified of changed client header in scrapy 

And yeah it doesnt easy today websites...

[lets check your agency](https://useragentstring.com/index.php)
in my case website says
* `Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:129.0) Gecko/20100101 Firefox/129.0`
and other machine examples
* `Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.79 Safari/537.36`
* `Mozilla/5.0 (X11; Ubuntu; Linux i686 on x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/53.0.2820.59 Safari/537.36`

Looks like id but it doesnt. Just says this is computer or phone or tablet. This device os is 'X' and borwser version is this.

lets change few changes in `bookspider.py`
```
# new list! 
  user_agent_list = [
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/93.0.4577.82 Safari/537.36',
        'Mozilla/5.0 (iPhone; CPU iPhone OS 14_4_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0.3 Mobile/15E148 Safari/604.1',
        'Mozilla/4.0 (compatible; MSIE 9.0; Windows NT 6.1)',
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 Edg/87.0.664.75',
        'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.18363',
    ]
# clipped code 
def parse(self, response):
       books =response.css('article.product_pod')
       for book in books:
           relative_url = book.css('h3 a ::attr(href)').get()

           if 'catalogue/' in relative_url:
               book_url = 'https://books.toscrape.com/' + relative_url
           else:
               book_url = 'https://books.toscrape.com/catalogue/' + relative_url
           yield response.follow(book_url, callback= self.parse_book_page, headers={"User-Agent":self.user_agent_list[random.randint(0, len(self.user_agent_list)-1)]})
# clipped code  
if next_page is not None:
           if 'catalogue/' in next_page:
               next_page_url = 'https://books.toscrape.com/' + next_page
           else:
               next_page_url = 'https://books.toscrape.com/catalogue/' + next_page
           yield response.follow(next_page_url, callback= self.parse, headers={"User-Agent":self.user_agent_list[random.randint(0, len(self.user_agent_list)-1)]})
```
We change any user agent whant we want! In this code we pick random user agent

`headers={"User-Agent":self.user_agent_list[random.randint(0, len(self.user_agent_list)-1)]}`
## Explaining & Using Request Headers To ByPass Getting Blocked 
Most complicated websites not only look `user-Agent` looks all of request headers.

Here is the all of them,
![table_tags](/images/scrapy-part8_1.png "table_tags")
some point 

server says : oh its only 5 machine much more faster than any human. Lets banned our website

This stuations we can create fake user agents and not only use 5 header.

Lets implement `middlewares.py`

but first we need tu fake user agent api for all these. Course redirected [this site](https://scrapeops.io/), create or login account.