# 10-K-scraper
This script is to download 10-k filing textual data (.htm) through Sec Edgar API, and to scrape specific sections, then save them into .txt file.
You are welcomed to do modifications on this scripts.

# Platform & Dependency: 
* Python 3.6 and standard libraries
* BeautifulSoup
# Introduction:
* <code>TenKDownloader(CIK, start_time, end_time)</code> will return a <code>TenKDownloader</code> object. <code>CIK</code> can be one string or a list of string. Check https://www.sec.gov/Archives/edgar/cik-lookup-data.txt to see the CIKs for companies that you are looking for. Sometimes this argument can be symbol. <code>start_time</code> and <code>end_time</code> are in format <code>%Y%m%d</code>.
  ## Attributes:
  ### Method:
  * <code>download(path='./data')</code> will download coresponding 10-k filing in <code>./data/\<CIK\>/date.htm</code>. Implementation of this function is to use __BeautifulSoup__ to scrape the web page and retrieve the .htm file;
  ### Data
  * <code>all_url</code> is a Python dictionary (key: CIK; value: list of tuple (date, filing url)).
  
  
* <code>TenKScraper(section, next_section)</code> will return a <code>TenKScraper</code> object. <code>section</code> is something like 'item 1', and <code>next_section</code> is where you stop. For example, if you want to scrape section 'item 2', you can create <code>TenKScraper('item 2', 'item 3')</code>.
  ## Attributes:
  ### Method:
  * <code>scrape(htm_file, txt_file)</code> will scrape and write textual data into <code>txt_file</code>, and will also return the text as a string. Implementation of this function is based on the work of http://community.mis.temple.edu/zuyinzheng/pythonworkshop/, using regular expression to recognize bond tag. You can customize the pattern which is p1-p13 in my code. Note that output path must exist, but the txt file is not necessary to be existed.
  
 # Example
 ```
from TenK import TenKDownloader, TenKScraper

company_CIK = ['6281', '6769']
downloader = TenKDownloader(company_name, '20150101','20181101')
downloader.download()

scraper = TenKScraper('Item 1A', 'Item 1B')  # scrape text start from Item 1A, and stop by Item 1B
scraper2 = TenKScraper('Item 7', 'Item 8')
scraper.scrape('./data/6281/20171122.htm', './data/txt/test.txt') # make sure ./data/txt exists
scraper2.scrape('./data/6769/20180223.htm', './data/txt/test2.txt')
 ```

