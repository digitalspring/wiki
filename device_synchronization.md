Device synchronization
======================

Introduction
------------

There are several ways to approach this problem and so far it has not
been decided yet which path we will take.

What is clear is that if a person uses multiple devices, each device
will be identified by its own key pair. The person's identity
therefore consists of several key pairs, governed by a master key pair
– the latter of which is synonymous to the identity itself and is used
to create new device keys or revoke them.

The problem lies in the question how one subscribes to / joins a peer
group if one's identity spans multiple devices. Which of the keys
shall we use to subscribe? The master key? But then all messages we
receive would be encrypted with the master key and we'd need to have
the corresponding private key available on all devices – which is not
an option. On the other hand, if we only subscribe with one of our
device keys, this device would need to forward all messages to the
remaining devices. But what happens if this one device is offline?

Hence, the only two options are the following:


Option 1: Create a subkey of the master key
-------------------------------------------

While the actual master key can still be used for revoking the subkey
and should therefore be stored in a safe place, a subkey will be used in
the master key's stead for all practical purposes. This means that it
can be stored on all devices such that every device can receive messages
encrypted with the corresponding public key. The device keys (which, in
turn, would be subkeys of the mentioned subkey) would only used for
purposes such as addressing a specific device.

However, this implies that, for the purposes of synchronization, all
devices operate under the same name / public key, and the network layer
must be able to handle this situation. More specifically, it must ensure
that *all* devices receive a message addressed to the public key.
Alternatively, if only one device receives the message (e.g. because the
network layer decides for *one* device to send the messages to), this
device must forward the message to all other devices. This leads to
issues, though, in case this one device goes offline right after having
received a message (i.e. before it was able to forward it), or if the
chosen device's bandwidth is low and forwarding therefore takes plenty
of time.


Option 2: Subscribe with *all* device keys
------------------------------------------

This is probably the easiest solution because synchronization happens by
virtue of the syncing between peers of the same group. However, to this
end, we would need to synchronize the groups all the devices should be
members of.

Finally, this should prove more flexible than the first option because
we can decide for each device individually which groups it should
subscribe to. (For instance, we would probably avoid subscribing our
mobile phone to a peer group that exchanges high-definition videos.)

We currently favor this approach.
