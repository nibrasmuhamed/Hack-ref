# Google Dork

Google is arguably the most famous example of “Search Engines”
"Search Engines" such as Google are huge indexers – specifically, indexers of content spread across the World Wide Web.

These essentials in surfing the internet use “Crawlers” or “Spiders” to search for this content across the World Wide Web, which I will discuss in the next task.


## crawler

These crawlers discover content through various means. One being by pure discovery, where a URL is visited by the crawler and information regarding the content type of the website is returned to the search engine. In fact, there are lots of information modern crawlers scrape – but we will discuss how this is used later. Another method crawlers use to discover content is by following any and all URLs found from previously crawled websites. Much like a virus in the sense that it will want to traverse/spread to everything it can.

[for more](https://tryhackme.com/room/googledorking)

## search engine optimisation - SEO

Search Engine Optimisation or SEO is a prevalent and lucrative topic in modern-day search engines. In fact, so much so, that entire businesses capitalise on improving a domains SEO “ranking”. At an abstract view, search engines will “prioritise” those domains that are easier to index. There are many factors in how “optimal” a domain is - resulting in something similar to a point-scoring system.
Aside from the search engines who provide these "Crawlers", website/web-server owners themselves ultimately stipulate what content "Crawlers" can scrape. Search engines will want to retrieve **everything** from a website - but there are a few cases where we wouldn't want **all** of the contents of our website to be indexed! Can you think of any...? How about a secret administrator login page? We don't want **everyone** to be able to find that directory - especially through a google search.

Introducing Robots.txt...


## Sitemaps

Comparable to geographical maps in real life, “Sitemaps” are just that - but for websites!

  
“Sitemaps” are indicative resources that are helpful for crawlers, as they specify the necessary routes to find content on the domain. The below illustration is a good example of the structure of a website, and how it may look on a "Sitemap":

![](https://i.imgur.com/L5WqJU4.png)

  

The blue rectangles represent the **route** to nested-content, similar to a directory I.e. “Products” for a store. Whereas, the green rounded-rectangles represent an actual page. However, this is for illustration purposes only - “Sitemaps” don't look like this in the real world. They look something much more similar to this:

![](https://i.imgur.com/12Yxcn5.png)

“Sitemaps” are XML formatted. I won't explain the structure of this file-formatting as the room [XXE](https://tryhackme.com/room/xxe) created by [falconfeast](https://tryhackme.com/p/falconfeast) does a mighty fine job of this.

The presence of "Sitemaps" holds a fair amount of weight in influencing the "optimisation" and favorability of a website. As we discussed in the "Search Engine Optimisation" task, these maps make the traversal of content much easier for the crawler!


## Using Google for Advanced Searching
### 
 Whilst Google will have many Cat pictures indexed ready to serve to Joe, this is a rather trivial use of the search engine in comparison to what it can be used for.  
For example, we can add operators such as that from programming languages to either increase or decrease our search results - or perform actions such as arithmetic!

![](https://i.imgur.com/hrfWM6i.png)  
  
Say if we wanted to narrow down our search query, we can use quotation marks. Google will interpret everything in between these quotation marks as exact and only return the results of the exact phrase provided...Rather useful to filter through the rubbish that we don't need as we have done so below:

![](https://i.imgur.com/pJSW4ou.png)

### Refining our Queries

  

We can use terms such as “**site**” (such as bbc.co.uk) and a query (such as "gchq news") to search the specified site for the keyword we have provided to filter out content that may be harder to find otherwise. For example, using the “site” and "query" of "bbc" and "gchq", we have modified the order of which Google returns the results.  
  
In the screenshot below, searching for “gchq news” returns approximately 1,060,000 results from Google. The website that we want is ranked behind GCHQ's actual website:

![](https://i.imgur.com/b4duG6S.png)  
  
But we don't want that...We wanted “**bbc****.co.****uk**” first, so let's refine our search using the “**site**” term. Notice how in the screenshot below, Google returns with much fewer results? Additionally, the page that we didn't want has disappeared, leaving the site that we did actually want! 

![](https://i.imgur.com/dG3e64O.png)  
  
Of course, in this case, GCHQ is quite a topic of discussion - so there'll be a load of results regardless.

