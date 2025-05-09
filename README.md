# OCP-template-app

This repository contains an OpenShift template to deploy a customizable `index.html` file served by Nginx. The template allows you to quickly deploy an Nginx-based application with a default or custom HTML file, making it ideal for learning or demonstrating OpenShift capabilities.

## Overview

The OpenShift template (`ocp-app.yaml`) includes the following resources:
- **ConfigMap**: Stores the `index.html` content.
- **Deployment**: Deploys an Nginx container to serve the HTML file.
- **Service**: Exposes the Nginx application within the cluster.
- **Route**: Provides external access to the application.

## Features

- Deploys an Nginx-based application with customizable HTML content.
- Configurable resource limits and requests for the Nginx container.
- Supports external access via OpenShift Routes.
- Easily scalable with configurable replica counts.

## Parameters

The template includes the following parameters:

| Parameter         | Description                                   | Default Value                     |
|-------------------|-----------------------------------------------|-----------------------------------|
| `APP_NAME`        | Name of the application                      | `nginx-html`                     |
| `REPLICA_COUNT`   | Number of pod replicas                       | `1`                              |
| `NAMESPACE`       | Namespace (project) to deploy the application| `dev`                            |
| `NGINX_IMAGE`     | Nginx container image to use                 | `nginx:latest`                   |
| `CPU_LIMIT`       | CPU limit for the container                  | `500m`                           |
| `MEMORY_LIMIT`    | Memory limit for the container               | `512Mi`                          |
| `CPU_REQUEST`     | CPU request for the container                | `200m`                           |
| `MEMORY_REQUEST`  | Memory request for the container             | `256Mi`                          |
| `CONTAINER_PORT`  | Port the Nginx container listens on          | `80`                             |
| `SERVICE_PORT`    | Port exposed by the Service                  | `80`                             |
| `HTML_CONTENT`    | Content of the `index.html` file             | Default HTML content (see below) |

### Default `HTML_CONTENT`

```html
<!DOCTYPE html>
<html>
<head>
  <title>Welcome to OpenShift Template application</title>
</head>
<body>
  <h1>Hello, OpenShift learners!</h1>
  <p>This is a simple webpage served by Nginx on OpenShift.</p>
</body>
</html>
```

## Deployment Instructions
Deploy with the Default `index.html`
To deploy the application with the default `index.html` content, run the following command:
```bash
oc process -f ocp-app.yaml -p APP_NAME=my-app -p NAMESPACE=my-project | oc apply -f -
```
Replace `my-app` with your desired application name and `my-project` with your OpenShift project name.

Deploy with a Custom `index.html` File
To deploy the application with a custom `index.html` file, use the following command:
```bash
oc process -f ocp-app.yaml -p APP_NAME=my-app -p NAMESPACE=my-project -p HTML_CONTENT="$(cat path/to/your/index.html)" | oc apply -f -
```
Replace:

- **my-app** with your desired application name.
- **my-project** with your OpenShift project name.
- **path/to/your/index.html** with the path to your local index.html file.

## Accessing the Application
1. After deployment, retrieve the route URL:
```bash
oc get route my-app-route -n my-project -o jsonpath='{.spec.host}'
```
2. Open the URL in your browser to access the application or used curl command.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.