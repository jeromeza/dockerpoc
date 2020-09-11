Giving devs the power to deploy their own sites - automagikally!

Devs before automated site deployments:  
![Semantic description of image](https://github.com/jeromeza/dockerpoc/blob/master/skeletor.gif "ARGGHHHHHH")

Devs after automated site deployments:  
![Semantic description of image](https://github.com/jeromeza/dockerpoc/blob/master/heman.gif "I HAVE THE POWEEEERR!")

# The project does the following:
- Deploys a Docker container on your GitLab runner
- Uses the Docker container to:  
  - Create a nginx-proxy Docker network.  
  - Create another Docker container - nginx-proxy which is bound to this network. 
  - Create another Docker container - blog.example.com which is also bound to this network and served by the nginx-proxy container. 
  - Pull down Git. 
  - Clone a website repo (blog.example.com). 
  - Copy the repo to the new blog.example.com Docker container. 
  - Enable .htaccess style rewrite rules for blog.example.com Docker container. 
  - Resart Apache on blog.example.com Docker container.   
    
The smart bits here are that the nginx-proxy is bound to the docker sock AND as such it can read docker events. Which means it can detect if any containers are started with a specific VAR. In this case VIRTUAL_HOST - which we alias to $FQDN. Once it detects this VAR it creates the relevant Nginx proxy config and serves the site, without any manual intervention / the need to adjust any Nginx based config.

Some more reading on this can be found here:  
http://jasonwilder.com/blog/2014/03/25/automated-nginx-reverse-proxy-for-docker/

### GitLab CI Pipeline needs to execute this flow, by making us of the .gitlab-ci.yml provided. The Following VARS need to be specified:  
  
$FQDN:  
blog.example.com  
  
$IMAGE:  
webdevops/php-apache:5.6  
webdevops/php-apache:7.0  
webdevops/php-apache:7.1  
webdevops/php-apache:7.2  
webdevops/php-apache:7.3  
webdevops/php-apache:7.4  
  
$REPO  
https://github.com/jeromeza/blog.example.com.git
