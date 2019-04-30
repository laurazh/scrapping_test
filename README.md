# scrapping_test

How to scrap website that do not provide API 

- [x] example with pandas
- [ ] beautifulsoup
- [ ] scrappy
- [ ] selenium/phantom JS
- [ ] pupeeteer/chrome headless
- [ ] pupeeteer/chrome headless with different browser
- [ ] pupeeteer/chrome headless with IP rotation (in different region)


--------------------------------

# With pandas

For data that are rendered as table, Pandas has provided an easy way to get them.
~~~~
import pandas as pd 
URL = 'https://en.wikipedia.org/wiki/List_of_countries_and_dependencies_by_population' 
df = pd.read_html(URL) 
df[0].head()
~~~~

![alt text](https://github.com/laurazh/scrapping_test/blob/master/image/Screenshot%202019-04-29%20at%2014.20.31.png)
 
# With Beautifulsoup
https://www.dataquest.io/blog/web-scraping-tutorial-python/

There is a lot of tutorial on how to do it, but basically Beautifulsoup will give a from html file a text file and the user will have to search in this text file to look for the needed information (thanks to the tag).
With the same example as previously, to create the same dataframe, you will have to work a little bit more.

~~~~
from bs4 import BeautifulSoup
import requests
import requests
page = requests.get(URL) 

page.content # will give the html content to a text content
soup = BeautifulSoup(page.content ) # you create the soup
print(soup.prettify()) # to have a better rendering
content_ = soup.find_all('table', class_="wikitable sortable")[0] #the table data is inside

ori_counstry =1
ori_pop = 2
ori_date = 3
ori_perc = 4
ori_source= 5
ori_rank= 0 

max_country = len(content_.find_all('span')) 
df_ = pd.DataFrame() 


for i in range(0,max_country*6,6): 
    rank = content_.find_all('td')[ori_rank+i].get_text().strip('\r\n')
    country = content_.find_all('td')[ori_counstry+i].get_text().strip('\r\n')
    source = content_.find_all('td')[ori_source+i].find_all('a')[0].get_text().strip('\r\n')
    pop = content_.find_all('td')[ori_pop+i].get_text().strip('\r\n')
    date = content_.find_all('td')[ori_date+i].get_text().strip('\r\n')
    perc = content_.find_all('td')[ori_perc+i].get_text() .strip('\r\n') 
    df_ = df_.append(pd.DataFrame([rank,country, pop, date, perc, source]).T) 
    
df_.columns =['rank','country', 'population','date','perc','source']
~~~~

# With scrappy

# With chromeheadless / pupeteer

---------------

Q : WEBSITE WITH JAVASCRIPT RENDERING ==> ??
Q : how to pass recaptcha ?
=> voice recognition
 => Kaldi 
 => mozilla => do not recognize if noise data, yet => no pretrain model
=> image recognition 
Q : how to pass honey pot ?
Q : what to use for IP rotation ? free VPN?
Q : what the use to make screen cap of website ? => in A/B Testing?
