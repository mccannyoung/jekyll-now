---
layout: post
title: Blog set up complete
---

All I gotta say is that this set up is very easy! I look forward to playing with the markup.

Javascript test:
```javascript
/* Some pointless Javascript */
var string = "Hello World;
```

C# test:
```csharp
/* Comment 1 */
// Comment 2
var greeting = "Hello World;
```

golang test:
```golang
// SimpleSender sends a submitsm pdu request
func SimpleSender(message2send JsonMTRequest, tx *smpp.Transmitter) int {
	log.Println("in the simple sender")

	sender := message2send.From  //c.Args()[0]
	recipient := message2send.To //c.Args()[1]
	text := message2send.Content //strings.Join(c.Args()[2:], " ")
```
python test:
```python
    def send_sms(self, ampq_request):
        """ this function is used to send the request on its way
        """
        if len(ampq_request["to"]) == 10:
            ampq_request["to"] = "1"+ ampq_request["to"]
```

Also, semi-related to my samples, SMS sending is taking over my life, and not in the same way it's happening for adolescents.
