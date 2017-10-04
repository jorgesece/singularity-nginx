# Nginx

This is an example of using a Singularity container to run a basic nginx server(port 8080), and serve one or more files. Download the repo if you haven't already:

      git clone https://github.com/jorgesece/singularity-nginx.git
      cd nginx


First, let's talk about how we would run this image:

      sudo singularity create nginx.img
      sudo singularity bootstrap nginx.img Singularity


You will see the image bootstrapping, including downloading of Docker layers, installation of nginx. Now let's run it, and we start a webserver:

     
      ./nginx.img 

You can run it also by using singularity instances:

	sudo singularity instance.start nginx.img nginx_instance
	sudo singularity run nginx_instance

![nginx-basic.png](nginx-basic.png)

Welp, that was easy! 

## How does it work?
How is this working? Let's look at the spec file:


	Bootstrap: docker
	From: nginx:latest

	%runscript
	     
	     exec /usr/sbin/nginx

	%post

	     echo "<h2>Nginx configured in port 8080</h2>"
	     sed -i 's/80/8080/g'  /etc/nginx/conf.d/default.conf
	     chmod -R 777 /var/cache/nginx
	     touch /var/run/nginx.pid
	     chmod 777 /var/run/nginx.pid

### The Header
The First line `bootstrap` says that we are going to bootstrap a `docker` image, specifically using the (`From` field) `nginx:latest`. You couldn't choose another distribution that you like, I just happen to like Debian.

### %post
Post is the section where you put commands you want to run once to create your image. This includes:

- Configure the 8080 port
- Git acces to any user to the nginx files/folders (if you are root you do not need it)


### %runscript
The `%runscript` is the thing executed when we run our container. For this example, we basically start the nginx service. Anything in that folder will then be available to our local machine on port 8080, meaning the address `localhost:8080` or `127.0.0.1:8080`.


## Example Use Cases


	sudo singularity instance.start nginx.img nginx_instance
	sudo singularity run nginx_instance
