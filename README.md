# IAXPing.js
Simple node module to send POKE requests to an Asterisk server, following RFC 5456 (IAX2 protocol).

## Usage

```javascript
const IAXPing = require('iaxping');

IAXPing({
	host: 'pbx.litenet.tel',
	port: 4569, // Optional, default is 4569
	timeout: 2000 // Optional, default is 2000ms
}).then((response) => {
	console.log('Response received:', response);
}).catch((error) => {
	console.error('Error:', error);
});
```

Example responses:
```
// Successful response
{
  remoteAddress: '163.123.239.31',
  remotePort: 4569,
  remote: '163.123.239.31:4569',
  rttMs: 309,
  sourceCall: 24900,
  remoteCall: 1
}
```

Error responses:
- Note: Errors are thrown as exceptions, use .catch() to handle them.

Timed out:
```
{
	code: 'ERR_POKE_TIMEOUT',
	message: 'Timeout waiting for PONG after 2000 ms'
}
```

Socket error:
```
{
	code: 'ERR_SOCKET',
	message: 'Socket error while waiting for PONG'
}
```

Failed to send POKE:
```
{
	code: 'ERR_SEND_POKE',
	message: 'Failed to send POKE frame'
}
```

Invalid arguments:
```
{
	code: 'ERR_INVALID_HOST',    // host is required and must be a non-empty string
	code: 'ERR_INVALID_PORT',    // port must be a valid UDP port (1-65535)
	code: 'ERR_INVALID_TIMEOUT'  // timeoutMs must be a positive integer in milliseconds
}
```

DNS Resolution error:
```
{
	code: "ENOTFOUND",
	message: "getaddrinfo ENOTFOUND invalidhost"
}

Quirks:
- The RFC states `it MUST be sent when there
   is no existing call to the remote endpoint`, but it still seems to work fine when there is an active call.

- Make sure your software is able to make outbound UDP connections, otherwise it just straight up won't work.