## What is Content Discovery
In web application, content refers to resources that are not directly visible to a normal user. Content can be file, image, video, audio, website's older versions, configuration files, administrative files etc.
We are discussing three types of content discoveries in this write up.
1. Manual 
2. OSINT
3. Automated
# Manual Discovery
## Robots.txt
`Robot.txt` is a file that tells the search engines not to show specified pages in search results. These pages can contain admin-related information that a developer does not want users to see. It is just like restricting a certain area of website so they are not displayed on search engine results.
## Favicon
Favicon is a small icon on the browser's address bar or tab used for branding.
When a framework is used to build a website, favicon is the part of installation. sometimes developers forget to replace favicon with a custom one, which can give us clue about the framework stack. OWASP holds a database of common framework icons  which you can use to check against target favicon.
[OWASP Favicon Databse](https://wiki.owasp.org/index.php/OWASP_favicon_database)
## Sitemap.xml
Unlike Robot.txt file this file contains web pages that owner wishes to display in search engine results. This file can contains  a web page that is difficult to navigate or is old that the site no longer uses but still working in the background.
## HTTP Headers
When we make a request to a web server. It responds with HTTP headers. These headers can sometimes contain useful information such as webserver software and programming/scripting language the server is using.
## Framework Stack
Once you've established the framework of a website, either from the above favicon example or by looking for clues in the page source such as comments, copyright notices or credits, you can then locate the framework's website.
# OSINT 
## Google Dorking
There are also external tools that we can use to gather information about our target. Google provided a feature where we just type a command on search engine and it gives us information that is relevant to our command.

| Filter   | Examples                     | Description                                                  |
| -------- | ---------------------------- | ------------------------------------------------------------ |
| site     | site:google.com              | returns results only from the specified website address      |
| inurl    | inurl:admin                  | returns results that have the specified word in the URL      |
| intitle  | intitle:usa                  | returns results which are a particular file extension        |
| filetype | site:google.com filetype:pdf | returns results that contain the specified word in the title |
## Wappalyzer
Wappalyzer is an online tool and browser extension used to find what technology the website is using such as frameworks, Content Management Systems (CMS), payment processors and much more, and it can even find version numbers as well.
[Wappalyzer](https://www.wappalyzer.com/)
## Wayback Machine
Wayback machine is an online tool used to see previous snapshots of websites. It is like a time machine where we can put domain name and  see how does a website look back in the days.
## Github
Git tracks changes in files and helps people work together on projects. Users save their changes and share them in a repository.
GitHub is an online place that stores these repositories. Some are public, some are private, and searching GitHub can sometimes reveal useful project files or code.
## S3 Buckets
S3 Bucket is an online storage service provided by Amazon AWS .It is used to store files and even web contents over HTTP and HTTPS. Users can set permission either to make it private or public. The format of the S3 buckets is:
`http(s)://{name}.s3.amazonaws.com` where `{name}` is decided by the owner, such as `my-assets.s3.amazonaws.com`.
# Automated
## What is Automated Discovery
Automated discovery is the process of using tools to discover content rather than doing it manually. This process is automated as it usually contains hundreds, thousands or even millions of requests to a web server. These requests check whether a file or directory exists on a website, giving us access to resources we didn't previously know existed. This process is made possible by using a resource called wordlists.
## Wordlists
A wordlist is a document that can contain millions of password, directory names, and other entities. This document is used by different tools to search for the given resources.

