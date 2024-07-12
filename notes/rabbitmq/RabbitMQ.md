## Rabbit MQ

Works on AMQP (Advanced Message Queuing Protocol)

Messaging broker recieves messages fom publishers and route them to consumers.

Messages are published to exchanges (post office or mailbox), Exchnages then distribute the message copies to queues using rule called bindings. Then the broker either delivers it to consumers subscribed to queues, or consumers fetches/pulls messages from queues on demand.

AMQP works on the basis of message acknowedgement. When a message is delivered to the consumer, the consumer notifies theh broker either automatically or as soon as application developer chooses to. A borker will only copletely removes that message form the queue when it recieves the acknowledgement.

In certain situations, if a message cannot be routed, messages may not be routed, message may be returned to publishers or dropped or placed into so called 'dead leeter queue'.

As explained above, exchanges take a message and route them to zero or more queues. The routing algorithm depends upon the exchange types and rules(bindings).

Exchnage Type:
1. Direct Exchnage
2. Fanout Exchange
3. Topic Exchange
4. Headers Exchange

The default exchange is the direct exchange.

1. Direct Exchange: It delivers the message to queues based on routing key. A Queue binds to the exchange with routing key K. When a new message arrives at exchange with routing key R, the exchaneg routes it to the queue if K==R. If multiple mqueue is configured with routing key K, the exchange will route the message to all queues.

2. Fanout Exchange: It delivers message to all the queues bouund to it irrespective of rounting key.

3. Topic Exchange: It route the message to the queuee based on the routing key and the pattern that was used to bind the queue to the exchange.

4. Header Exchange: It routes the messages based on the header attribute ignoring the rounting key.
