---
layout: post
title:  "How to set up a blog with jekyll in github in 5 minutes"
date:   2015-09-28 18:13:47
---

To set up just a simple blog in 5 minutes you just need to:

*   Install ruby in your machine.
*   Run the following commands in the command line:

{% highlight ruby %}
$ gem install jekyll
$ jekyll new my-awesome-site
{% endhighlight %}

That will create and empty jekyll website with a single blogpost. You can just go an manually edit the post under the folder "_posts".

If you want to browser your website locally: 
{% highlight ruby %}
$ cd my-awesome-site
$ jekyll serve
{% endhighlight %}
And go to http://127.0.0.1:4000 in your web browser.

If you already have a github account you can host your site in githubpages. To do so just create a repository with the name [username].github.io and point and point the origin of the folder with the website to it.

{% highlight ruby %}
$ git init
$ git add .
$ git commit -m "Initial commit"
$ git remote add origin git@github.com:[username]/[username].github.io.git
$ git push origin master
{% endhighlight %}

And that's it! Now you have a self hosted blog up an running in the internet! 

Happy blogging! :)

