The Bootstrap Problem
=====================

How does a node get to know about activity in a multicast group or an
invitation to a new group or a friendship request etc. when it happened
while was offline?


What doesn't work
-----------------

- Waiting for another message in a multicast group to look at the
  increased message_id and realize that there must have been another
  message in the past. Multicast groups are usually asynchronous, so it
  might take months or years for another message to be sent.
- Having the sender or some server continuously poll the recipient and
  try to notify him. Too much overhead.


Our Approach
------------

Create a distributed hash table where the node stores the identifiers of
of some other nodes that he will poll for notifications upon booting up.
Peers trying to contact him (but failing to do so because he's offline)
are requested to send notifications to these "notification nodes".


### Pros

- No need to update everyone in a multicast group when the list of
  notification nodes changes. (Also: No need to store any such
  information for one's neighbors in a multicast group.)


### Cons

- The notification nodes may not consist of friends of the node.
  Otherwise he will give away his social graph to anyone who's
  interested in him and asks for his notification nodes. Instead, he
  should list his other devices and possibly some public servers.


### Further thoughts

- We might even resort to storing the notifications in the DHT itself.
