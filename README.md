# WordPress Kickstarter

This is a basic repo that I use to kickstart my WordPress sites. It was inspired by and based on Mark Jaquith's WordPress-Skeleton but I have since made many changes to better fit the way I work. I'm in the process of writing a post detailing my WordPress Development Environment and Deployment Strategy which this is a core compnenet of. Use it to kickstart your WordPress site repos, or fork it and customise it to your own liking!

## Overview

* WordPress is included as a Git submodule in `/wp/`
* There is a custom content directory in the root `/wp-content/` (it can't be in `/wp/`)
* `wp-config.php` is in the root (because it can't be in `/wp/`)
* There is a .gitignore file

## Instructions

### Managing WordPress as a Git Submodule 

When you clone this repo you will notice that the `/wp` directory is initially empty. To fix that you need to run two commands, `$ git submodule init` and `$ git submodule update`. The WordPress repo is synced via SVN every 15 minutes and includs branches and tags. You will want to ensure your project is using the current stable version of WordPress. As of writing, this is 3.5.1. To do that `$ cd wp` and `$ git checkout 3.5.1`. This has now updated the WordPress repository to 3.5.1. Commit the changes to your main project by running `git commit -am "Checkout Wordpress 3.5.1"`. The subrepository is now isolated, and we don’t want to make any changes to it, besides updating WordPress. When you want to do that you simple checkout and commit the latest version. 

Resources:

* http://davidwinter.me/articles/2012/04/09/install-and-manage-wordpress-with-git/
* http://git-scm.com/book/en/Git-Tools-Submodules

### The Content Directory

With WordPress isolated in a subrepository we need a custom `/wp-content/` directory. This gives us the oportunity to not include certain plugins and themes in our base install or specify alternate ones. For instance I have removed Hello Dolly.      

#### About mu-plugins

Must-use plugins (a.k.a. mu-plugins) are plugins which are automatically enabled on all sites in the installation. Must-use plugins do not show in the Plugins page of wp-admin and cannot be disabled except by removing the plugin file from the must-use directory. Some mu-plugins I use include WordPress SEO, Backup Buddy and the Ultimate Comming Soon Page. 

#### About themes

The default WordPress themes are symlinked. I use a theme called _s, or underscores for nearly all the sites I develop from scratch. - http://underscores.me/

#### About uploads

The `/wp-content/uploads` directory is one of the locations in my `.gitignore` file. I don't keep my `wp-content/uploads` directory under version control for the following reason:

* As soon as a client starts uploading media on production, the repo will be out of date.

* By default, WordPress generates multiple copies of every uploaded image (thumbnail, medium, large). If all of those images are in your repo that means more bandwidth, larger repos, and no real benefit.

Instead, inspired by [this post](http://stevegrunwell.com/blog/keeping-wordpress-under-version-control-with-git), I use a rewrite rule to check the local `wp-content/uploads` directory for the requested file(s) and, if it doesn’t exist, attempt to load it from production.

Note: Mark Jaquith uses a `/shared` directory and some symlink stuff with Capistrano. 


### One 'wp-config' to Rule them All 

Instead of using Mark Jaquith's `local-config.php` method I prefer to use a global config file that checks which environment I am on, dev, staging or production, and loads the relevant database information. It also enables me to do some other nifty things based on the environment.   

Thanks to http://abandon.ie/wordpress-configuration-for-multiple-environments/ for this fantastic solution. 

