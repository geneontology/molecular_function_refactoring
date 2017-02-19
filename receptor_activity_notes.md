# Receptor activity notes

A basic, uncontroversial taxonomy of [receptor types](https://en.wikipedia.org/wiki/Receptor_(biochemistry)#Structure) is pretty straightforward:

- transmembrane receptor: transduces a signal across a membrane.
  - iontropic receptor: A sensor on one side of the membrane (typically ligand binding) opens or closes an ion channel. 
  - [enzyme linked transmembrane receptor](https://en.wikipedia.org/wiki/Enzyme-linked_receptor):  A sensor on one side of a membrane (typically ligand binding) activates an enzyme activity on the other (protein kinase or guanylate cyclase activity)
  - G-protein coupled receptor: A sensor on one side of the membrane (ligand binding or light) activates G-protein specific GEF activity on the other side of the membrane.
- [nuclear receptor](https://en.wikipedia.org/wiki/Nuclear_receptor): Binding of ligand activates transcription factor acivity (typically by activating DNA binding activity, although in some cases there is also an effect on the ability to bind other parts of the transcriptional machinery.)

Note that, perhaps with the exception of nuclear receptor, these classes are defined without reference to larger processes.  

The *term* receptor is also used for 'cargo receptors'.  These bind and target particular molecules as part of vesicular transport. They do not transduce a signal and should probably be treated as a seaprate, unrelated heirarchy in the GO.

Is it possible to unify the two transducing receptors classes described above using a necessary and sufficient (logical definition)? Defining necessary conditions is straightforward enough, but defining a sufficient set that distinguishes receptors from other transducers is surprsingly hard.  It is tempting to try to define receptors as transducers that detect a signal from outside of a cell. This distingishes the classical nuclear receptors such as that for retinoic acid or for estrogen, from non-membrane transducers activated by intracellular ligands such as PKA (activated by cAMP) and calmodulin (activated by Ca2+).  But some iontropic receptors use intracellular ligands, (e.g. IP3 receptors and ryanodine receptors on the ER), and some nuclear receptors may also [respond to intracellular signals](https://en.wikipedia.org/wiki/Nuclear_receptor#Ligands). In the absence of clear necessary and sufficient criteria for defining a transducing receptor, we should probably just assert it as a superclassification for 'transmembrane receptor' and 'nuclear receptor' and use this as the genus class for defining all subclasses.

Unifying transmembrane receptors with some necessary and sufficient definition is more straightforward.  But what relation should we use to link receptor to membrane? **'occurs in'** is insufficient: Ligand binding doesn't (typically) occur in the membrane and arguably neither do enzyme effector functions or GEF activity.  To indicate cargo for transmembrane transporters and channels we use a specialized relation: **results_in_transport_across**.  By analogy we could define **results in  transduction across** or **results in regulation across**.  This same relation could be used to link receptors to specific membrane types in LEGO models.

## Proposal:

Add the above hierarchy using that [standard molecular transducer pattern](https://github.com/geneontology/molecular_function_refactoring/issues/31#issuecomment-278605047) with 'transducing receptor' as the genus:

* transmembrane receptor: 'transducing receptor activity'' that 'results in transduction across' some 'membrane'
   * iontropic receptor: 'transducing receptor' that 'results in transduction across' some 'membrane' and 'has effector' some 'ion channel activity'
   * enzyme linked transmembrane receptor'": 'transducing receptor activity' that 'results in transduction across' some 'membrane' and 'has effector' some 'catalytic activity'
   * GPCR activity: 'transducing receptor activity' that 'results in transduction across' some 'membrane' and 'has effector' some 'GEF activity'
 * 'nuclear receptor activity': 'transducing receptor activity' that 'has effector' some '(sequence-specific DNA binding) transcription factor activity' ( and occurs_in some intracellular ?)

## Classifications of receptors that take into account process context

For general discussion see [this ticket](https://github.com/geneontology/molecular_function_refactoring/issues/38).  

* origin of signal (differentia: part relationships to BP process context).
  * intracellular:
     (BP term TBA)
  * extracellular
     (TBD: add general class or not.)
     - environment
       (BP term TBA)
     - cell-cell signaling
        part\_of signal reception involved in cell-cell signalling\*
     - ECM-cell signaling
        part\_of signal reception involved in ECM-cell signalling\*
      
\* part_of cell-cell or ECM signaling is not sufficient as these processes include signal transduction, which can involve intracellar receptors such as IP3 receptor etc.

* location of receptor: intracellular, plasma membrane, ER

differentia: 'results in transduction across' X membrane / 'occurs in' some intracellular

Naming - need to be very careful to distinguish intracellular location of receptor/activity from intracellular origin of signal. Nuclear receptors are intracellular but mostly respond to extracellular signals. Some ion channels in the plasma membrane are triggered by intracellular ligands.

















