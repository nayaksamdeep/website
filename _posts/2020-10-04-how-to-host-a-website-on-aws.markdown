---
layout: post
title:  "How to host a website on AWS"
date:   2020-09-27 00:36:19 +0530
categories: aws ci/cd 
permalink: /blogs/
---
In my previous blog, we covered the steps involved in creating the
content for the static website. In this blog, we will go through the
steps involved in hosting the site on AWS as well as automating the
Continuous Integration and Continuous Deployment using AWS Code Pipeline
and Code Build Services

**Step 1 -- Register your favorite Domain**

AWS provides Route 53 service to register a domain. This is fairly
straight forward process and the details are documented
[here](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/domain-register.html)
For customers registering the domain from India, they will need to make
the payment first before they can complete the Domain registration.

**Step 2 -- Create S3 Buckets**

Go ahead and create 2 S3 buckets that match the domain name. E.g. if you
have picked the domain as "mysite.com" and a subdomain of
"www.mysite.com", you can name the buckets accordingly. For the S3
bucket (that matches mysite.com in this example), configure the property
as static website. In addition, you will need to set the permission to
Public for the Access Control List and Bucket Policy. More details are
[here](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html)

For the second bucket that matches the subdomain (in this case
www.mysite.com) , configure the static website property to redirect the
request your domain (in this case mysite.com )

**Step 3 -- Push the blog content to S3**

As mentioned in the previous blog, run "Jekyll build" to create the blog
content. The bits are stored under **\_site** directory. You can go to
S3 bucket and upload all the contents of **\_site** directory using the
console. Your blog is ready to be viewed on your own domain

**Step 4 -- Automating the CI/CD**

Instead of manually building and uploading the bits to the S3 bucket, we
can leverage the Code Pipeline and Code Build services to automate this
step. As a pre-requisite, you will need to check-in the code into github
or any other modern code repository.

Code build brings up a docker image that can pull the source code,
install the dependencies and build the bits.

Code pipeline polls on the git repository for any check-in. Once a code
is checked-in, it trigger Code build to build the bits and push it to
the S3 bucket. In this example, I have not leveraged AWS Code Deploy
services

