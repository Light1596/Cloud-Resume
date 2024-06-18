# CLOUD RESUME PROJECT

## About the Project
The goal was to create a static cloud resume website.

## Architecture Diagram
![Hosting a Static website architecture diagram - Page 3 (1)](https://github.com/Light1596/Cloud-Resume/assets/127042301/c137287a-3230-4563-97a2-0d99cd6a5389)

## Pre-requisites
+ An active AWS account

## Steps

### S3 

1. Create an s3 bucket and enable public access. Proceed to attach the policy below
```
{
"Id": "Policy1718745369258",
"Version": "2012-10-17",
"Statement": [
{
  "Sid": "Stmt1718745362146",
  "Action": [
    "s3:GetObject"
  ],
  "Effect": "Allow",
  "Resource": "arn:aws:s3:::${BucketName}/",
  "Principal": "*"
}
]
}
```
This allows the public to access the objects in the s3 bucket. Replace the `${BucketName}` with the name of your bucket. Be sure to use this same name when creating a hosted zone in Route 53. This will prevent the `No Resources Found` when creating an alias.

2. Upload the objects into the bucket.
   
3. Enable the bucket for static website hosting. This is done under `properties` tab of the bucket. It is disabled by default. Specify the home or default file as the `index.html`. This name has to be the same name of the index file you uploaded. The bucket becomes a website endpoint. The url provided can be used to access the objects in the bucket via http

### AWS Certificate Manager
An SSL certificate is needed to redirect http to https.
+ Provide a fully qualified domain name then request for a certificate. You can purchase a domain name from [here](https://www.hostafrica.ke/)
  
### CloudFront Distribution
+ Use the s3 bucket you created in the first step as the origin.
+ Provided an SSL certificate issued by the <mark>AWS Certificate Manager</mark> in the previous step.
+ Provided an alternate domain name(This is important when created an A record in Route 53. The alias will reference this domain you provided else `No Resources Found` will pop up when creating the record).
+ Enabled WAF and leave the rest of the settings at default.
+ Proceed to create a distribution.

### Route 53
+ Create a hosted zone. Use the domain name you bought.
+ Copy the name servers under `NS record` one by one and paste them under `manage nameservers` from where you bought the domain name. Make sure the nameservers provided here in this record match the nameservers from where you bought the domain name. You will have to manually copy and paste. Watch the Project Walkthrough below👇
+ Create an A-record and aliased to cloudfront distribution. This maps the cloudfront distribution to a custom domain name.

## Project Walkthrough
I used an old machine to make the video. There is a lot of fan noise noise in the background. Apologies for this, I will work on it next time.

[Project Walkthrough](https://drive.google.com/file/d/1b14P7NXy-ZoS9__dXrKrihzvnssnfY8R/view?usp=drive_link)

## Future Features
+ Implement CI/CD Pipeline using GitHub Actions

## Cost
Some services aren't covered by the free tier. You will be billed for the hosted zone and WAF. The rest of the services are free provided you are within the limits of the free tier.

## Authors
[Linkedin](www.linkedin.com/in/light-situma-35b522166)

<lightsituma@gmail.com>

## Acknowledgements
I am grateful to [Anne Andega](https://www.linkedin.com/in/anne-andega?miniProfileUrn=urn%3Ali%3Afs_miniProfile%3AACoAAD4okqgBAYePr9vokcGJsQsEXFI-ciETjY4&lipi=urn%3Ali%3Apage%3Ad_flagship3_search_srp_all%3BWssziKLWRmev0GietXddww%3D%3D) for the syle.css and index.html templates


