Page Link Abstractor module for ProcessWire 2
=============================================

WHAT IT DOES
------------

Converts links in textarea/rich-text fields to an abstract format 
for storage, and converts them back at runtime. This means that if 
you move a page and another page is linking to it, the link won't 
be broken.  It also means you can move your site from subdirectory 
to root (or the opposite) and not break links you may have created 
in your textarea fields. This applies to any kind of links: pages,
files, images, etc. 

This module will also notify you when you edit a page that has a 
link to a page that doesn't exist or is in the trash.

This module has been tested with ProcessWire 2.1 but should also
work with 2.0.

Since this module has not yet had a lot of testing, it should be
considered "beta" and use is at your own risk. Please let me know
of any issues or bugs you run into.


HOW TO INSTALL
--------------

1. Download and place the PageLinkExtractor.module file in:
/site/modules/

2. In the admin control panel, go to Modules. At the bottom of the
screen, click the "Check for New Modules" button. 

3. Now scroll to the PageLinkAbstractor module and click "Install".

4. Edit any of your textarea fields in Setup > Fields. You'll see 
a new configuration option to enable this module for that field.


HOW IT WORKS
------------

This is a technical explanation for how this module works for
those that are interested. Reading this is not required in
order to use this module, but it may help you to use it more
effectively. 

When you save a page that has a textarea field with this module
enabled, it will look for URLs in HTML attributes by looking for
an equals sign followed by a URL. It replaces instances of your
site's root URL with a special tag: {~root_url}

Next it checks to see if any of the URLs it found can be loaded
as pages in your site. If so, it replaces those URLs with this
special tag: {~page_123_url} where "123" is the Page's ID. 

When the page is loaded, ProcessWire does the opposite and 
converts those special tags back to their URLs. Because the URLs
were abstracted to tags that are generated at runtime, when a page
(or a site) is moved, no links are broken. 

Note that this module only converts URLs to tags when you save
a page, so it only affects pages saved after the module is installed.


WHERE TO USE IT
---------------

This would be most useful on your main 'body' field that
uses a rich text editor (like TinyMCE). 


WHERE NOT TO USE IT
-------------------

There is some overhead in using this module that will be
insignificant if you use it carefully. Here are a few 
instances to avoid using it: 

Avoid use on fields that have the 'autojoin' option on, unless 
your site doesn't load lots of pages in a given request. 

Don't use on textarea fields that can contain anonymous (guest)
user input. 

Avoid use on fields that aren't likely to contain links to
local site pages in HTML markup. No need to have this module
parsing things unnecessarily. 

Avoid use on fields where you think you might disable it later.
Once disabled, the abstract tags representing the URLs will
still be in place. If the module is disabled, those tags will 
no longer be converted to URLs are runtime. You would have to 
correct them manually by editing the page.


SIDE BENEFIT
------------

The tags that this module abstracts to are intentionally fulltext
indexable, so you can perform searches for these tags. 

This means that you can search for all pages linking to to another
by searching for it (minus the brackets and "~"). For example:

$links = $pages->find("body~=page_123_url"); 

That would return all [viewable and visible] pages linking to 
page ID 123. 


PLEASE NOTE
-----------

In order to convert URLs for pages, this module needs to load those
pages in order to obtain their URL. If you are linking to a hundred
pages in your 'body' field, you should expect that it may slow down
the page load and save time for pages containing lots of links. 

This module doesn't yet abstract local URLs that have a 
schema/protocol and domain in it. It just works with path-type 
links like /path/to/page/ and not http://domain.com/path/to/page/.

This module hasn't yet been tested with migrating a site from 
subdirectory to root, but I will be testing this soon. 

