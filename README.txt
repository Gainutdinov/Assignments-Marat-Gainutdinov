## How to run Vanilla Laravel web application in Docker container ##


**Prerequisites** ubuntu 14.04 or other linux based distributive.

### Docker installation ###
make following commands below for install docker

    $ sudo apt-get update
    $ sudo apt-get install apt-transport-https ca-certificates
    $ sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
    $ echo "deb https://apt.dockerproject.org/repo ubuntu-trusty main" | sudo tee /etc/apt/sources.list.d/docker.list
    $ sudo apt-get update
    $ apt-cache policy docker-engine
    $ sudo usermod -aG docker $USER
Open a terminal on your Ubuntu host.

    $ sudo apt-get update
    $ sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual   
Log into your Ubuntu installation as a user with sudo privileges.

    $ sudo apt-get update
    $ sudo apt-get install docker-engine
    $ sudo service docker start
    $ sudo docker run hello-world

### Containers downloading and seving Laravel###

    $ docker pull dylanlindgren/docker-laravel-data 
    $ docker pull dylanlindgren/docker-laravel-composer 
    $ docker pull dylanlindgren/docker-laravel-artisan   
    $ docker pull dylanlindgren/docker-laravel-phpfpm  
    $ docker pull dylanlindgren/docker-laravel-nginx 
    $ docker pull dylanlindgren/docker-laravel-bower

Let's create directories where we will store our own website
    
    $ mkdir  ~/myapp
    $ mkdir  ~/myapp/www
    $ mkdir  ~/myapp/logs   
    $ docker run --restart always --name myapp-data -v /home/marat/myapp:/data:rw dylanlindgren/docker-laravel-data 
    $ docker run --privileged=true --volumes-from myapp-data --rm dylanlindgren/docker-laravel-composer myapp-composer create-project laravel/laravel /data/www --prefer-dist

Running artisan commands

    $ docker run --privileged=true --volumes-from myapp-data --rm dylanlindgren/docker-laravel-artisan
    
**Serving Laravel**
    
    $ docker run --restart always --privileged=true --name myapp-php --volumes-from myapp-data -d dylanlindgren/docker-laravel-phpfpm 
    $ docker run --restart always --privileged=true --name myapp-web --volumes-from myapp-data -p 80:80 --link myapp-php:fpm -d dylanlindgren/docker-laravel-nginx  
    $ sudo chmod -R 777 /home/marat/www/

And finally if we open our web browser and go to http://localhost we will see our Laravel welcome page! 
BUT wait a minute let's change default laravel page into to our own website.

Download from current repo our My_website.rar archive.

[https://github.com/Gainutdinov/Assignments-Marat-Gainutdinov/tree/3rd-week](https://github.com/Gainutdinov/Assignments-Marat-Gainutdinov/tree/3rd-week "https://github.com/Gainutdinov/Assignments-Marat-Gainutdinov/tree/3rd-week")

and let's replace HTML code of *welcome.blade.php* (location /home/marat/myapp/www/resources/views/welcome.blade.php) with our html code from My_website.rar -> *website2.html*
also, let's copy our images and videos into the *public* directory (location is /home/marat/myapp/public).


Finally let's reopen our browser and type http://localhost and we will see our own website.