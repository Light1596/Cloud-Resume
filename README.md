# CLOUD RESUME PROJECT

## Table Of Contents
1. [Introduction](## Introduction)
2. [Architecture Diagram](## Architecture Diagram)

## Introduction
The goal was to create a static cloud resume website.

## Architecture Diagram
![Hosting a Static website architecture diagram - Page 3](https://github.com/Light1596/Cloud-Resume/assets/127042301/02fc6591-6a65-45e0-94f2-b8e29e902716)



### S3 Part

1. Creation of an s3 bucket. I enabled public access and proceeded to attach a policy with an action to allow `GetObject` and the principle as `*`. This allows the public to access the objects in the s3 bucket. I gave the bucket a name similar to my domain name because I realised that giving it a different name from the domain name used in creating a hosted zone in Route 53 results in a `No Resources Found` error. In my case the name given to the bucket was `lightsituma.buzz`.
2. Upload of the objects into the bucket. Shout out to Anne Andega for the `index.html` and `style.css` templates. I modified them to suit my needs. I uploaded the two files together with `profile_photo.jpeg`.
3. Enable the bucket for static website hosting. Under `properties`tab of the bucket, I enabled static website hosting. It is disabled by default. I specified the home or default file as the `index.html`. This name has to be the same name of the index file you uploaded.
4. Upon completion of this step, the bucket becomes a website endpoint and a url is provided that can be used to access the objects in the bucket via http

### CloudFront Distribution
I used the s3 bucket as the origin, redirected http to https, provided an SSL certificate that had been issued by the <mark>AWS Certificate Manager</mark>(You need a domain name for this), provided an alternate domain name(This is important when created an A record in Route 53. The alias will reference this domain you provided else `No Resources Found` will pop up when creating the record). I enabled WAF, left the rest of the settings at default and proceeded to create a distribution

### Route 53
To use Route 53, I needed a domain name. So I bought one from hostAfrica `lightsituma.buzz` and used it to create the hosted zone. Upon creation of the zone, two records are `NS record` and the `SOA record` are created by aws for you. Make sure the nameservers provided here in this record match the nameservers from where you bought the domain name. You will have to manually copy and paste. Watch the video below 

I then created an A-record and aliased to cloudfront distribution. I chose the cloudfront distribution I had created above and mapped it to the root domain name. Provision of a subdomain is optional.

The users could now access the resume using `lightsituma.buzz`

## Project Walkthrough
I used an old machine to make the video. There is a lot of fan noise noise in the background. Apologies for this, I will work on it next time.

[Project Walkthrough](https://drive.google.com/file/d/1b14P7NXy-ZoS9__dXrKrihzvnssnfY8R/view?usp=drive_link)

## Acknowledgements
The style.css and index.html templates were provides by [Anne Andega](https://www.linkedin.com/in/anne-andega?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAD4okqgBAYePr9vokcGJsQsEXFI-ciETjY4&lipi=urn%3Ali%3Apage%3Ad_flagship3_search_srp_all%3BWssziKLWRmev0GietXddww%3D%3D)


