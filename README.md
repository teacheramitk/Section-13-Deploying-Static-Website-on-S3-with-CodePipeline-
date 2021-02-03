# Section-13-Deploying-Static-Website-on-S3-with-CodePipeline-


# Create S3 Static Website - Lab

**Step 1.Goto AWS Management Console>All Services>S3>Create Bucket**

Provide the following details-
- Provide globally unique Bucket name - "static-website-example"
- Region - Asia Pacific(Mumbai) ap-south-1
- Block all public access - uncheck this
- Bucket versioning - leave as for now
- Give acknowledgement for becoming public 
- Leave other things default

Click on Create Bucket

**Step 2.Bucket "static-website-example"  has been created successfully**

**Step 3.Goto Buckets>static-website-example>Permissions>Bucket Policy>Edit**
- Paste the following policy to make it public

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::static-website-example/*"
            ]
        }
    ]
}
```

Click on Save changes

**Step 4.Goto Buckets>static-website-example>Properties>Bucket Versioning>Edit**
- Click on Enable 

Click on save changes

**Step 5.Goto Buckets>static-website-example>Objects>Upload>Add files**
- Select about.html and index.html files to upload
- Click on Upload
- Click on Exit to return back 

**Step 6.Goto Buckets>static-website-example>Properties>Static website hosting>Edit**
- Select Enable
- In Hosting type - Select Host a static website
- index document - index.html
- Error document - n/a

Click on Save changes

**Step 7.Goto Buckets>static-website-example>Properties>Static website hosting**
- Click on Endpoint URL 
  - e.g. http://static-website-example.s3-website.ap-south-1.amazonaws.com

**Step 8.See that the static website is working**

### End of lab


# Push Static Website Code to CodeCommit - Lab

**Step 1.Goto AWS Console>CodeCommit>Repositories>Create Repository**

In repsitory settings-
- Repository name - s3-static-website

Click on Create

**Step 2.Repository created successfully**
- Repositories>s3-static-website>Clone URL>Click Clone HTTPS

**Step 3.Open Virtual Studio and type the following commands**
```sh
$ git init
$ git remote add origin "Clone HTTPS URL"
$ git status
$ git add .
$ git commit -m "first commit"
$ git push -u origin master
```

**Step 4.Goto AWS Console>CodeCommit>Repositories>s3-static-website**
- Check and validate that the code has been pushed

### End of lab

#  Deploy Static Website with CodePipeline (on S3) - Lab

**Step 1.Goto AWS Management Console>Services>Developers Tools>CodePipeline>Create Pipeline**

**Step 2.In Pipeline Settings give following details:**
- Pipeline name - s3-static-website-cp
- Service role - Existing service role
- Advanced settings
  - Artifact store - Default location
  - Encryption key - Default AWS Managed key

Click on Next

**Step 3.Source in Add source stage:**
- Source provider - AWS CodeCommit
- Repository name - s3-static-website
- Branch name - master
- Change detection options - Amazon CloudWatch Events
- CodePipeline - CodePipeline default

Click on Next

**Step 4.Build - optional in Add build stage**
- Skip build stage>Skip


**Step 5.Deploy-optional in Add deploy stage**
- Deploy provider - Amazon S3
- Region - Asia Pacific(Mumbai)
- Bucket - static-website-example
- Select Extract file before deploy  

Click on Next

**Step 6.Review all stages**
- Click on Create pipeline

**Step 7.Goto S3>Buckets>codepipeline-ap-south-1-xxxxxx>s3-static-website-cp/>SourceArti/>4xRn5Ab**
- This is the Zip file

**Step 8.Goto Buckets>static-website-example>Objects**
- about.html
- index.html

**Step 9.Open Visual Studio and do some changes in index.html**

Run the following commands
```sh 
$ git status
$ git add .
$ git commit -m "changed index and about for cp"
$ git push
```
**Step 10 Go back to Pipeline and see it is activated just now**

**Step 11.Goto S3>Buckets>codepipeline-ap-south-1-xxxxxx>s3-static-website-cp/>SourceArti/>**
- See It is creating source artifacts

**Step 12.Deployment is successful now goto "static-website-example" Bucket>about.html**
- Goto Versions and see deployment files

**Step 13.After Completion of Pipeline refresh the website page and see the recent changes**

### End of lab
