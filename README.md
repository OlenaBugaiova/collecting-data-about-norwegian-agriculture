# Collecting data about Norwegian agriculture

All data is derived from the following websites:

- [Nibio.no](https://www.nibio.no/)
- [Plantevernleksikonet.no](https://www.plantevernleksikonet.no/)
- [nlr.no](https://www.nlr.no/fagartikler)

### Approaches:

Nibio

The website has a navigation menu on the home page and stores subject URLs hierarchically in a javascript block. To approach the hierarchical structure we used recursion. We downloaded the high-level topics and their descriptions and then retrieved URLs for all the lower-level web pages recursively. Then we downloaded lower-level web pages and parsed them with the BeautifulSoup tool

See [the code](https://github.com/OlenaBugaiova/collecting-data-about-norwegian-agriculture/blob/main/code/NIBIO_Web_Scraping_of_Agriculture_Text.ipynb)

---
Plantevernleksikonet

URLs were unreachable from the home web page. We found that webpages for each subject could be retrieved by URL of the following form:

https://www.plantevernleksikonet.no/l/oppslag/number/

where “number” is a number in a certain range. For example, the following URL is a URL for the Småkattost webpage

https://www.plantevernleksikonet.no/l/oppslag/1700/

We used an iterative approach: going through all numbers from 0 to 10000, we downloaded web pages and checked if they were not empty. If there were a certain number of empty web pages consecutively we break the iteration cycle.
There were 1690 not empty webpages. We parsed them by searching for specific html hashtags with the BeautifulSoup tool.
Some web pages had headers but no text content. In that case, they showed the following message:

'Denne organismen mangler oppslag i leksikonet. Beklager!'
like in this example: https://www.plantevernleksikonet.no/l/oppslag/679/, we ignored them.

The number of the webpages that remained after removing was 1090

See [the code](https://github.com/OlenaBugaiova/collecting-data-about-norwegian-agriculture/blob/main/code/Plantevernleksikonet_Web_Scraping_of_Agriculture_Text.ipynb)


---
Om Norsk Landbruksrådgiving (NLR)

The Fagartikler page had a filter where you could select a category or region and then web pages were loaded. Therefore URLs for articles were selected dynamically and unreachable from the static HTML file.
We found that the URL for each article has the following structure:

https://www.nlr.no/fagartikler/kategori/region/title

Per each category and region, we collected titles, by manually copying them from the website and storing them in a JSON file under the corresponding URL prefix. Then we constructed URLs.

Exceptions:
- Some URLs contained version 2:
https://www.nlr.no/fagartikler/korn/sor/organisk-gjodsel-til-korn-2
- Some URLs contained a region with a “default” value:
https://www.nlr.no/fagartikler/okologisk/default/naeringsforsyning-i-okologisk-akerbruk

We handled these cases:
1. We constructed two versions of each URL: with version 2 and without version 2. We
downloaded webpages from the URLs and checked if they were empty. Then we
saved non-empty web pages and URLs for the empty ones
2. For the empty webpage URLs, we replaced the region with the default value and
repeated the procedure.
3. Then we parsed webpages with Beautiful Soup.
Some web pages were empty because a title was modified or the article was located under one region but the URL references to another region. We didn’t handle those cases.

See [the code](https://github.com/OlenaBugaiova/collecting-data-about-norwegian-agriculture/blob/main/code/NLR_Web_Scraping_of_Agriculture_Text.ipynb)

---

### DOI
[![DOI](https://zenodo.org/badge/834213332.svg)](https://zenodo.org/doi/10.5281/zenodo.13371100)
                                
