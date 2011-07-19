# gae_fast_slow_queue

GAE Fast/Slow Queue is a useful little decorator/utility designed to help
minimize App Engine taskqueues' degradation of user-facing request performance.

It optimizes taskqueue behavior by helping you separate fast-running tasks from
slow-running and splitting the two into separate queues.

gae_fast_slow_queue is designed to replace common App Engine taskqueue/mapper situations like this:

<pre>
def daily_activity_summary(user):
    if user.has_recent_activity():
        # Some slow task
        user.calculate_and_store_recent_activity()
</pre>

...with this:

<pre>
@fast_slow_queue.handler(
    lambda user: user.has_recent_activity()
)
def daily_activity_summary(user):
    # Some slow task
    user.calculate_and_store_recent_activity()

</pre>

See http://bjk5.com/post/7796556212/fast-and-slow-queues-on-app-engine for more about how this helps App Engine scale and why your users care.

This code is [MIT licensed](http://en.wikipedia.org/wiki/MIT_License).
