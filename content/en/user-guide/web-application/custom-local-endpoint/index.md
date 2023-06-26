---
title: "Custom local endpoint"
linkTitle: "Custom local endpoint"
weight: 6
description: >
  Overview of the configurable custom local endpoint for the LocalStack Web Application
aliases:
  - /localstack/custom-local-endpoint/
  - /user-guide/web-application/custom-local-endpoint/custom-local-endpoint/
---

You can configure the local endpoint URL under which LocalStack is accessible from the LocalStack Web UI. To configure your local endpoint URL, navigate to your [Account settings](https://app.localstack.cloud/account/settings), to configure the local endpoint URL (defaults to https://localhost.localstack.cloud). Additionally, you can configure the local endpoint URL to view your resources if you are running LocalStack on a different machine (see instructions below).

## Connecting to a LocalStack instance on a different machine

To ensure that the Web user interface can connect with your running LocalStack instance, you would need to configure the endpoint URL so that the server's SSL certificate matches the hostname/IP address of the endpoint URL. This situation arises when users configure the endpoint URL to be something like `https://myhost:4566` or use an IP address like `https://1.2.3.4:4566`. Websites with an `https://...` URL can only request other endpoints using HTTPS (i.e., not on `http://`). Additionally, while requesting an HTTPS page, the SSL certificate must match the hostname (i.e., `localhost.localstack.cloud` in our case).

To navigate this, we recommend you create a local TCP proxy server. The proxy server could listen on `127.0.0.1:4566` and forward all requests to your target endpoint where the LocalStack instance is running. You could leave the configuration in the Web user interface to use the default value, `https://localhost.localstack.cloud:4566`. We recommend [simpleproxy](https://manpages.ubuntu.com/manpages/trusty/man1/simpleproxy.1.html) or [proxy.py](https://github.com/abhinavsingh/proxy.py) as a way to implement this.

An alternate workaround would be to re-point `localhost.localstack.cloud` to the IP address of your target machine by adding an entry to `/etc/hosts`. This is especially useful if you're accessing the LocalStack Web UI on a macOS/Linux-based machine.

## Troubleshooting

After successfully deploying LocalStack and mapping it to the internet at `https://myhost:4566`, you should be able to access the LocalStack Web Application and other exotic features, such as Resource Browsers, on your main machine without any problems. However if you are unable to access the LocalStack Web Application, start by checking the **Networking tab** of your browser's developer console to gather more information about the connection issue.

It's possible that your browser is preventing the connection between HTTPS pages and non-encrypted HTTP endpoints. To overcome this, try configuring `https://192.168.9.88` as the target endpoint in the profile settings on `app.localstack.cloud`. Keep in mind that doing so may result in SSL certificate validation problems. To handle this, open the provided URL in your browser and manually accept the certificate.

If this approach still doesn't work, another option is to configure your operating system's host file. By adding an entry to resolve the domain name `localhost.localstack.cloud` to your remote IP address (`192.168.9.88`), you may be able to establish the desired connection.