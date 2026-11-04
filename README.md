# CST8915 — Lab 5 | Algonquin Pet Store

For this lab, we worked with the Pet Store application provided by our professor, focusing on containerizing its services and running them together using Docker Compose.

We chose the Python implementation of the Product Service, alongside the Node.js Order Service and Vue frontend. The frontend is built using Node.js and served through Nginx, while RabbitMQ handles the order queue.

## Source Code & Published Images

We forked the provided repositories, added Dockerfiles for each service, and published the resulting images to Docker Hub.

| Component | Source Code | Docker Image |
|---|---|---|
| Orders (Node.js) | [GitHub](https://github.com/onurozko/order-service-L4) | [Docker Hub](https://hub.docker.com/r/tonurozkoral/order-service) |
| Products (Python) | [GitHub](https://github.com/onurozko/product-service-python-L4) | [Docker Hub](https://hub.docker.com/r/tonurozkoral/product-service) |
| Frontend (Vue + Nginx) | [GitHub](https://github.com/onurozko/store-front-L4) | [Docker Hub](https://hub.docker.com/r/tonurozkoral/store-front) |

For RabbitMQ, we used the official `rabbitmq:4.3.6-management` image.

## Getting It Running

Our final Compose file references the images published to Docker Hub, so there's no need to build the application locally.

First, copy `.env.example` to `.env` and configure the RabbitMQ credentials.

```powershell
docker compose pull
docker compose up -d --wait
```

Once the containers are running, the application is available at **http://localhost:8080**.

The frontend communicates with the Order Service on port `3000` and the Product Service on port `3030`. The product list is also available directly at `http://localhost:3030/products`.

RabbitMQ Management can be accessed at `http://localhost:15672`, using the credentials configured in `.env`.

To check the containers or stop the application:

```powershell
docker compose ps
docker compose down
```

## Verification

We first built the three application images locally and tested the services before bringing everything together using Docker Compose.

After starting the full application, we placed two orders through the frontend and confirmed that both messages appeared in RabbitMQ's `order_queue`.

We then uploaded our images to Docker Hub and switched to a Compose configuration that pulls the published images instead of building locally. All four containers started successfully, and the application continued working as expected.

## A Few Notes

I've used Docker quite a bit in my previous workplaces, mainly for local development and running multi-service applications. Most projects already had their Docker configurations in place, so my day-to-day work usually involved commands like `docker compose build` and `docker compose up`.

What was relatively new to me in this lab was working with the image distribution side of Docker, especially tagging images, publishing them to Docker Hub, and then pulling those images to run the application independently of the source code.

I also found the multi-stage build for the Vue frontend interesting. It's a useful way to keep the final image smaller by separating the build environment from the environment actually serving the application.

Overall, the lab was a good opportunity to explore parts of the Docker workflow that I hadn't needed to work with much professionally, while building on my existing experience.

## Demo

[YouTube](https://youtu.be/2Aje1DtvwUM)
