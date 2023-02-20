sudo docker run --name some-nginx -v /home/ubuntu/panda/public:/usr/share/nginx/html:ro -d -p 80:80 nginx



docker run --name some-nginx -v /Users/yun/GolandProjects/pandariel/public:/usr/share/nginx/html:ro -d -p 8090:80 nginx


ssh -i /Users/yun/Desktop/korea.pem lighthouse@43.155.158.119


scp -i /Users/yun/Desktop/korea.pem -r ./public ubuntu@43.155.158.119:/home/lighthouse/panda  


ssh  ubuntu@43.155.158.119

scp -r ./public  ubuntu@43.155.158.119:/home/ubuntu/panda

hugo server --minify --theme hugo-book
hugo --minify --theme hugo-book
