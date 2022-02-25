# Playstation 5 Stock Checker

## Motivation
This is a simple project that parses the official Sony Store website with the Playstation 5 product (https://store.sony.com.sg/collections/playstation-consoles/products/playstation-5-digital-bundle), and checks whether the item is Out Of Stock or not.

If the item is no longer "Out Of Stock", the script will perform a POST request to a Telegram Bot, which will then send a message to a designated Chat of your selection.

This allows for a fuss-free and automated process of checking the stock availability without the need to manually visit the website from time to time.

## Development
The code is written as a shell script executed in an alpine image within the Pod. Users will have to use their own Telegram Bot ID and Telegram Chat ID that are specified within `helm/templates/deployment.yaml`

As the script polls the Sony Store website every 3 seconds, to prevent the chance of being blocked by any potential cybersecurity checks, there is a CronJob written in `helm/templates/cronjob.yaml` that restarts the Kubernetes deployment every 5 minutes. This will attempt to reallocate the deployment to one of 3 nodes available, and as each node in Kubernetes has its own external IP address, it relieves the pressure of polling from a single IP address at all times.

## Deployment
The project is deployed with Helm 3, into a Kubernetes Cluster.
