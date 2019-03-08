# just-crop-it using OpenFaaS

Based on a chat with @egsy :)

Happy International Womens Day!

# Install OpenFaaS

Installed OpenFaaS locally. Instructions here : https://github.com/openfaas/faas

Cloned https://github.com/egsy/just-crop-it.git

## Created the OpenFaaS framework for just-crop-it
```
faas new --lang dockerfile just-crop-it --prefix egsy
```

This creates a .yml file and a directory called just-crop-it.

# Make the code Serverless

Copy the copy.py file and an example .jpg into just-crop-it directory.

Edit just-crop-it/Dockerfile to install python3 py3-requests and copies in both the copy.py script and example .jpg.

Dockerfile example :
```
FROM alpine:3.8

RUN mkdir -p /home/app

RUN apk --no-cache add curl python3 py3-requests \
    && echo "Pulling watchdog binary from Github." \
    && curl -sSL https://github.com/openfaas/faas/releases/download/0.9.14/fwatchdog > /usr/bin/fwatchdog \
    && chmod +x /usr/bin/fwatchdog \
    && cp /usr/bin/fwatchdog /home/app \
    && apk del curl --no-cache

# Add non root user
RUN addgroup -S app && adduser app -S -G app

RUN chown app /home/app

WORKDIR /home/app

USER app

COPY original/IWD2019.jpg /home/app/original/IWD2019.jpg

ADD crop.py /home/app/crop.py

# Populate example here - i.e. "cat", "sha512sum" or "node index.js"
ENV fprocess="xargs python3 /home/app/crop.py"

# Set to true to see request in function logs
ENV write_debug="false"

EXPOSE 8080

HEALTHCHECK --interval=3s CMD [ -e /tmp/.lock ] || exit 1
CMD [ "fwatchdog" ]

```

## Build the image
```
faas build -f just-crop-it.yml 
```

## Deploy
```
faas deploy -f just-crop-it.yml 
```

## Trigger the copy serverless function
```
echo -n "" | faas invoke just-crop-it
```
