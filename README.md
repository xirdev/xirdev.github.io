# V3.0 API Introduction

Xirsys 3.0 introduces a completely rearchitected Xirsys platform, based around Docker! The new platform is a general purpose cloud application server \(with data services\) which happens to do TURN. Please see the Platform docs for more Enterprise info.

Through an extensible REST API Xirsys 3.0 provides custom analytics and data storage for customers. Xirsys 3.0 still supports the V2 API.

# Gateways

Xirsys is now available in the following regions:

| Region | Gateway | Location |
| --- | --- | --- |
| US West | ws.xirsys.com | San Jose, CA |
| US East | us.xirsys.com | Washington, DC |
| Europe | es.xirsys.com | Amsterdam |
| Asia | ss.xirsys.com | Singapore |
| Australia | ms.xirsys.com | Melbourne |

as demand grows more data centers will be added. Each of these data centers contains a Xirsys cluster. All of these endpoints are equivalent and host the entire Xirsys REST API.

# Service Overview

Xirsys is based around a dynamically expanding set of services. Basic services include; accounts, namespace, data, stats and . Extended services include; turn and video processing, with more to come.

Each service has a service prefix which must be used in HTTP requests:

| Service | Prefix | Descrpition |
| :--- | :--- | :--- |
| accounts | \_acc | You may manage sub accounts/logins. |
| namespace | \_ns | Create a publicly available tree. |
| turn | \_turn | Get ICE strings here! |
| host | \_host | Find best loaded hosts. |
| data | \_data | Store custom data. |
| stats | \_stats | Get turn/stun stats,  store custom stats, use the Xirsys Grafana driver. |
| subscription | \_subs | Boot/List subscriptions \(chatters\) |
| token | \_token | Get secure/expiring tokens for your users to connect to our services. |
| discovery | \_ds | query any of the services above to find the existing paths/keys. |

A service provides some entity which is identified by path and key, .e.g:

* The account service recognizes "/\_acc/accounts?k=myident"

* The subscriptions service recognizes "/\_subs/domain/app/room?k=chatter"

The API is "soft" in that a new service may be added which may interact with other data in the system - typically for the same path.

## The Shape of the Service API

Using the Xirsys HTTP API, you can GET/PUT/POST/DELETE to any service using the following syntax \(depending on applicability\). The basic shape of all the commands is the same, and references to keys, layers are consistent, there are however parameter variations when required.

Note, your username and secret are required. All your data resides in it's own database \(bucket\) within the Xirsys system.

## GET

A value for a specific key ...

```
curl "https://user:secret@endpoint/_servicename/my/path?k=key"
```

All keys at node

```
curl "https://user:secret@endpoint/_servicename/my/path"
```

## POST/PUT

```
curl -XPOST -H 'Content-type: application/json' "https://user:secret@endpoint/_servicename/my/path" -d '{"k": "mykey","v": "myvalue"}'
```

You typically PUT to create new entities, and POST to update, e.g. you may create and update accounts respectively.

## DELETE

Delete a key.

```
curl -XDELETE "https://user:secret@endpoint/_servicename/my/path?k=key"
```

Delete all keys at path.

```
curl -XDELETE "https://user:secret@endpoint/_servicename/my/path"
```

**Note, that the prefix \(\_servicename\) used in the API is the layer that the data resides in.**

Again this is the general shape of the API, now please check the specifics for each service.

