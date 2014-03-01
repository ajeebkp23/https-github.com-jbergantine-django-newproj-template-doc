## Frontend Development

### Code Quality

* Validate every page with the [W3C markup validator](http://validator.w3.org/).
* Validate stylesheet with the [W3C CSS validator](http://jigsaw.w3.org/css-validator/).
* Verify that no page is throwing JavaScript errors.
* Review JavaScript with JSLint.
* Verify that JavaScript `console.log()` statements have been removed in production.

### Accessibility

* Verify that each image has either legitimate `alt` text  or a null `alt` text attribute.
* Verify that abbreviations and acronyms are marked up with the `abbr` element.
* Verify that the site meets [accessibility requirements](http://webaim.org/resources/quickref/) (Section 508, WCAG 2.0), use [CynthiaSays](http://www.contentquality.com/) as necessary.
* Review [color contrast](http://www.checkmycolours.com).

### Performance

* Verify that compressor middleware is properly working in production and that CSS and HTML files are minified and JavaScript files are minified and concatenated.
    * On Django this will likely be a combination of [django-slimmer](https://github.com/hbussell/django-slimmer) and [django-compressor](https://github.com/mintchaos/django_compressor).
* Evaluate the site with [YSlow](http://developer.yahoo.com/yslow/) (shoot for a score of 85+).
* Evaluate the page speed with [Google Page Speed](https://developers.google.com/speed/pagespeed/) (shoot for a score of 90+).

### Print

* If a print stylesheet is specified in the project description ensure placement in the template, review each page by print preview.

### Microdata

Establish whether any data would be well-suited for markup with:

* [Microformats](http://www.microformats.org)
* Microdata
    * [Schemas](http://schema.org/)
    * [Schema creater](http://schema-creator.org)
    * [Schema extractor](http://www.w3.org/2003/12/semantic-extractor.html)

### Cross-browser Evaluation 

_Review common breakpoints for scroll stoppers._

For the most efficient testing experience, [Browser Stack](http://www.browserstack.com) provides cross-browser testing as a service.

#### Windows Desktop

_Pay particular attention to JavaScript errors and CSS box model issues in IE._

_Verification in IE versions earlier than 8 necessary only if project specifications dictate such._

* IE8
* IE9
* IE10
* Firefox
* Chrome
* Opera

#### Apple Desktop

* Firefox
* Chrome
* Safari
* Opera

#### Ubuntu Desktop

_Verification necessary only if using native system font stacks or the project specifications dictate Linux review._

* Firefox
* Chrome or Chromium
* Opera


#### Mobile browsers

_It is acceptable to only verify in the device's native browser unless the project specifies otherwise._

* iPad
* iPad Mini
* iPhone 4 (iPod)
* iPhone 5 (iPod)
* Kindle Fire HD
* Samsung Galaxy Nexus (Android 4)
* Motorola Droid Razr (Android 4)

## Analytics

* Setup a new [Google Analytics](http://google.com/analytics) account for the project and ensure that the correct GATC tracking code is present on the site.
* Setup a new [Google Webmasters](http://www.google.com/webmasters/) profile for the project and verify the site.
* Review with Accounts team any custom Google Analytics actions they might want to track on the site:
    * Outbound links (can use: [this script](https://gist.github.com/4035433)).
    * Form abandonment (track clicks on submit vs traffic on end pages).
    * Clicks on various marketing areas (top nav, highlighted content, buttons).
* Review with Accounts team any custom funnels to setup in Google Analytics.
* Review with Accounts team any custom segmentation profiles to setup in Google Analytics:
    * Unfiltered raw data
    * All traffic except select IP addresses
    * Only organic search
    * Only organic search: branded keywords
    * Only organic search: non-branded keywords
    * Only direct traffic
    * Only paid search traffic
    * Only referrals
    * Only social referrals
    * Only email campaigns
    * Only new visitors
    * Only returning visitors
* Setup [Facebook Insights](http://facebook.com/insights/) for the site

## Metadata

### Favicon

Ensure favicon type icons are specified in `base.html`.

#### For theory, reference: 

* [_Understand the Favicon_ by Jonathan T. Neal](http://www.jonathantneal.com/blog/understand-the-favicon/)
* [_The State of Favicons_ by Chris Coyier](http://css-tricks.com/video-screencasts/122-the-state-of-favicons/)
* [_Everything you always wanted to know about touch icons._ by Mathias Bynens](http://mathiasbynens.be/notes/touch-icons)

#### For Apple:

* `apple-touch-icon-152x152-precomposed.png` should be a 152x152 PNG for the iPad with high-resolution Retina display running iOS ≥ 7
* `apple-touch-icon-120x120-precomposed.png` should be a 120x120 PNG for the iPhone with high-resolution Retina display running iOS ≥ 7
* `apple-touch-icon-76x76-precomposed.png` should be a 76x76 PNG for the iPad mini and the first- and second-generation iPad on iOS ≥ 7
* `apple-touch-icon-precomposed.png` should be a 57x57 PNG for non-Retina iPhone, iPod Touch, and Android 2.1+ devices

#### Favicons:

* `favicon.png` should be a 32x32 PNG on a solid color or transparent background
  (FYI, the article references 96x96 but this seems to be a typo)
* `favicon.ico` should be a combination of 16x16 and 32x32 PNGs compiled into a .ico file 
  (use http://www.kodlian.com/apps/icon-slate to compile that .ico file)

#### For Microsoft:

* `tileicon.png` should be 144x144 PNG on a transparent background
* a background tile color can be specified using a hex RGB color (e.g. `#RRGGBB`)

### Page Titles

_Page titles require SEO review and approval._

* 65 characters or less
* Ends in ` | site name`
* Spell checked

### Meta Description

_Meta descriptions require SEO review and approval._

* 150 characters or less
* Spell checked

### Keywords

_Meta keywords require SEO review and approval._

* 200 characters or less

### Facebook Graph Data

#### base.html

* Default image is specified (`og:image`). [Documentation](https://developers.facebook.com/docs/opengraph/creating-object-types/#properties).
* Default URL is specified (`og:url`). [Documentation](https://developers.facebook.com/docs/opengraph/creating-object-types/#properties).
* Default title is specified(`og:title`). [Documentation](https://developers.facebook.com/docs/opengraph/creating-object-types/#properties).
* Default type is specified (`og:type`). [Documentation](https://developers.facebook.com/docs/opengraph/creating-object-types/#properties).
* Default description is specified (`og:description`). [Documentation](https://developers.facebook.com/docs/opengraph/creating-object-types/#properties).

#### Every Page

* Verify that key sharable pages have been optimized
* Validate through the [Facebook Debugger](https://developers.facebook.com/tools/debug)
* Title is spell checked
* Description is spell checked
* URL is verified

### Canonical Pages

_Configuring this requires SEO review and approval._

* Establish whether `rel=canonical` needs to be specified for any pages (see [Google Webmasters](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=139394)).

### Nofollow Pages

_Configuring this requires SEO review and approval._

* Establish whether `rel=nofollow` needs to be specified for any pages or links (see [Google Webmasters](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=96569&topic=2371375&ctx=topic)).

### Google Plus

_Configuring this requires accounts team review and approval._

* Establish whether a Google+ profile needs to be associated with the site (see [Google Webmasters](http://support.google.com/webmasters/bin/answer.py?hl=en&answer=2539557&topic=2371375&ctx=topic)).

### Twitter Cards

* Establish whether a Twitter card would benefit the site (discuss with Accounts team).
    * [Documentation](https://dev.twitter.com/docs/cards)
    * [Validation](https://dev.twitter.com/docs/cards/validation/validator)

## Misc. Extra Pages

### Error Pages

* Verify the following templates exist and have been configured for the project:
    * 404 page
    * 410 page
    * 500 page
* If the site isn't using Django, verify server is properly configured to throw each page instead of Apache defaults.
    
    .htaccess configuration for a PHP site:
    
        ErrorDocument 404 /404.php
        ErrorDocument 410 /410.php

### XML Sitemap

_The XML Sitemap requires SEO review and approval._

* Establish if alternate sitemaps need to be included (mobile, image, video, etc.).
* Verify that sitemap(s) are served with the proper MIME/type.
* Verify that sitemap(s) are setup to be updated with dynamic content automatically.
* Verify that sitemap(s) include all static pages of the site.
* Verify that sitemap(s) prioritize content appropriately for the site (including `<priority>` where appropriate).
* Verify that sitemap(s) specify the `<lastmod>` and `<changefreq>` where appropriate.

### robots.txt File

_The robots.txt file requires SEO review and approval._

* is served with the proper MIME/type
* allows crawling
* points to sitemap

## Server Configuration

### Managers, Admins, Contact Form Recipients

* Review the `MANAGERS`, `ADMINS`, and if applicable `CONTACT_FORM_RECIPIENTS` constants in the production settings file; work with accounts team as necessary to verify who those should be.

### Server Redirects

_Server redirects require SEO review and approval._

* www. redirects to no-www domain with a 301 redirect

    Via .htaccess:


        # permanently redirect from www domain to non-www domain
        RewriteEngine on

        # the following may be required (on WebFaction for example) 
        # it may also cause problems depending on how the server is setup
        RewriteBase /
        
        Options +FollowSymLinks
        RewriteCond %{HTTP_HOST} ^www\.domain\.com$ [NC]
        RewriteRule ^(.*)$ http://domain.com/$1 [R=301,L]

    Via httpd.conf:
    
        LoadModule rewrite_module modules/mod_rewrite.so
        
        RewriteEngine On

        RewriteCond %{HTTP_HOST} ^domain.com$ [NC]
        RewriteRule ^(.*)$ http://domain.com$1 [R=301,L]

* Alternate domains and www. variations redirect to no-www domain with a 301 redirect.

### Database

* If the site is built with Django, review the number of connections using [django-debug-toolbar](https://github.com/robhudson/django-debug-toolbar) and optimize as necessary.
* Review SQL queries for potential SQL injection.
    * [SQL Injection and Django](http://www.djangobook.com/en/2.0/chapter20.html#sql-injection).
    * If the site is built with PHP, use prepared SQL statements or escape the data with `mysql_real_escape_string` if MySQLi- or PDO-type connections aren't available.
        * PDO is the preferred connection type as it is the most portable. It requires PHP 5+.
        * MySQLi is a perfectly acceptable alternative if PDO isn't available. It just isn't as portable.
* Review whether new database indexes are necessary if new columns or SQL queries were added since the last review.
* Load test the site: 
    * [Apache Bench](http://robots.thoughtbot.com/post/8392002973/7-minute-ab-impatient-mans-load-tests-for-a-heroku) will quickly test a single URL but won't download (static) resources.
    * [Siege](http://www.joedog.org/siege-home/) can be used for [more realistic tests](http://www.euperia.com/linux/tools-and-utilities/speed-testing-your-website-with-siege-part-one/720).
* Setup a CRON job (or other method) to backup database daily, verify that backup is working:
    * [MySQL on WebFaction](http://docs.webfaction.com/user-guide/databases.html#id1)
    * [PostgreSQL on WebFaction](http://docs.webfaction.com/user-guide/databases.html#id2)
* Send connection details to IT to copy backup files locally.

### Forms

* Explicitly reference or bulk match the submitted form fields against an array of expected form fields and only convert that to a variable for processing if there is a match.
* Consider trimming whitespace ([see debate](https://code.djangoproject.com/ticket/6362)) from all input variables with the `trim()` method (PHP) or `strip()` method (Django; this is best implemented by [creating a base form class](http://notesondjango.wordpress.com/2012/12/21/trim-spaces-in-form/)).
* Validate length of submitted values against maxlength of the SQL column it gets written to.
* To the greatest possible extent validate all input fields for the type and length of content expected, and when necessary sanitize it as well.
* Accessing data by using `$_POST` and not `$_REQUEST` (PHP) or `HttpRequest.POST` ([documentation](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.HttpRequest.POST)) (Django).
* Review site for [potential XSS vulnerabilities](http://www.djangobook.com/en/2.0/chapter20.html#cross-site-scripting-xss) and [potential email header injection vulnerabilites](http://www.djangobook.com/en/2.0/chapter20.html#e-mail-header-injection)
    * Use `htmlentities($value, ENT_QUOTES, "utf-8")` ([documentation](http://php.net/manual/en/function.htmlentities.php)) to present any user submitted content (PHP), [auto escaping](https://docs.djangoproject.com/en/dev/topics/templates/#automatic-html-escaping) (Django) or `cgi.escape` ([documentation](http://docs.python.org/2/library/cgi.html#functions)) (Python).
* Review site for [directory traversal vulnerabilities](http://www.djangobook.com/en/2.0/chapter20.html#directory-traversal).

### Static

* If not already in place, would the site be better served by a CDN such as Amazon S3?
* Enable caching of static content:

    On Apache, via .htaccess:

        # Turn on Expires and set default expires to 3 days
        <IfModule mod_expires.c>
            <IfModule mod_headers.c>
                ExpiresActive On
                ExpiresDefault A259200

                # Set up caching on media files for 1 month
                <FilesMatch "\.(ico|gif|jpg|jpeg|png|flv|pdf|swf|mov|mp3|wmv|ppt|svg)$">
                  ExpiresDefault A2419200
                  Header append Cache-Control "public"
                </FilesMatch>
                
                # Set up 2 Hour caching on commonly updated files
                <FilesMatch "\.(xml|txt|html|js|css)$">
                  ExpiresDefault A7200 
                  Header append Cache-Control "private, must-revalidate"
                </FilesMatch>
                
                # Force no caching for dynamic files
                <FilesMatch "\.(php|cgi|pl|htm)$">
                  ExpiresDefault A0 
                  Header set Cache-Control "no-store, no-cache, must-revalidate, max-age=0"
                  Header set Pragma "no-cache"
                </FilesMatch>
            </IfModule>
        </IfModule>

* Enable gzip compression of static files: 

    On Apache, via .htaccess:


        # gzip
        <IfModule mod_deflate.c>
            SetOutputFilter DEFLATE
        </IfModule>

### Security

#### SSL

* Does the site need an SSL certificate?
    * [When to use SSL/TLS](https://wiki.mozilla.org/WebAppSec/Secure_Coding_Guidelines#When_To_Use_SSL.2FTLS).

#### File Browsing (Static Files servers and PHP servers)

* Establish what kinds of files shouldn't be indexed (.htaccess, .inc, .bak, .svn, etc.).

    On Apache, via .htaccess:

        # don't display files that begin with ., end with ~ or end with # in the directory index
        IndexIgnore .??* *~ *#
        
        # hide .htaccess file in the directory index
        IndexIgnore .htaccess
        
        # hide foo.inc in the directory index
        IndexIgnore *.inc
        
        # hide foo.bak in the directory index
        IndexIgnore *.bak

* Establish what kinds of files should be blocked if access is attempted (.htaccess, .inc, .bak, .svn, etc.) (PHP sites with database config files especially should not be indexed and should have access attempts blocked):

    On Apache, via .htaccess:

        # block access to .htaccess file
        <Files ~ ".htaccess">
            Order allow,deny
            Deny from all
        </Files>
        
        # block access to .inc files
        <Files ~ ".inc$">
            Order allow,deny
            Deny from all
        </Files>
        
        # block access to .bak files
        <Files ~ ".bak*">
            Order allow,deny
            Deny from all
        </Files>
        
        # block access to .svn directories
        # turn on rewriteengine if it isn't already
        RewriteEngine On
        RedirectMatch 404 /\.svn(/|$)

* Deny directory tree browsing:

    On Apache, via .htaccess:

        # Deny directory browsing
        Options -Indexes

#### Django Security Configuration

* [Django-Secure](https://pypi.python.org/pypi/django-secure) provides utilities and a 'linter' to help make a Django site more secure.

#### PHP Security Configuration

_On WebFaction a [php.ini file will need to be created](http://docs.webfaction.com/software/php.html#id4) before being configured._

* Turn off `magic quotes` ([documentation](http://www.php.net/manual/en/info.configuration.php#ini.magic-quotes-gpc)):

    In php.ini:

        magic_quotes_gpc = "0"

* Turn off `register_globals` ([documentation](http://www.php.net/manual/en/ini.core.php#ini.register-globals)):

    In php.ini:

        register_globals = "0"

* Turn off `display_errors` ([documentation](http://www.php.net/manual/en/errorfunc.configuration.php#ini.display-errors)) and turn on `log_errors` ([documentation](http://www.php.net/manual/en/errorfunc.configuration.php#ini.log-errors)) setting a path to the `error_log` ([documentation](http://www.php.net/manual/en/errorfunc.configuration.php#ini.error-log)) instead:

    In php.ini:

        display_errors = "0"
        log_errors = "1"
        error_log = "/path/to/a/log.log"


### Monitoring

* Does the site need [uptime monitoring](http://pingdom.com)?
* Does the site need [real-time server metrics](http://newrelic.com)?

## Redirects

_In the event that the site is an update to an existing site and not a new site the following criteria should be met. This requires SEO approval._

* Review `site:domainname.com` in Google search for a list of existing indexed pages.
* Create redirects of a permanent nature for those pages into the new site where appropriate.
* Work with SEO to establish how pages match between the old and new sites.
* Test that those old indexed URLs properly redirect into the new site.