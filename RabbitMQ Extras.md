`amqp.Table{}` is just a type alias for a map.

If you look at the source code of the go RabbitMQ library, you'll see it's defined like this:

```go
type Table map[string]interface{}
```

### Why does it exist?

RabbitMQ follows the **AMQP 0-9-1 protocol**. This protocol allows you to send "arguments" (extra configuration) when you create queues or exchanges. These arguments aren't part of the standard fixed parameters (like name or durability); they are optional "extras."

The protocol calls these collections of arguments **"Field Tables."** To make it easy for Go developers, the library creators named the Go type `amqp.Table`.

### How do you use it?

Since it is just a map, you use it to pass **Key-Value pairs** where:

- **The Key** is always a string (the name of the setting).
- **The Value** can be almost anything (string, integer, boolean, etc.).


Example:

```go
	queue, err := connCh.QueueDeclare(
		queueName,
		queueType == Durable,
		queueType == Transient,
		queueType == Transient,
		false,
		amqp.Table{
			"x-dead-letter-exchange": "peril_dlx",
		},
	)
```

If you wanted messages in a queue to automatically expire in 10 seconds:
```go
amqp.Table{
    "x-message-ttl": 10000, // 10,000 milliseconds
}
```


If you wanted queue to every hold 50 messages:
```go
amqp.Table{
	"x-max-length": 50,
}
```

The first few parameters (name, durable, etc.) are **standard** for every queue. The amqp.Table at the end is the **"Everything Else"** bucket.