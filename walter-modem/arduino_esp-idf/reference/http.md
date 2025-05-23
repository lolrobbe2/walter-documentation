## Methods Overview

- [httpConfigProfile](#httpconfigprofile)
- [httpSend](#httpsend)
- [httpQuery](#httpquery)
- [httpDidRing](#httpdidring)

## Enums Overview

- [WalterModemHttpSendCmd](#waltermodemhttpsendcmd)
- [WalterModemHttpQueryCmd](#waltermodemhttpquerycmd)
- [WalterModemHttpPostParam](#waltermodemhttppostparam)

---

## Methods

### `httpConfigProfile`

Configure a HTTP profile.

This function will configure a HTTP profile
with parameters such as server name and auth info.
The profile info is stored persistently in the modem, so it is possible
to store connection info once, using an Arduino sketch to prepare all settings,
and later rely on this preconfigured profile in the modem without the need to
set the parameters again in the actual Arduino sketch used in production.

> [!WARNING]
> You first need call tlsConfigProfile.

> [!NOTE]
> **File uploads/downloads** not supported.

#### Example

<!-- tabs:start -->

##### **Arduino**

```cpp
/**
 * @brief HTTP profile
 */
#define HTTP_PROFILE 1

/**
 * @brief TLS profile
 */
#define TLS_PROFILE 1

if(modem.httpConfigProfile(HTTP_PROFILE, "tls13.akamai.io", 443, TLS_PROFILE)) {
    Serial.println("http: Successfully configured the http profile");
} else {
    Serial.print("http: Failed to configure HTTP profile\r\n");
}
```

##### **ESP-IDF**

```cpp
/**
 * @brief HTTP profile
 */
#define HTTP_PROFILE 1

/**
 * @brief TLS profile
 */
#define TLS_PROFILE 1

if(modem.httpConfigProfile(HTTP_PROFILE, "tls13.akamai.io", 443, TLS_PROFILE)) {
    ESP_LOGI("http", "Successfully configured the HTTP profile");
} else {
    ESP_LOGE("http", "Failed to configure HTTP profile");
}
```

<!-- tabs:end -->

#### Params

<!-- tabs:start -->

##### **Arduino**

| Param          | Description                                                                                                           | Default   |
| -------------- | --------------------------------------------------------------------------------------------------------------------- | --------- |
| `profileId`    | HTTP profile id (0, 1 or 2).                                                                                          |           |
| `serverName`   | The server name to connect to.                                                                                        |           |
| `port`         | The port of the server to connect to.                                                                                 | **80**    |
| `tlsProfileId` | If not 0, TLS is used with the given profile (1-6).                                                                   | **0**     |
| `useBasicAuth` | Set true to use basic auth and send username/pw.                                                                      | **false** |
| `authUser`     | Username.                                                                                                             | **""**    |
| `authPass`     | Password.                                                                                                             | **""**    |
| `rsp`          | Pointer to a modem response structure to save the result of the command in. When NULL is given the result is ignored. | **NULL**  |
| `cb`           | Optional callback argument, when not NULL this function will return immediately.                                      | **NULL**  |
| `args`         | Optional argument to pass to the callback.                                                                            | **NULL**  |

##### **ESP-IDF**

| Param          | Description                                                                                                           | Default   |
| -------------- | --------------------------------------------------------------------------------------------------------------------- | --------- |
| `profileId`    | HTTP profile id (0, 1 or 2).                                                                                          |           |
| `serverName`   | The server name to connect to.                                                                                        |           |
| `port`         | The port of the server to connect to.                                                                                 | **80**    |
| `tlsProfileId` | If not 0, TLS is used with the given profile (1-6).                                                                   | **0**     |
| `useBasicAuth` | Set true to use basic auth and send username/pw.                                                                      | **false** |
| `authUser`     | Username.                                                                                                             | **""**    |
| `authPass`     | Password.                                                                                                             | **""**    |
| `rsp`          | Pointer to a modem response structure to save the result of the command in. When NULL is given the result is ignored. | **NULL**  |
| `cb`           | Optional callback argument, when not NULL this function will return immediately.                                      | **NULL**  |
| `args`         | Optional argument to pass to the callback.                                                                            | **NULL**  |

<!-- tabs:end -->

#### Returns

`bool`
True on success, False otherwise.

---

### `httpSend`

Perform a http **POST** or **PUT** request.

No need to first open the connection with the buggy httpConnect command
unless you need TLS + a private key.

#### Example

<!-- tabs:start -->

##### **Arduino**

```cpp
/**
 * @brief The buffer to transmit to the HTTP server.
 */
uint8_t dataBuf[8] = { 0 };

/**
 * @brief Buffer for incoming HTTP response
 */
uint8_t incomingBuf[256] = { 0 };

if(modem.httpSend(HTTP_PROFILE, "/", dataBuf, 8, WALTER_MODEM_HTTP_SEND_CMD_POST, WALTER_MODEM_HTTP_POST_PARAM_OCTET_STREAM, ctbuf, sizeof(ctbuf))) {
    Serial.println("http: query performed");
} else {
    Serial.println("http: query failed");
}
```

##### **ESP-IDF**

```cpp
/**
 * @brief The buffer to transmit to the HTTP server.
 */
uint8_t dataBuf[8] = { 0 };

/**
 * @brief Buffer for incoming HTTP response
 */
uint8_t incomingBuf[256] = { 0 };

if(modem.httpSend(HTTP_PROFILE, "/", dataBuf, 8, WALTER_MODEM_HTTP_SEND_CMD_POST, WALTER_MODEM_HTTP_POST_PARAM_OCTET_STREAM, ctbuf, sizeof(ctbuf))) {
    ESP_LOGI("http", "query performed");
} else {
    ESP_LOGE("http", "query failed");
}
```

<!-- tabs:end -->

#### Params

<!-- tabs:start -->

##### **Arduino**

| Param                | Description                                               | Default                                      |
| -------------------- | --------------------------------------------------------- | -------------------------------------------- |
| `profileId`          | The profile id (0, 1 or 2) of the HTTP context.           |                                              |
| `uri`                | The URI.                                                  |                                              |
| `data`               | Data to be sent to the server.                            |                                              |
| `dataSize`           | Length of the data buffer to be sent to the server.       |                                              |
| `httpSendCmd`        | POST or PUT [command](#waltermodemhttpsendcmd).           | **WALTER_MODEM_HTTP_SEND_CMD_POST**          |
| `httpPostParam`      | [Content type](#waltermodemhttppostparam) *(enum value)*. | **WALTER_MODEM_HTTP_POST_PARAM_UNSPECIFIED** |
| `contentTypeBuf`     | Optional user buffer to store content type header in.     | **NULL**                                     |
| `contentTypeBufSize` | Size of the user buffer, including terminating null byte. | **0**                                        |
| `rsp`                | Response object.                                          | **NULL**                                     |
| `cb`                 | Callback.                                                 | **NULL**                                     |
| `args`               | Callback arguments.                                       | **NULL**                                     |

##### **ESP-IDF**

| Param                | Description                                               | Default                                      |
| -------------------- | --------------------------------------------------------- | -------------------------------------------- |
| `profileId`          | The profile id (0, 1 or 2) of the HTTP context.         |                                              |
| `uri`                | The URI.                                                  |                                              |
| `data`               | Data to be sent to the server.                            |                                              |
| `dataSize`           | Length of the data buffer to be sent to the server.       |                                              |
| `httpSendCmd`        | POST or PUT [command](#waltermodemhttpsendcmd).           | **WALTER_MODEM_HTTP_SEND_CMD_POST**          |
| `httpPostParam`      | [Content type](#waltermodemhttppostparam) *(enum value)*. | **WALTER_MODEM_HTTP_POST_PARAM_UNSPECIFIED** |
| `contentTypeBuf`     | Optional user buffer to store content type header in.     | **NULL**                                     |
| `contentTypeBufSize` | Size of the user buffer, including terminating null byte. | **0**                                        |
| `rsp`                | Response object.                                          | **NULL**                                     |
| `cb`                 | Callback.                                                 | **NULL**                                     |
| `args`               | Callback arguments.                                       | **NULL**                                     |

##### **Micropython**

<!-- tabs:end -->

#### Returns

`bool`
True on success, False otherwise.

---

### `httpQuery`

Perform a http **GET**, **DELETE** or **HEAD** request.

No need to first open the connection with the buggy httpConnect command
unless you need TLS + a private key.

#### Example

<!-- tabs:start -->

##### **Arduino**

```cpp
static char ctbuf[32];

if(modem.httpQuery(HTTP_PROFILE, "/", WALTER_MODEM_HTTP_QUERY_CMD_GET, ctbuf, sizeof(ctbuf))) {
    Serial.println("http: query performed");
} else {
    Serial.println("http query failed");
    ESP.restart();
}
```

##### **ESP-IDF**

```cpp
static char ctbuf[32];

if(modem.httpQuery(HTTP_PROFILE, "/", WALTER_MODEM_HTTP_QUERY_CMD_GET, ctbuf, sizeof(ctbuf))) {
    ESP_LOGI("http", "query performed");
} else {
    ESP_LOGI("http", "query failed");
    esp_restart();
}
```

<!-- tabs:end -->

#### Params

<!-- tabs:start -->

##### **Arduino**

| Param                | Description                                               | Default                             |
| -------------------- | --------------------------------------------------------- | ----------------------------------- |
| `profileId`          | The profile id *(0, 1, or 2)* of the HTTP context.        |                                     |
| `uri`                | The URI.                                                  |                                     |
| `httpQueryCmd`       | GET, DELETE, or HEAD [command](#waltermodemhttpquerycmd). | **WALTER_MODEM_HTTP_QUERY_CMD_GET** |
| `contentTypeBuf`     | Optional user buffer to store content type header in.     | **NULL**                            |
| `contentTypeBufSize` | Size of the user buffer, including terminating null byte. | **0**                               |
| `rsp`                | Response object.                                          | **NULL**                            |
| `cb`                 | Callback.                                                 | **NULL**                            |
| `args`               | Callback arguments.                                       | **NULL**                            |

##### **ESP-IDF**

| Param                | Description                                               | Default                             |
| -------------------- | --------------------------------------------------------- | ----------------------------------- |
| `profileId`          | The profile id *(0, 1, or 2)* of the HTTP context.        |                                     |
| `uri`                | The URI.                                                  |                                     |
| `httpQueryCmd`       | GET, DELETE, or HEAD [command](#waltermodemhttpquerycmd). | **WALTER_MODEM_HTTP_QUERY_CMD_GET** |
| `contentTypeBuf`     | Optional user buffer to store content type header in.     | **NULL**                            |
| `contentTypeBufSize` | Size of the user buffer, including terminating null byte. | **0**                               |
| `rsp`                | Response object.                                          | **NULL**                            |
| `cb`                 | Callback.                                                 | **NULL**                            |
| `args`               | Callback arguments.                                       | **NULL**                            |

<!-- tabs:end -->

#### Returns

`bool`
True on success, False otherwise.

---

### `httpDidRing`

Fetch http response to earlier http request, if any

#### Example

<!-- tabs:start -->

##### **Arduino**

```cpp
/**
 * @brief Buffer for incoming HTTP response
 */
uint8_t incomingBuf[256] = { 0 };

/**
 * @brief How many times we have left to receive response
 */
static short httpReceiveAttemptsLeft = 0;

/**
 * @brief Buffer for Content-Type
 */
static char ctbuf[32];


/* while loop so we can fetch old responses as well as the expected response */
while(modem.httpDidRing(HTTP_PROFILE, incomingBuf, sizeof(incomingBuf), &rsp)) {
    httpReceiveAttemptsLeft = 0;

    Serial.printf("http: status code: %d\r\n", rsp.data.httpResponse.httpStatus);
    Serial.printf("http: content type: %s\r\n", ctbuf);
    Serial.printf("http: [%s]\r\n", incomingBuf);
}
```

##### **ESP-IDF**

```cpp
/**
 * @brief Buffer for incoming HTTP response
 */
uint8_t incomingBuf[256] = { 0 };

/**
 * @brief How many times we have left to receive response
 */
static short httpReceiveAttemptsLeft = 0;

/**
 * @brief Buffer for Content-Type
 */
static char ctbuf[32];


/* while loop so we can fetch old responses as well as the expected response */
while(modem.httpDidRing(HTTP_PROFILE, incomingBuf, sizeof(incomingBuf), &rsp)) {
    httpReceiveAttemptsLeft = 0;

    ESP_LOGI("http", "status code: %d", rsp.data.httpResponse.httpStatus);
    ESP_LOGI("http", "content type: %s", ctbuf);
    ESP_LOGI("http", "[%s]\r\n", incomingBuf);
}
```

<!-- tabs:end -->

#### Params

<!-- tabs:start -->

##### **Arduino**

| Param           | Description                                                                 | Default  |
| --------------- | --------------------------------------------------------------------------- | -------- |
| `profileId`     | Profile for which to get the response.                                      |          |
| `targetBuf`     | User buffer to store response in.                                           |          |
| `targetBufSize` | Size of the user buffer, including space for a terminating null byte.       |          |
| `rsp`           | Pointer to a modem response structure to save the result of the command in. | **NULL** |

##### **ESP-IDF**

| Param           | Description                                                                 | Default  |
| --------------- | --------------------------------------------------------------------------- | -------- |
| `profileId`     | Profile for which to get the response.                                      |          |
| `targetBuf`     | User buffer to store response in.                                           |          |
| `targetBufSize` | Size of the user buffer, including space for a terminating null byte.       |          |
| `rsp`           | Pointer to a modem response structure to save the result of the command in. | **NULL** |

<!-- tabs:end -->

#### Returns

`bool`
True on success, False otherwise.

---

## Enums

### `WalterModemHttpSendCmd`

The possible commands for a HTTP send operation.

<!-- tabs:start -->

#### **Arduino**

> **WALTER_MODEM_HTTP_SEND_CMD_POST** = `0` \
> Perform a POST request \
> **WALTER_MODEM_HTTP_SEND_CMD_PUT** = `1` \
> Perform PUT request

#### **ESP-IDF**

> **WALTER_MODEM_HTTP_SEND_CMD_POST** = `0` \
> Perform a POST request \
> **WALTER_MODEM_HTTP_SEND_CMD_PUT** = `1` \
> Perform PUT request

<!-- tabs:end -->

### `WalterModemHttpQueryCmd`

The possible commands for a HTTP query operation.

<!-- tabs:start -->

#### **Arduino**

> **WALTER_MODEM_HTTP_QUERY_CMD_GET** = `0` \
> Perform a GET request \
> **WALTER_MODEM_HTTP_QUERY_CMD_HEAD** = `1` \
> Perform a HEAD request \
> **WALTER_MODEM_HTTP_QUERY_CMD_DELETE** = `2` \
> Perform a DELETE request

#### **ESP-IDF**

> **WALTER_MODEM_HTTP_QUERY_CMD_GET** = `0` \
> Perform a GET request \
> **WALTER_MODEM_HTTP_QUERY_CMD_HEAD** = `1` \
> Perform a HEAD request \
> **WALTER_MODEM_HTTP_QUERY_CMD_DELETE** = `2` \
> Perform a DELETE request

<!-- tabs:end -->

### `WalterModemHttpPostParam`

The possible post params for a HTTP send operation.

<!-- tabs:start -->

#### **Arduino**

> **WALTER_MODEM_HTTP_POST_PARAM_URL_ENCODED** = `0` \
> Param type is encoded in the url \
> **WALTER_MODEM_HTTP_POST_PARAM_TEXT_PLAIN** = `1` \
> Param is plain text \
> **WALTER_MODEM_HTTP_POST_PARAM_OCTET_STREAM** = `2` \
> Param is an octet/byte stream \
> **WALTER_MODEM_HTTP_POST_PARAM_FORM_DATA** = `3` \
> Param is form data \
> **WALTER_MODEM_HTTP_POST_PARAM_JSON** = `4` \
> Param is JSON \
> **WALTER_MODEM_HTTP_POST_PARAM_UNSPECIFIED** = `99` \
> Param is unspecified

#### **ESP-IDF**

> **WALTER_MODEM_HTTP_POST_PARAM_URL_ENCODED** = `0` \
> Param type is encoded in the url \
> **WALTER_MODEM_HTTP_POST_PARAM_TEXT_PLAIN** = `1` \
> Param is plain text \
> **WALTER_MODEM_HTTP_POST_PARAM_OCTET_STREAM** = `2` \
> Param is an octet/byte stream \
> **WALTER_MODEM_HTTP_POST_PARAM_FORM_DATA** = `3` \
> Param is form data \
> **WALTER_MODEM_HTTP_POST_PARAM_JSON** = `4` \
> Param is JSON \
> **WALTER_MODEM_HTTP_POST_PARAM_UNSPECIFIED** = `99` \
> Param is unspecified

<!-- tabs:end -->
