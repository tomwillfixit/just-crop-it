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
