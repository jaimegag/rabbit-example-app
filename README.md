Installation
============

Inside the app repository, run the following commands:

```bash
  cf push test-app
  cf bind-service test-app my_rabbitmq_service
  cf restage test-app
```

Usage
=====

To write messages to the queue:

```
http://test-app.<YOUR_DOMAIN>/write
```

To read messages from the queue:

```
http://test-app.<YOUR_DOMAIN>/read
```

While the `write` and `read` enpoints will open a websocket, the `store` endpoint
allows to write and read single messages using HTTP POST and GET
requests. For example:

```
curl -XPOST -d 'test' http://test-app.<YOUR-DOMAIN>/store

curl -XGET http://test-app.<YOUR-DOMAIN>/store
```

#### If you have`HAProxy allows HTTPS traffic only` enabled in CloudFoundry, then
- Replace http with https with http in the above mentioned URL's.
- Ensure you have a rule on your LoadBalancer (or HAProxy), which forwards secure TCP from port 4443 to TCP on port 80.

Demonstration
=============

* Open the URLs to read and write in separate browser tabs, side by side.
* You will see messages being sent & received
* Identify the HAProxy that the writing app is connected to from the IP in the bold color coded URI string
* Using BOSH you can `bosh stop rabbitmq-haproxy-partition-default_az_guid <index>`
* Observe the writing application fail and then automatically reconnect and continue sending messages
* You could also `bosh stop rabbitmq-server-partition-default_az_guid <index>` to simulate a RabbitMQ node failing

License
=======

Released under the Apache 2.0 license

(c) 2015, Pivotal Software
