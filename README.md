# Website_image_loaf_from_S3
#In this Doc we can see an HTML website's images are loading from S3 bucket

Step1 : Host website on a linux server.
For example its Document root is "/var/www/html" and all website images residing on location "/var/www/html/img".

Step2 : Create an S3 bucket and upload images to this bucket 
aws s3 cp /var/www/html/img/*  s3://bucket_name  --profile=new

Step3 : Give propper bucket policy. (This will provide access to all users ) and also enable public access to the Bucket.

....................................................................
{
    "Version": "2012-10-17",
    "Id": "Policy1689700532511",
    "Statement": [
        {
            "Sid": "Stmt1689700525280",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::bucket_name/*"
        }
    ]
}
....................................................................


Step4 : Last but not least give propper rewrite rule from img directory to S3 bucket URL.

....................................................................
vi html_site.conf

<VirtualHost *:80>
servername example.com
DocumentRoot /var/www/html

RedirectMatch 301 ^/img/(.*)$ https://blez-state-file.s3.amazonaws.com/$1

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

....................................................................
