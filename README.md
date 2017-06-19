## WORK IN PROGRESS

# donejs-production-guide
A Complet guide about Running NodeJS Applications in Production on Scale


# Content
1. What is a Application Delivery Controller
2. Use NGINX as Proxy with static file delivery as also Loadbalancer if needed aka Application Delivery Controller
2. Setup systemd unit files for your DoneJS Application and nignx
3. Start and Monitore the Stack via systemd
4. Scale over Multiple Servers.

# Using Docker Containers
1. What is Docker
2. run Nginx with right configuration
3. run Donejs Application with right configuration
4. Monitore.
5. Scale.


# 1 NGINX
First of all why should you always run NGINX or a Other Production Webserver/Proxy/Loadbalancer in front of NodeJS Applications?

- because nodejs applications should by design crash if some unhandled error get thrown and on the internet all kind of unintended Traffic and requests will reach your nodejs application. You can handle all that in your nodejs application it self sure but C is more fast and NGINX has a lot of expirence with high traffic Applications and Sites thats why it Calls it self Application Delivery Controler
