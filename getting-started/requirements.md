# Requirements

<p class="uk-article-lead">Pagekit requirements are few, and should be fulfilled by any modern webserver.</p>

## System requirements

Make sure your server meets the following requirements.

- Apache 2.2+ or nginx
- MySQL Server 5.1+ or SQLite 3
- PHP Version 5.4+

## PHP extensions

In addition, Pagekit needs the following PHP extensions to be enabled: [JSON](http://php.net/manual/book.json.php), [Session](http://php.net/manual/book.session.php), [ctype](http://php.net/manual/book.ctype.php), [Tokenizer](http://php.net/manual/book.tokenizer.php), [SimpleXML](http://php.net/manual/book.simplexml.php), [DOM](http://php.net/manual/book.dom.php), [mbstring](http://php.net/manual/book.mbstring.php), [PCRE](http://php.net/manual/book.pcre.php) 8.0+, [ZIP](http://php.net/manual/book.zip.php) and [PDO](http://php.net/manual/book.pdo.php) with [MySQL](http://php.net/manual/ref.pdo-mysql) or [SQLite](http://php.net/manual/ref.pdo-sqlite) drivers.

Although it is optional, we strongly recommend enabling the following PHP extensions: [cURL](http://php.net/manual/book.curl.php), [iconv](http://php.net/manual/book.iconv.php) and [XML Parser](http://php.net/manual/book.xml.php), as well as [APC](http://php.net/manual/book.apc.php) or [XCache](http://xcache.lighttpd.net/) for caching.

## Browser requirements

The administration interface of Pagekit is compatible with Internet Explorer 8 and later. Chrome, Firefox, Opera, and Safari have an automatic update system, therefore we only support the latest versions for those browsers.

The browser support for the frontend is dependent on the theme your site is currently using.
