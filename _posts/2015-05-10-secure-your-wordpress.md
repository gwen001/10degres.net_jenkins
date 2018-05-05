---
title: Secure your Wordpress
tags:
- security
- wordpress
see_also:
  -
    link: Official WordPress
    url: https://wordpress.org/
  -
    link: How to choose a good password
    url: /choose-your-password/
  -
    link: Sucuri blog
    url: https://blog.sucuri.net/
  -
    link: Official Fail2ban
    url: http://www.fail2ban.org/
---
With more than 60 million websites, WordPress is the most popular CMS currently in use but it's also based on the most hacked environment aka LAMP. 

As we all know, there is no way to stop a determined hacker but you can slow him down or detect him before things become serious. 
Below some techniques to improve the security of your site. 
This post is directly inspired from [Wordpress official codex](http://codex.wordpress.org/Hardening_WordPress){:class="flashlink" target="_blank"} and some hackers techniques I learned last months.

## Files

According to Worpdress documentation, and I won't discuss this point here, directories must have the following permission: `drwxr-xr-x` (755) and files must be: `-rw-r--r--` (644). 
Wordpress says that automatic update changes file/dir permissions, that's true but not that way in my case, maybe a cron job could do it ?

<!--more-->

## Database

It's very important that your database is protected with strong credentials, check the post about [how to choose a good password](http://blog.10degres.net/choose-your-password/). 
Keep in mind that a SQL user must be only used for one application and **must not have administrator privileges**.

It's also possible to change the table prefix which is `wp_` by default, this will prevent from automatic SQL injection attacks. 
However this must be done while performing the installation, it's too hard/dangerous to change that parameter on a running app.

Backup your database very often, once a week is a good average but it actually depends on your post frequency. 
Of course the data files must not be saved under the web tree.

## Core

When a new bug is found it's quickly registered on many security listing. 
Years later, it's still there with the list of infected versions.

Keep your Wordpress up to date. For each new release you'll get a warning in your dashboard. 
If you have automatic update enable (implemented in version 3.7), you'll receive a message but you'll not need to do anything.

It can be usefull to hide the current version of your Wordpress with this small script in `functions.php`:

~~~php
function remove_wp_version_tag() {
  return null;
}

add_filter( 'the_generator', 'remove_wp_version_tag' );
~~~

## Users

The point here is to hide usernames as much as possible.

Rename the default user admin to whatever you want because this username is the first target of a brute force attack.

Give your users a nickname, which should be selected as public and different than the username.

Disable authors dedicated page with this small script in `functions.php`:

~~~php
add_action('template_redirect', 'bwp_template_redirect');

function bwp_template_redirect() {
    global $wp_query, $post;
    if (is_author() ) {
        $wp_query->set_404();
    }
}
~~~

You can also remove the tag `<dc:creator>` from all feed: `wp-admin/includes/export.php`, `wp-includes/feed-rss2-comments.php`, 
`wp-includes/feed-rss2.php` and `wp-includes/feed-rdf.php` however those files could be overwrited at the next update.

Of course users must have a strong password, a single user access, even with low privileges, can be deadly.

## Plugins & Themes

It's very important to stay aware about updates, most of the times they fixe small bugs but sometimes it's about security. 
You wouldn't keep a Local File Inclusion because of a gallery plugins, even if it's disable. Btw remove disabled plugins/themes, you don't need them and you probably won't care about patches...

Try to hide the current version of your plugins  and themes by removing `.txt` files who disclose to much infos (versions, patches, bugs, ...) as well as most `.html` files. 
Those two functions prevent display of version numbers in the html source of your site, in `functions.php`:

~~~php
function remove_wp_version_strings( $src ) {
  global $wp_version;
    parse_str(parse_url($src, PHP_URL_QUERY), $query);
  if ( !empty($query['ver']) && $query['ver'] === $wp_version ) {
    $src = remove_query_arg('ver', $src);
  }
  return $src;
}

add_filter( 'script_loader_src', 'remove_wp_version_strings' );
add_filter( 'style_loader_src', 'remove_wp_version_strings' );

// remove wp version param from any enqueued scripts
function vc_remove_wp_ver_css_js( $src ) {
    if ( strpos( $src, 'ver=' ) )
        $src = remove_query_arg( 'ver', $src );
    return $src;
}

add_filter( 'style_loader_src', 'vc_remove_wp_ver_css_js', 9999 );
add_filter( 'script_loader_src', 'vc_remove_wp_ver_css_js', 9999 );
~~~

## Admin & Login

To protect admin section you can restrict `wp-admin` directory and `wp-login.php` to trusted IP address. In `/.htaccess`:

~~~
<filesmatch "wp-login"="">
  order deny,allow
  deny from all
  allow from xxx.xxx.xxx.xxx
</filesmatch>
~~~

In `/wp-admin/.htaccess`:

~~~
order deny,allow
deny from all
allow from xxx.xxx.xxx.xxx
~~~

To prevent brute force, and if you have a server with **fail2ban** installed, there is a Wordpress plugin planned to interact with it: 
[Wordpress fail2ban plugin](https://wordpress.org/plugins/wp-fail2ban/){:class="flashlink" target="_blank"} (the installation requires root access to the server).

## Tools

To test all changes you can use [WPScan](http://wpscan.org/){:class="flashlink" target="_blank"}, this tool is a great WordPress vulnerability scanner.

Sucuri Team has developped a [security plugin](https://wordpress.org/plugins/sucuri-scanner/ "Wordpress Sucuri plugin"){:class="flashlink" target="_blank"} who will ~~spam~~ send you reports about: 
file integrity, brute force, updates, log in and so on...
