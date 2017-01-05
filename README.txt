###**INSTRUCTION - HOW TO MAKE DOCKERFILE WHICH WILL RUN PHP SCRIPT**

First of all I assume you are using *Linux* and you have already installed *Docker*.

Our further steps will be in the terminal, so by pressing `Ctrl+Alt+T` let's start entering in the command line and commands below:


    mkdir example    #make new directory "example", for convenience
    cd ./example/    #change current working directory to example directory
    touch hello_Docker.php    #create new file
    nano hello_Docker.php    #let's edit it via nano editor, and type save there line below, it's will our script
		
		<?php echo "Hello Docker"."\n"; ?> //this script will just print "Hello Docker"

	nano Dockerfile #create new file and edit it in one line
		#i found tiny container with PHP 7.0 CLI and in this command include it into my new container
		FROM php:7.0-cli
		#this line isn't obligatory, it's include only ,maintainer name and etc    
		MAINTAINER Marat Gainutdinov <m.gaynutdinov@mail.ru>
   		#COPY <src> <dest>, the COPY instruction will copy new files from <src> and add them to them to the container's filesystem at path <dest>
		COPY . /usr/src/myapp   
 		#WORKDIR makes current directory in container /usr/src/myapp
		WORKDIR /usr/src/myapp   
		#execute command in container, it's like you enter: php ./hello_Docker.php
		CMD [ "php", "./hello_Docker.php" ] 
 
	#build our image into the container, -t option to specify our repository and tag it, DON'T FORGET TO PUT DOT IN THE END 
	docker build -t hello-php . 
	#automatically remove the container when it exits, the -t and -i flags allocate a pseudo-tty and keep stdin open even if not attached. This will allow you to use the container like a traditional VM as long as the bash prompt is running.
	docker run --rm -ti hello-php
	#finally our image will print in the terminal "Hello Docker" 

