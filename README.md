# IPstackapp-containerization

This is a simple dockerized web application in flask which provides the geolocation of the given IP address. We make use of redis for caching purpose and the flask application has been converted to a docker image.

## Features

This module will provision 4 containers in total :
- REDIS container : caching (for caching purpose)
- IPSTACK(application) containers : ipstackapp1,ipstackapp2,ipstackapp3

Requirements 

- [Install docker](https://docs.docker.com/engine/install/)
- [Install docker-compose](https://docs.docker.com/compose/install/)
- An [IP stack Login](https://ipstack.com/) and API for for finding the geolocation

## Provisioning

1. Make sure to provide the IPSTACK_KEY (api-key value) in the .env file.
```
IPSTACK_KEY=eb469934dsfgha12fcb500ddc #Enter your api-key here
```
2. You may execute the below commands to 
```
git clone https://github.com/Freeda-F/ipstackapp-containerization.git
cd ipstackapp-containerization
```
```
docker-compose config
```
![image](https://user-images.githubusercontent.com/93197553/147142790-e444d11c-535a-4cf7-98b0-e5e34fd9fa54.png)

```
docker-compose up -d
```
![image](https://user-images.githubusercontent.com/93197553/147143010-e2f19d2c-6124-4d8e-98a0-3f9f78a4463f.png)

```
docker-compose ps
```
![image](https://user-images.githubusercontent.com/93197553/147143134-2c14bf93-e176-49aa-848b-611a9daa771b.png)


Note : This provisioned containers run in 3 different ports in the server i.e 8081,8082,8083. Hence, these containers can be exposed to a load balancer for these to work in an efficient manner.

### Adding Load balancer to the build :

#### - To manually add the load balancer to this build via AWS console, you may follow the below steps.

1. Create a target group for the targets (containers) with the below values.
![image](https://user-images.githubusercontent.com/93197553/147143818-c148af89-880e-4956-9306-fce728a220a8.png)

2. Once the target group has been created, you can add the targets with the required ports i.e 8081,8082 and 8083 using 'Add to registered' option.
![image](https://user-images.githubusercontent.com/93197553/147144414-16fbcb10-6659-4f15-a469-dab868b4d39e.png)

3. Then, you can either choose application LB or network LB to distribute the load among the containers.
  3.1) Provide a name for the load balancer. Also, choose the availability zones required. If you wish to redirect the traffic to HTTPS then make sure to add HTTP and HTTPS listeners.
  ![image](https://user-images.githubusercontent.com/93197553/147144752-2379acc5-6a28-4dd0-ba7d-49b336f18d22.png)
  
  3.2) In the next step, you can upload an SSL certificate if you have added HTTPS listener. If not, you can skip this step. Also, choose a security group for the LB as well.
  ![image](https://user-images.githubusercontent.com/93197553/147145021-b6c09174-c588-466a-9c07-80345ff42252.png)
  
  3.3) You can configure the routing by choosing the target group that we have already created in step 1. If required, you may also configure health checks.
  
  3.4) In the next page, you can view the registered targets from the target gorup in the LB.
 ![image](https://user-images.githubusercontent.com/93197553/147145500-60aec7c8-3cfb-41fb-bb27-1ac79a53b67d.png)

Now, the load balancer has been applied to the docker build.

#### - To automatically provision an application load balancer to this build, please check [dockerization with terraform](https://github.com/Freeda-F/terraform-aws-docker)

## Result

The web application which provides the geolocation is up and running long side with a LB. You can access the web application via a loadbalancer DNS like given below.

Example : http://ipstack-alb-1215678543574.ap-south-1.elb.amazonaws.com/123.4.5.6

![image](https://user-images.githubusercontent.com/93197553/147145664-2ce961e6-4cb7-42c2-9ab2-5b64b38617e5.png)
