# Jots

**Exporting files**
- Zip directory: `tar -czvf archive.tar.gz bounty/`
- Unzip file: `tar -xzvf archive.tar.gz`

## Summary
- [General workflow](#general-workflow)
- [Speed up your work](#speed-up-your-work)

## General workflow
- [ ] Enumerate for subdomains using Subfinder & other tools
  - `subfinder -d redacted.com -all -silent -o subdomains.txt`
  - `ffuf -u 'https://redacted.com' -H "Host: FUZZ.redacted.com" -w wordlists.txt -o subdomains.txt`

- [ ] Check for sitemap.xml or robots.txt ***(Often reveals list of urls & paths)***
  - https://redacted.com/sitemap.xml
  - https://redacted.com/robots.txt

- [ ] Use Hakrawler to crawl all visible url in a page
  - `echo 'https://redacted.com' | hakrawler | tee crawler.txt`

- [ ] Use WaybackUrls to trace back older versions ***(Enlarge attack surface)***
  - `echo 'redacted.com' | waybackurls | tee wayback.txt`

- [ ] Use GF Patters to find url with specific functionality
  - `cat crawler.txt | gf xss | sed 's/=.*/=/' | sed 's/URL: //' | tee xss-urls.txt`
  - `cat wayback.txt | gf idor | sed 's/=.*/=/' | sed 's/URL: //' | tee idor-urls.txt`

- [ ] Run automation tool on the urls & domains retrieve
  - dalfox
  - nuclei
  - commix
  - sqlmap

- [ ] Exploit manually
  - understand the app's flow
  - intercept requests, data & header tampering, use other HTTP methods
  - try basic payloads and find out the security measures applied
  - using Hacktricks or PayloadsAllTheThings resources & many more
  - do a lot of research
  - read writeups

## Speed up your work
- Make a GET request to a list of domains to check for validity
  - `xargs -a subdomains.txt -n 1 -P 10 sh -c 'curl -s -I -o /dev/null -w "%{url_effective} %{http_code}\n" "$@" | grep -v "000"' sh | tee subdomains.txt'`
