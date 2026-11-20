
SDN_Firewall Project is basically about building a firewall on top of an SDN controller by running a bunch of controller apps.

This repo has a few Ryu applications, which are just Python scripts that handle firewall stuff on a Ryu controller. To run them, you need Ryu and Mininet installed on your system. These apps can handle ICMP, TCP, and UDP firewall rules.

There are 2 main types of apps, and each type has 2 versions:

* Inefficient Application
* Efficient Application

### Inefficient Firewall

It’s called “inefficient” because it doesn’t really use Open vSwitch flow tables properly. It barely stores flow entries on the switch. Instead, it just forces the switch to send pretty much everything to the controller. Then the controller looks at every single packet and decides what to do with it (allow or drop). So yeah, kind of slow and controller-heavy.

### Efficient Firewall

This one actually uses flow tables on the switch. So once a rule is learned, the switch handles future packets itself without bothering the controller again. Way less traffic to the controller, so it’s faster and more scalable.

### Two behavior styles:

* Stateful Firewall
* Stateless Firewall

#### Stateful Firewall

This one actually remembers connections and packet states. So it tracks ongoing communication between two hosts. When a new packet comes in, it checks if it belongs to an existing connection and uses that info to decide what to do. Basically, it keeps track of “who is talking to who”.

#### Stateless Firewall

This one doesn’t care about connections at all. It just looks at each packet separately and checks it against the firewall rules. No memory, no tracking, just basic match-and-action stuff.

---

So in total, you get 4 firewall apps:

* Inefficient Stateful Firewall
* Inefficient Stateless Firewall
* Efficient (secure) Stateful Firewall
* Efficient (secure) Stateless Firewall

---

### How to mess around with it:

* Install Mininet and Ryu first
* Clone/download this repo
* Build a Mininet topology (like 3 hosts, 1 Open vSwitch, 1 controller — check the guide or appendix stuff)
* Read the example `.txt` files for how firewall rules are written
* Put your own rules into `Firewall.txt`
* Run the right Ryu app using `ryu-manager` on the controller
* For TCP testing, use `nc` (netcat) — one side listens, the other connects
* Send some messages between hosts and watch stuff happen in the controller/switch logs
* If you’re using the “Efficient” version, you can even see flow table entries getting installed on the switch

That’s basically it — mess with rules, run traffic, and see how the firewall reacts.
