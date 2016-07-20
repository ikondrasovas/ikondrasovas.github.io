---
published: false
---
## Decouple file Delivery from Code Availability

In order to decouple file delivery from code availability, a great practice I learned from this [continous delivery book](http://www.oreilly.com/webops-perf/free/continuous-delivery-with-windows-and-net.csp) we should separate in two independent task the delivery of the files on the server and making the new software active.

In order to acomplish that, I read a great article about [fine grained versioning with ClickOnce](https://www.infoq.com/articles/ClickOnce-Versioning).

This strategy could be useful not just to acomplish this best practice but also to selectivery choose what users shoudl receive updates first. 

If we want to make some kind of study group or A/B testing, we could use some logic to redicrect the correct version user should download when update.

