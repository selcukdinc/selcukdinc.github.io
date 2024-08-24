+++
title = 'Python-Scrapy'
date = 2024-08-24T16:33:42+03:00
draft = false
+++

[Course Video - Youtube link](https://www.youtube.com/watch?v=mBoX_JCKZTE)

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

### Step 6 : Install & Open Easier access with Shell

* install shell
  * `pip install ipython`
* Open shell
  * `scrapy shell`

# Part 4 (I) : First Scrapes with Shell 

  * Get data from "https://books.toscrape.com/"
    * `fetch fetch('https://books.toscrape.com/')`
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
#### Method 2 : Get iwth class names
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
run this program
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