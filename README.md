# LogoScrapper
Class for scrapping logos from sites.

Do not use private methods outside.
It makes error.

### Example of using this class:

    logo = LogoScrapper(URL, render=True)
    print(logo.logo_result)

    > {
    >   website: https://someurl.com,
    >   type: ImageLink,
    >   image: https://someurl.com/src/images/logo.png
    > }

First you need to create an object by passing a link to the page with 
the logo into it and setting some arguments which described below.
After creating the object, simply take 
the information from the logo_result field.
There will be a dict with 3 fields:
    
* website - just a link to page which you setted in a scrapper

* type - one from five posible types, described below

* image - it can be a link to logo, html tag or just NoneType

## Args:

* url (str): link to target site
proxies (dict): your proxies, described in more detail below
    (default is None)

* render (bool): whether the script needs to render the cs code on the 
    page, it can improve the scripts work but speed of script can be less, 
    he recommended value is True.
    (default is False)

* favicon (bool): whether to take a favicon from the site, often the favicon
    matches the logo, can increase search success
    (default is False)

* meta_image (bool): whether to search for a logo from a meta tag with 
    an attribute property with value og:image, 
    sometimes there may be a logo here, 
    but often it can pull just a image or background
    (default is False)

## Returns:

* None


## Proxies:
### Example of using proxies with scrapper:

        proxies = {
            "http": "<proxy>",
            "https": "<proxy>"
        }
        logo = LogoScrapper(URL, proxies=proxies)
    
## Types in logo_result:
* ImageLink - link to image

* SvgTag - svg tag from html

* ConnectionError - some error while connection, probably site not exist or something else

* TimeoutError - the connection request is longer than the time limit argument

* ConnectionError:<status_code> - error while connection with status code

# Methods of LogoScrapper:

## get_session()

Method for making session object.

### Args:

* None

### Returns:
* cfscrape.CloudflareScrapper: special object for making requests more like a usual user

## __get_site_response()
Method for getting data from site.

### Args:
* session (cfscrape.CloudflareScrapper): Session for making request

* url (str): link to target site
timeout (int): time limit per request
    (default is 3)

* verify (bool): making request with or without verifying
    (default is True)

### Returns:
* requests_html.HTMLResponse: response with data from 

* site: text, status, etc.

## __set_response()
Makes session with get_session() method and
takes response. Also sets up response and status fields.

### Args:
* None

### Returns:
* None

## __set_soup()
Sets up soup field in class.

### Args:
* text (str): html code from site

### Returns:
* None

## __get_css_file()
Gets link to css file from site and makes request
to this link. Takes data and returns it.

### Args:
* None

### Returns:
* str or None: data in css file or nothing if some problems with connection

## get_logo_url_from_css()
Method is parsing css file and looking for
some styles with logo or brand in name.
Takes url to logo if exists in file.

### Args:
* css_file (str): data from css file

* url (str): full link to target site from where css file

### Returns:
* str or None: link to logo or nothing

## request_with_selenium()
Method for taking data from problem sites with Selenium.

### Args:
* url (str): link to site
* path (str): path to ChromeDriver
    (default is None)

### Returns:
* str: html code of site

## __if_elem()
Method for following DRY in __scripts() method.

### Args:
* elem (bs4.element.Tag or NoneType): the element in which to search if it exists

* needed (str): the element script is looking for

### Returns:
* bs4.element.Tag or NoneType: result of searching

## __if_get()
Method for following DRY in __scripts() method.

### Args:
* elem (bs4.element.Tag or NoneType): the element in which to search
    
* attr (str): the attribute that is checked by the script

### Returns:
* bs4.element.Tag or NoneType: result of searching

## __if_get_list()
Method for following DRY in __scripts() method.

### Args:
* elem (bs4.element.Tag or NoneType): the element in which to search
    
* attr (str): the attribute that is checked by the script

### Returns:
* bs4.element.Tag or NoneType: result of searching

## __scripts()
Generator which checks possible positions logo in html code.

### Args:
* soup (bs4.BeautifulSoup): BeatifulSoup object for looking for elements

### Returns:
* bs4.element.Tag or NoneType: html tag with possible logo or nothing

## __set_logo_url()
Determinants type of logo and takes data about logo depends on type.
Sets result in logo field.

### Args:
* script (bs4.element.Tag): html tag with logo

### Returns:
* None

## __run()
Method for running generator-method __scripts().

### Args:
* None

### Returns:
* None

## get_re_page_url()
Search needed part of url with regex.

### Args:
* url (str): link to check

### Returns:
* str: validated url

## get_re_img_url()
Looking for type of images in link.

### Args:
* url (str): link to check

### Returns:
* bool: the result is whether the desired image type

## get_validated_logo_link()
Checks link to logo and if link is not full makes full link.

### Args:
* logo (str): link to logo

### Returns:
* str: validated full link to logo

## __set_results()
Method sets type and logo to logo_results field.

### Args:
* type_ (str): type of result
    * ImageLink
    * SvgTag
    * ConnectionError
    * TimeOutError
    * ConnectionError:<status_code>

### Results:
* None

## __set_logo_results()
General method for validation and setting results.

### Args:
* None

### Returns:
* None

## create_rotating_log()
Method for getting logger object with file handler.

### Args:
* folder (str): path to folder
* filename (str): name of file in format <name>.log

### Returns:
* logging.rootLogger: logger object for using

# BlockAll
A special class for disabling pop-up windows notifying you of cookie permissions.
It is using in LogoScrapper class.
