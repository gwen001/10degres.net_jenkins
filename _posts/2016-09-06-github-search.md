---
title: GitHub search
tags:
- github
- password
- security
- bug bounty
- tools
see_also:
  -
    link: Uber database keys revealed
    url: http://arstechnica.com/security/2015/03/in-major-goof-uber-stored-sensitive-database-key-on-public-github-page/
  -
    link: Public Slack credentials
    url: http://arstechnica.co.uk/security/2016/04/hacking-slack-accounts-as-easy-as-searching-github/
  -
    link: GitHub dorks
    url: https://github.com/techgaun/github-dorks/blob/master/github-dorks.txt
---
We all know the famous quote "*Think out of the box*".
Technical knowledge is important but creativity is also.
In bug bounty, to get nice rewards, sometimes you don't need to be a crazy coder or great network engineer, you simply need to try what other didn't.

This year, Slack get in trouble because many developers leave their credentials in their public repository.
Last year Uber had to deal with a major security issue: database keys were stored in GitHub (this leads to a sweet bounty for the finder).

I found an interesting project, on GitHub itself, to parse the search engine results: [vcsmap from Melvinsh](https://github.com/melvinsh/vcsmap){:class="flashlink" target="_blank"}.
Unfortunately the scrapper seems to have trouble with search that required authentication.
Since I don't understand Ruby, I wrote my own tool with PHP.
<!--more-->

The code is available on my [GitHub repository](https://github.com/gwen001/github-search){:class="flashlink" target="_blank"} and for sure, you can tweak it to fit your needs.
Usage is:

~~~
Usage: php github-search.php [OPTIONS]

Options:
    -c  set cookie session
    -e	file extension filter
    -f  looking for file
    -h  print this help
    -o  provide organization name
    -r  maximum number of results, default 50
    -s  search string

Examples:
    php github-search.php -o myorganization -s db_password
    php github-search.php -o myorganization -f wp-config.php -s db_password
    php github-search.php -c "user_session=B0KqycP8LlYORc-s3WFZoH71TG" -f wp-config -e php -r 1000
~~~

An organization name must be provided when you are not authenticated on GitHub, that why the cookie option exists, of course you should use your own cookie value. And here is the output:

[![GitHub search PHP tool](/images/github-search-example.png)](/images/github-search-example.png)

The following fields are displayed:

- repository: name of the repository where the file/string has been found
- file: name of the file found or where the string has been found
- language: the estimated language used in the file
- summary: the lines where the string has been found with their number
- link: direct link to the concerned file

Give it a try and let me know if you find a bug!
