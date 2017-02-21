# Transducers, sensors, integrators and inference from direct regulation

## Compound functions

Compound functions are functions made up of multiple subfunctions.  We use has_part, or subproperties of has_part, to relate compound functions to their subfunctions.

Reasoning about compound functions:

~~~~~~~~~~
gp enables MF1
MF1 has_part MF2
-> gp enables MF2
~~~~~~~~~~~

It may be worth using a rule rather than a property chain for this in order to restrict to MFs.

TBD: Do we allow subfunctions to also be compound?


## Transducers and sensors

A transducer is a compound function consisting of  sensor and effector functions where the sensor regulates the activity of the effector.  Sensors are simple functions like binding (possibly also sensing of modification state, e.g. phosphorylation). These simple functions only become sensors by virtue of being part of a transducer that regulates transducer activity.  We can represent the regulatory edge between sensor and effector in LEGO, but not on the class level.  Instead, on the class level we use specialised subproperties of has_part to indicate sensor and effector.

~~~~~~ 
'has part' (transitive)
   <-subPropertyOf- 'has sensor' (non-transitive)
   <-subPropertyOf- 'has effector' (non-transitive)
~~~~~~~

TBD: How do these work with compounding of compound functions if we allow it?

**Some subclasses of transducer:**

* calcium sensing transducer activity^ ***EquivalentTo:*** transducer *that* **has sensor** *some* 'Ca2+ binding'
* 'transducer via protein binding'^^ ***EquivalentTo:*** transducer *that* **has sensor** *some* 'protein binding' 

^ we could just call this calcium sensor activity

^^ the precise set of general classifications for transducers is still under discussion

## Defining direct regulation

~~~~~~~~~~
X directly positively regulates Y iff:
   X positively regulates Y
   X enabled_by gp1
   Y enabled_by gp2
   X has_input gp2
~~~~~~~~~~

With this we can define classes such as:

'kinase activator activity: ***EquivalentTo:*** molecular_function *that* **directly_positively_regulates** *some* 'kinase activity'


## direct regulation and transducers in LEGO

Putting this all together in LEGO, what inference do we need, what patterns should we support?

Modelling as simple functions:

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

reasoning can be enabled using a property chain: 

enables o directly_positively_regulates -> has_input 

But consider:

~~~~~~~~~~~
GP1 enables (kinase activity)k
k directly_positively_regulates BP(bp)
~~~~~~~~~~

Would we still want the has_input inference. If not can we use a rule limit this inference to MFs?


## Transducer via protein binding - Single binding node

~~~~~~~~~
GP1 enables binding(b)
b has_input GP2
GP2 enables transducer activity(t)
t has_sensor b
t has_effector kinase activity (k)
t internally_positively_regulates k

=> b directly_positively_regulates k
=> b enables 'kinase activator activity'

~~~~~~~~~

reasoning requires this property chain: 

input_of^ o internally_positively_regulates -> directly_postively_regulates

^ On the instance level we can rely on inference of inverses.


## Transducer via protein binding - separate binding nodes


~~~~~~~~~
GP1 enables binding(b)
b1 has_input GP2
GP2 enables transducer activity(t)
t has_sensor binding(b2)
t has_effector kinase activity (k)
t positively_regulates k

~~~~~~~~~~

This is unsafe: 'b1 directly_positively_regulates k' 

b1 might be binding to some part of GP2 that doesn't regulate k. We also have no way to infer without linking b1 and b2. Solution: Add an edge to link b1 to b2 (sameAs?)

What about inference in the other direction?  As long as we use has_sensor and has_effector on the instance level, ontology design patterns mean this just works.  However, the classification may also be introduced by choice of template.

...

### Constraints/patterns for LEGO annotation

Should we support all possible patterns and rely on inference to resolve differences, or should we be constraining/standardising pattterns. 

## Integrators

(Suggestion from Paul T)

Some compound molecular functions are integrators, rather than simple transducers.  For example NMDA receptors are only activated by a combination of glutmate binding, glycing binding and membrane depolarization.  Might it be possible to represent integrator functions in OWL on the instance level?  Or is this a step too far..?  An integrator would be a compound function with multiple sensors and integrator component that encodes how signals are combined to regulate an effector function.   
