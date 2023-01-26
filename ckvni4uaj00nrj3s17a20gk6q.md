# Simple queues system in Nodejs 🚀

Modern applications grow to require expensive processing, asynchronous operations, necessary tasks delays before completions, logging, and triggering processes from initiated application/user events. 


## Why queue system?
A queue system would thus,  efficiently cater to the increasing asynchronous operations from an increase of actions/events. thus reducing errors, improving user experience, and processing actions in the background, and executing functions or triggering an application service when it's ready.

Some tasks that can be handled by a job queue:
- Sending system-authored push notifications
- Sending system-authored emails
- communication with third applications and webhooks 
- event-driven programming 
- logging events, e.t.c 

Necessary terminology in the context of queues;

**Job** - This is a task that can get pushed into a queue system, to be processed at a later stage in the application flow. the job is the Object that contains the required data for the consumer to process.

**Consumer** - 
This is the task to be carried out in the queue process. such as sending emails, push notifications e.t.c

**Producer** - The producer would represent the instantiated queue constructor. Many Jobs can be added to the specified named queue object; for processing to be carried out. such as push notifications, notifications, emails. 
The options argument will have options such as delay (when to execute the job from the time of creation), attempts (number of retries when a job fails), rate-limiting of jobs e.t.c.

## Let's get started with a simple queue system implementation with [**Bull**](https://www.npmjs.com/package/bull#documentation)

![1*wk16U_GrKwWacETRhu9ScA.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636183504844/4BqKBUMPx.png)

Bull sells its queue system implementation as 
> 
The fastest, most reliable, Redis-based queue for Node. 
Carefully written for rock-solid stability and atomicity.

Bull is fully is compatible with typescript, well tested, queue system UI, and built on top of Redis cache with a lot of helpful abstraction methods and functionalities. 

Of course, the community could use more abstracted systems to simplify solutions when using the plethora of tools with their surging complexities 😅. 

### Implementation
- Install [Redis](https://redis.io/topics/quickstart) locally or set up a Redis docker container as below; let's add a ``Dockerfile`` and a ``docker-compose.yml``:

```
# docker-compose.yaml

# pull a light-weight redis image from docker
# open a reverse port 
services:
 simple_queue_redis:
    image: redis:6.2-alpine
    container_name: simple_queue_redis
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
    ports:
      - 6379:6379
    volumes:
      - ./redis:/db

```


```
 # docker-compose.yaml
FROM node:14-alpine

# Create app directory
WORKDIR /app

# Install dependencies
RUN yarn install
RUN yarn

# Copy source files from the host computer to the container
COPY . .

# Run the app
CMD ["yarn", "start:dev"]

```
- Install the Bull package from NPM using the following command.

`` npm install bull --save``

- Create queues and processes.
We assume that we're working in a type-safe environment, hence the use of typescript.
Create a new file named ``notificationQueue.ts`` and paste the following code.
Here we assume we're working on a notification job.

The `NotificationQueue` instance, in this case, would represent the producer and the notification job gets added to the consumer, the `notificationProcess`.

```
  // ./queues/notificationQueue.ts

import Queue from 'bull';

import { notificationProcess } from '../processes';

// producer
const NotificationQueue = new Queue('Notification', process.env.REDIS_URL);

// consumer
NotificationQueue.process(notificationProcess);

export const addNewNotification = (data: {
title: string,
body: string,
url: string,
}
) => {
  NotificationQueue.add(data);
};

export default NotificationQueue;

```

The following code block defines the implementation of the push notification job from the server.

We're working with [pusher beams](https://pusher.com/docs/beams/getting-started/web/sdk-integration/) for sending push messages from the server. it's easy to use, has good documentation, and helps abstract the complexities of socketing a server to allow messaging between the server and client 😀. 

Install the pusher Nodejs SDK from NPM using the following command.

``  npm i @pusher/push-notifications-server ``

We won't be focusing on how pusher works in this tutorial, the official [docs](https://github.com/pusher/push-notifications-node#readme) is a good resource.

```
  // ./processes/notification.process.ts

import { Job } from 'bull';
import PushNotifications from '@pusher/push-notifications-server';

const beamsClient = new PushNotifications({
  instanceId: process.env.PUSHER_BEAMS_INSTANCE,
  secretKey: process.env.PUSHER_BEAMS_SECRET,
});

const NotificationProcess = async (job: Job) => {
  const {
    title,
    body,
    url,
   
  } = job.data as {
title: string,
body: string,
url: string,
};

  await beamsClient.publishToInterests([userId], {
    web: {
      notification: {
        title,
        body,
        deep_link: `${process.env.BASE_URL}${url}`,
      },
    },

  });
 return true;
}

export default NotificationProcess;
```
Save the file and run it using node ``./index.ts``. A notification will be sent to the subscribed client. (not covered in this tutorial).

```
// ./index.ts
import { NotificationQueueHelpers, } from '.../queues';

...

 NotificationQueueHelpers.addNewNotification({
    title: "simple queue system",
    body: "push notification sent...",
    url: `/notification/${user.id}`,
  });
```
### Wrap-up

This is a simple example of a queue system using Bull to create queues and schedule Jobs. As mentioned above bull offers a lot more options to manage the Redis caching system behind the scenes such as Event listeners, Rate limiters, Job types, delayed jobs, number of retries, using Cron expressions, and etc.
 
The [**official documentation**](https://optimalbits.github.io/bull/) is still a great resource worth exploring, do check it out.

Thanks for the audience and I hope you found this article helpful 🤗. Feel free to reach out to me on  [Github](https://github.com/nextwebb), [Twitter](https://twitter.com/i_am_nextwebb) and [LinkedIn](https://www.linkedin.com/in/peterson-oaikhenah-102645144/).
Do drop a like, comment, and share 😌.


Happy coding 💙!



 

