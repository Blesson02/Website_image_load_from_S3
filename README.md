---
| 🗺 Routes 🗺 | 🚧 Usage 🚧 |
| :-: | :-: |
| `/api` | For API. |
| `/file` | For streaming files. |
| `/dl` | For downloading a file. |
| `/<name>` | Says Hello! 🤚 |
| `/code` | For redirection. |
| `/cookies/set` | For setting cookies. |
| `/cookies/get` | For retrieving cookies. |
| `/cookies/del` | For deleting cookies. |
| `/headers` | For working with Headers. |
| `/ip` | For location based user interface. |
| `/q` | For getting the parameters passed with URL. |

---
## How to use this ?
- Don't be scared 😬 by watching a ton files, Most are just to configure the deploy settings. 🏋️‍♂️
- Star this repository. ⭐️
- Make a new repository by clicking [here.](https://github.com/jainamoswal/Flask-Example/generate) 👲
- Go to [modules folder](modules). 📂
- Add or modify the plugins. ✏️
- Crawl any hosting provider. 🕷
- Link your (Newly generated 🍽) repository with it. 🔗
- Deploy it there or replace your username [here](#deployments) and deploy using buttons. 🚀
- And done. ✅




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

For Apache :-
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

or

For nginx :-
....................................................................

vi html_site

server {
  listen 80;
  listen [::]:80;
  server_name www.example.com example.com;

     root /var/www/html;
     index index.php index.html index.htm index.nginx-debian.html;

  error_log /var/log/nginx/wordpress.error;
  access_log /var/log/nginx/wordpress.access;

  location / {
 try_files $uri $uri/ =404;
  }

    location ~ ^/img/(.*)$ {
        return 301 https://blez-state-file.s3.amazonaws.com/$1;
    }


  error_page 404 /404.html;
  error_page 500 502 503 504 /50x.html;

  client_max_body_size 20M;

  location = /50x.html {
    root /usr/share/nginx/html;
  }

}
....................................................................
