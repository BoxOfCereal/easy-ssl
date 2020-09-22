# Easy SSL for hosting on platforms such as Digital Ocean/OVH/Hetzner etc

This setup can be used for hosting any image (although here I use a machine learning service) with ssl so long as you have a domain.

## Instructions

### Conifg

Inside `docker-compose-production.yml` make sure to change everything inside `<>` to your respective values

```
ml-service:
image: <image-name>
environment:
    VIRTUAL_HOST: <something.com>
    LETSENCRYPT_HOST: <something.com>
    LETSENCRYPT_EMAIL: <someone@something.com>
```

Make sure your image exposes port 80 Here's an example from the (shameless plug) [FastAPI-Fastai2](https://github.com/BoxOfCereal/FastAPI-Fastai2) template repo's Dockerfile

```
FROM fastdotai/fastai:latest

RUN apt-get update && apt-get install -y git python3-dev gcc \
    && rm -rf /var/lib/apt/lists/*

COPY requirements.txt .

RUN pip install --upgrade -r requirements.txt

COPY app app/

RUN ls app/

# RUN python app/server.py

EXPOSE 80

CMD ["python", "app/server.py", "serve"]
```

Inside `limit.conf` you can choose to limit the size of a files that can be uploaded to your service. The default is `client_max_body_size 25m;`

### Running it

Clone the repo onto your VM and use docker-compose up
`git clone https://github.com/BoxOfCereal/easy-ssl.git && docker-compose up -d`
