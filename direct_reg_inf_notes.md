## Transducers, sensors, integrators and inference from direct regulation

A transducer is a compound function consisting of a sensor and effector functions where the sensor regulates the activity of the effector.  Sensors are simple functions like binding (possibly also sensing of phosphorylation state?). These simple functions only become sensors by virtue of being part of a transducer that regulates transducer activity.  We can represent the regulatory edge between sensor and effector in LEGO, but not on the class level.  Instead, on the class level we use specialised subproperties of has_part to indicate sensor and effector.

Some subclasses of transducer:

calcium sensing transducer activity^ EquivalentTo: transducer *that* **has sensor** *some* 'Ca2+ binding'
transducer via protein binding EquivalentTo: transducer *that* **has sensor** *some* 'protein binding'

^ we could jsut call this calcium sensor activity


Defining direct regulation:

X directly positively regulates Y iff:
   X positively regulates Y
   X enabled_by gp1
   Y enabled_by gp2
   X has_input gp2

With this we can define:

'kinase activator activity: molecular_function that directly_positively_regulates some kinase activity

Reasoning about compound functions.

~~~~~~~~~~
gp enables MF1
MF1 has_part MF2
-> gp enables MF2
~~~~~~~~~~~

May be worth using a rule rather than a property chain for this in order to restrict to MFs

Putting this all together in LEGO, what inference do we need, what patterns should we support?

Simple functions:

~~~~~~~~~~~~
GP1 enables kinase activity(k1)
GP2 enables kinase activity(k2)
k1 directly_positively_regulates k2
=> k1 has_input GP2
~~~~~~~~~~~~~

~~~~~~~~~~~~~
GP1 enables protein binding(b)
GP2 enables kinase activity(k)
b directly_positively_regulates k
=> b has_input GP2
~~~~~~~~~~~~~

property chain: enables o directly_positively_regulates -> has_input 
Or use rule to limit this to MFs?


## Transducer via protein binding - Single binding node

~~~~~~~~~
GP1 enables binding(b)
b has_input GP2
GP2 enables transducer activity(t)
t has_part b
t has_part kinase activity (k)
t internally_positively_regulates k

=> b directly_positively_regulates k
=> b enables 'kinase activator activity'

~~~~~~~~~

has_input o internally_positively_regulates -> directly_postively_regulates


## Transducer via protein binding - separate binding nodes


~~~~~~~~~
GP1 enables binding(b)
b1 has_input GP2
GP2 enables transducer activity(t)
t has_part binding(b2)
t has_part kinase activity (k)
t positively_regulates k

~~~~~~~~~~

This is unsafe: b1 directly_positively_regulates k
b1 might be binding to some part of GP2 that doesn't regulate k
We also have no way to infer without linking b1 and b2
Solution: Add an edge to bind b1 to b2 (sameAs?)


