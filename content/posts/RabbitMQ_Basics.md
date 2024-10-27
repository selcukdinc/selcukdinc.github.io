+++
title = 'RabbitMQ_Basics'
date = 2024-10-24T18:08:44+03:00
draft = false
+++
### Publish / Subscribe Based Topics
Exchanges > ex.events >  Bindings 
To | Routing key
--|--
q.events.client1 | *.sport.\*
q.events.client2 | *.sport.#
q.events.client2 | *.weather.london.\*


` ` | client#1 | client#2
--|--|--
event.sport             |❌ |✅ 
event.sport.rugby.news  |❌ |✅ 
event.sport.footbal     |✅ |✅ 
event.sport.today-news  |✅ |✅ 

### Fanout Exchange
Without route key publish message to queues

### Direct Exchange
must be declared route key to publish messages (news type)