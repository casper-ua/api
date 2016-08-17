# API documentation

## Overview

We make use of several different services:

### A HTTP API at https://api_server:port.

This is a HTTP API that accepts and returns JSON. Every call uses POST.
NB: For the test period HTTPS is not(!) supported.

### RTMP streaming to Media Server.

The client video is streamed to EC2 instances running [RTMP](http://en.wikipedia.org/wiki/Real_Time_Messaging_Protocol) Media Server. After transcoding it could be re-streamed to different services such as Youtube or Facebook.

## HTTP API methods

### POST https://api_server:port/api/v1/login?ver=1.0

#### Request Body

```
{
    "client_id": "<application_key>",
    "client_secret": "<application_secret>",
}
```

#### Response Body

```
{
    "token": "<a_value_used_for_authenticating_further_requests>",
    "server": "<a_server_to_connect_to>",
    "client": "<remote_ip>"
}
```

### POST https://api_server:port/api/v1/connect?ver=1.0

#### Request Body

```
{
    "action": "on_connect",
    ...
}
```

#### Response Body (not JSON)

```
0 - on success
1 - on fail
```

## RTMP Streaming

The `token` from the `login` call should be included in the RTMP
application name. The URL format is then:

```
rtmp://#{host}:#{port}/live2?token=#{token}/#{stream_name}" for Youtube
rtmp://#{host}:#{port}/rtmp?token=#{token}/#{stream_name}" for Facebook
```

FFMPEG (or another other RTMP-capable client) can then be used to stream video.
