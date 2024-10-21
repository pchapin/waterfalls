
# Waterfalls of New England

Waterfalls of the New England Region. This repository contains my waterfall website. You can
find the site online at
[https://www.pchapin.org/waterfalls](https://www.pchapin.org/waterfalls). It is my intention to
keep this URL stable for the foreseeable future.

The site uses Bootstrap 5.3.3, but the introduction of Bootstrap into the site is still in the
early stages. PHP is also used, but currently only to manage file inclusions (e.g., of the
signature into every page). It is my plan to eventually use PHP to query a database of waterfall
information.

This repository makes use of Git LFS. Be sure you have Git LFS installed before you clone this
repository. If the command `git lfs version` returns an error about how `lfs` is an unknown Git
command, then you do *not* have Git LFS installed, and you will need to address that.

It has been a long time since I've worked on this page, but I hope to change that in the comming
weeks/months. I have a lot of organization and stylistic changes in mind, as well as potential
new features to add. Finally, and of course, I hope to also visit more waterfalls and gather new
trip reports and pictures to add here.

Enjoy!

Peter Chapin  
waterfalls@pchapin.org  

## Setup

I primarily use PhpStorm on Windows for development. I use Visual Studio Code as a secondary
editor. Here I will describe how you can configure your environment the same way I have. Similar
steps can likely be used on other platforms.

First, install a local Apache instance as a testing server. Also install PHP locally. (_TODO_
Provide more details on how to set up the baseline Apache and PHP systems)

Next, clone the repository using a command line Git client, but GitHub Desktop would also work
well. I put my clone in C:\\Projects\\Waterfalls, but any location on your system will work.

### Apache Configuration

Modified the Apache configuration in the following ways:

First, uncomment the line:

    LoadModule rewrite_module modules/mod_rewrite.so

This site uses rewriting rules, and the rewriting module needs to be available. Next, add the
following `Directory` block:

    # Added to support testing my Waterfalls of New England site.
    Alias /waterfalls "C:/Projects/Waterfalls"
    <Directory "C:/Projects/Waterfalls">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
    
Note that in my experience it is essential to use an upper case letter for the drive specifier
(on Windows).

Finally, add `index.php` to the list of files that can be used for directory indexes:

    <IfModule dir_module>
        DirectoryIndex index.html index.php
    </IfModule>

After restarting Apache, you shoudl now be able to access the waterfall site locally using
[http://localhost/waterfalls](http://localhost/waterfalls). Assuming PHP is properly installed
in your local Apache server, the site should work normally via that URL. No database is
(currently) required.

### PhpStorm Configuration

When you view a page in the editor, PhpStorm allows you to launch the page by clicking on the
browser icons that hover in the upper right corner of the window. Launching a page that way will
cause PhpStorm to use its internal server to process the page (you need to first tell PhpStorm
where the PHP processor is located).

This is fine for a quick test of a particular page. Unfortunately the internal web server in
PhpStorm does not know about the rewriting rules. As a consequence, none of the local links will
work and you can't navigate the site at large this way.

However, you can configure PhpStorm to launch the site via Apache for you.

In the Settings for PhpStorm, go to PHP > Servers and add a server definition. Use whatever name
you like (I used "Local Apache Server"), and set the host to localhost, port 80. Click on the
"Use path mappings" checkbox, and specify the path on the server as `/waterfalls` that
corresponds to the physical location of the project in the local file system.

I believe it is necessary to use path mappings in this case because of the Alias definition in
the Apache configuration file. However, I might be mistaken.

Next, from the Run menu of PhpStorm, select Edit Configurations. In the dialog that appears, add
a new Run/Debug configuration of type PHP Web Page. Give the configuration a suitable name and
select your "Local Apache Server" as the server. The Start URL is the page that will be loaded
by the run configuration. A value like `/waterfalls` will load the root index page, but you can
specify any start page that is convenient (or make multiple run configurations for different
pages). 

When you launch this run configuration, PhpStorm will start the browser you select in the
configuration and load the Start URL you selected. Since the site is being processed by your
local server and not PhpStorm's internal server, it should work normally, meaning all the links
will work because Apache will honor the rewriting rules.

Unfortunately PhpStorm's editor also does not fully understand the rewriting rules. It will
"resolve" links that point at other files in the same folder as the page on which the link is
located, _provided_ there is a `.htaccess` file in that folder). For example, on the main index
page, the link

    <a href="list.html">...</a>
    
is resolved even though there is no file `list.html` (it's called `list.php` due to the
rewriting). However, a link such as

    <a href="VT/Old-City-Falls.html">...</a>
    
is *not* resolved. The file `VT/Old-City-Falls.html` does not exist, but the name should be
rewritten to `VT/Old-City-Falls.php`. PhpStorm doesn't understand this and marks the link as an
error. I do not know of a way to fix this in PhpStorm, so I disabled that particular inspection
in this project. That is unfortunate because detecting broken links is a useful service. Perhaps
I will write a Python script later that does that task.

(_TODO_ Talk about setting up PHP debugging)

### Visual Studio Code Configuration

(_TODO_ Write something here)
