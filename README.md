# webpush-go


Web Push API Encryption with VAPID support.

```bash
go get -u github.com/sehogas/webpush-go
```

## Example

For a full example, refer to the code in the [example](example/) directory.

```go
package main

import (
	"encoding/json"

	webpush "github.com/sehogas/webpush-go"
)

const URL_ICON_APP = "https://......../icon-512x512.png"

func main() {
	// Decode subscription
	s := &webpush.Subscription{}
	json.Unmarshal([]byte("<YOUR_SUBSCRIPTION>"), s)

	payload := struct {
				notification struct { title, body, icon string }
			}{ notification: { "Hello", "Test message", URL_ICON_APP }}
	}

	payloadJSON, err := json.Marshal(payload)
	if err != nil {
		// TODO: Handle error
	}
	
	// Send Notification
	resp, err := webpush.SendNotification([]byte(payloadJSON), s, &webpush.Options{
		Subscriber:      "example@example.com",
		VAPIDPublicKey:  "<YOUR_VAPID_PUBLIC_KEY>",
		VAPIDPrivateKey: "<YOUR_VAPID_PRIVATE_KEY>",
		TTL:             30,
	})
	if err != nil {
		// TODO: Handle error
	}
	defer resp.Body.Close()
}
```

### Generating VAPID Keys

Use the helper method `GenerateVAPIDKeys` to generate the VAPID key pair.

```golang
privateKey, publicKey, err := webpush.GenerateVAPIDKeys()
if err != nil {
	// TODO: Handle error
}
```

## Development

1. Install [Go 1.17+](https://golang.org/)
2. `go mod vendor`
3. `go test`

#### For other language implementations visit:

[WebPush Libs](https://github.com/web-push-libs)
