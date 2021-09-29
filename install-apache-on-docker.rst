======================================================
Setting up an Apache Container on Docker with CentOS 8
======================================================

Prerequisites:
~~~~~~~~~~~~~~ 
**Install Docker**

We have already covered this step in our previous article. If you do not have Docker setup, please consult the following article: `Here <https://blog.6servers.com/2021/09/23/how-to-install-docker-on-centos-8/>`_

**Add Apache Ports to Firewall D**

You can do this by adding port 80 as seen here:

``firewall-cmd --permanent --zone=public --add-port=80/tcp --permanent``

Or you can add http as a service: 

``firewall-cmd --permanent --zone= --add-service=http --permanent``

After adding Apache you will need to restart FirewallD, you can do this by running:

``firewall-cmd --reload``


Step 1: Pull an Apache Image from Docker
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Docker contains many different standard containers including Apache. In order set up an Apache container in Docker, we are going to pull an image from Docker Hub that we can use locally. For more information on the specific container that we will be using consult: `Docker Hub <https://hub.docker.com/_/httpd/>`_

``docker pull httpd``


<photo-docker-pull-httpd>


You can then run docker image list to get a list of your most recently installed images, here you will see that httpd (Apache) should be added to your list from were we initiated a pull above. 


<photo-docker-image-list>


Step 2: Run a detached container image of Apache 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After we have successfully pulled a Docker image to use locally, the next step is to run the Docker container. Note, it is highly recommended to use the following flags with this command:

``docker run -it -d -p 80:80 --name six_apache httpd``

<photo-docker-run-container-httpd>

The ``-i`` flag means that the mode is interactive, ie. you can enter commands into it 

The ``-t`` flag includes a terminal, this allows you the same ability as if you ssh'ed into the container 

The ``-d`` flag detaches, this allows you to run the container in the background 

The ``-p`` flag specifies the port, in this particular case we want to make sure our container port forwards to the same port on our server. The first 80 specifies your server port, the second 80 belongs to the apache container.

six_apache is the name that we assigned to the container for this example, you can keep it blank if you desire and Docker will assign a random name to the container or assign your own name.


Step 3: Validating your results
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If your container is working properly, going to your website should display an "It Works!" page. By default that is what is included within your Apache container within Docker. We will dive further on how to manipulate and modify this container to run specific web pages in the next article.



**Solving common problems:**

This is a rather uncommon issue, but during testing this error was found 

``Error response from daemon: driver failed programming external connectivity on endpoint <container_name> (<container_id_number):  (iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 80 -j DNAT --to-destination 127.0.0.1:80 ! -i docker0: iptables: No chain/target/match by that name.``


If the following error occurs, you will need to flush your IP tables:

``sudo iptables -t filter -F``

``sudo iptables -t filter -X``


Congratulations on running your apache container within Docker, hopefully you have gotten alot of value from this guide. If you have any questions or comments, feel free to reach out to me on LinkedIn. 
