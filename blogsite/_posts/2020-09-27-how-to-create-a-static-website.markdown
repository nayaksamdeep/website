---
layout: post
title:  "How to create a static website"
date:   2020-09-27 00:36:19 +0530
categories: jekyll staticsite 
permalink: /blogs/
---
A static website is one that hosts pages that are not editable. By
contrast, a dynamic website has a server side component and is
interactive. While blogs are a common use case for static websites, they
are also used for single page web applications. 

In my example to create a static website, I am using Jekyll for the
static blogging platform, AWS for the hosting infra and GitHub for
version control. However if you are serious about blogging, I suggest
you go through [this site][blogging-basics] before considering jekyll

**How to create a static website**

**Step 0 - Before you start**

You will need the following

-   AWS Account

-   Github Account

-   Computer (Mac in my case) with git and ruby installed

**Step 1 - Set up your Development Environment**

*Install jekyll*

*Commands*

-   *gem install jekyll*

[Create a new repository for the website in github and clone it]{.ul}

*Commands*

-   *git clone
    https://github.com/)\<replace with your github account\>/\<repository name\>*

*Build a default Website*

*Commands*

-   *jekyll new my-jekyll-site*

-   *cd my-jekyll-site*

-   *jekyll serve*

You should be able to point to port 4000 on your localhost and view the
blogging site that you just created

**Step 2 - Understanding Jekyll**

When you run the jekyll command, the blog site bits are stored under the
**\_site** directory.

You can copy the contents of **\_site** to your target system and render
the blog. We will cover this in detail in the later sections

Jekyll supports multiple **themes**. I am using the minima which is the
default theme. Pls refer to the
[tutorials][tutorial-link] for using alternate themes in your blog

The main entry point is called \_**config.yml** You can edit title,
description and other details to reflect your blog

For each of the markdown, you can provide details such as **layout**
(home, page or posts), **permalink** (the way you want the URL to
appear), **categories** (keywords associated)

**Step 3 - Creating a Jekyll Blog Post**

The blogs are created under the **\_posts** directory. Jekyll requires
blog post files to be named according to the following format:

\`**YEAR-MONTH-DAY-title.MARKUP**\`

Where \`YEAR\` is a four-digit number, \`MONTH\` and \`DAY\` are both
two-digit numbers, and \`MARKUP\` is the file extension representing the
format used in the file.

If you are not comfortable editing the markdown on your computer, there
are plenty of tools available. Alternatively, you can write your blog as
a doc and convert it into markdown using [pandoc tool][pandoc-tool]

**Step 4 - Review the Blog Post and Check-In the Code**

You can review the blog as it appears by running the command ***jekyll
serve***

You can view the contents at Port 4000 through your browser

Now, run git add, commit and push to check-in your new blog site

**Step 5 - Upload the files into S3 and test**

*Create a S3 bucket*

It is a good practice to name your S3 bucket the same as your site name.
We will cover details of creating your own domain in the next blog

*Configure S3 bucket*

Configure S3 bucket to be used for static website

By default, the S3 bucket contents are private. Pls go ahead and make
the contents of this bucket public

*Upload blogs to S3 bucket*

Now upload all the contents in the \_site directory into the S3 bucket.
Once this is successful, you should be able to view your blog by
accessing S3 URL


[blogging-basics]: https://www.bloggingbasics101.com/how-do-i-start-a-blog
[tutorial-link]: https://jekyllrb.com/tutorials/video-walkthroughs
[pandoc-tool]: https://pandoc.org
