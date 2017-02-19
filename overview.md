# Molecular function refactoring - Overview

## Introduction

## What is a molecular function?

Biologists frequently refer to the functions and activities of gene products and the complexes they form, for example, asserting that a particular gene product has protein kinase activity, or that another acts as a receptor for glutamate. In building an ontology to represent these functions and activities we need to decide what general class or classes of thing terms like protein kinase activity and glutamate receptor activity refer to.  This question is intimately connected to the question of how we relate these activities and functions to gene products and to the biolgical processes such as signal transduction, glycolysis or gastrulation.

One account of functions and activities is that they are 'realizables'. By saying that a gene product has 'protein kinase activity' I am stating that it has the potential, given the right environmental conditions and the presence of a suitable substrate, to be the active participant (the catalyst) in the process of protein phosphorylation.  In ontology jargon we say the function or activity is *realized* by participation in the process.  This fits nicely with the everyday use of the terms function and activity by biologists: they'd be happy to say that the molecules of a restriction enzyme sitting in glycerol in a -20'C freezer have site-specific endonuclease activity even though they are not currently involved in cutting DNA.

Despite this, the GO asserts that its molecular_function terms refer not to the 'realizables' but to the process in which gene products are the active participants.  More precisely, a GO molecular_function is a process, carried out by a *single* gene product or complex. This has the advantage that it is simple to relate molecular functions to biological processes involving the activities of many gene products: An EGFR receptor activity is **part of** cell-cell signaling; pyruvate kinase activity is **part of** glcolysis.  Within the GO, **part of** is used to relate molecular functions to biological processes which they are always part of.  The newly developed LEGO/nocuta annotation system for GO annotation uses **part of** to capture the component molecular functions of particular instances of biological processes. *(TBA - broader argument - almost all of the direct relationships which are useful to make are relations to the process not the realizable: what its inputs and output are, what it regulates...)

Defining GO molecular_function requires one further clause. The GO and its associated annotations aim to record the cannonical (non-pathalogical) function of gene products. To make this clear, we define a GO molecular_function as any process carried out by a *single* gene product or complex, which that gene product or complex has evolved.  By this definition, virus receptor activity is not a GO molecular function: CD4 did not evolve to bind HIV, HIV evolved to bind CD4. It would be correct, however, to record that HIV:gp230 has the GO molecular function 'protein binding' with input CD4).

### Types of molecular function.  

Some molecular functions are simple biochemical activities such as a catalysing a reaction or binding an ion or another protein.  Other molecular functions involve multiple causally linked biochemical activities. For example a receptor activity has a ligand binding component which regulates and effector component, such as a kinase activity. Ca2+ binding regulates protein binding by calmodulin. In an active transporter activity, ATPase activity provides energy for ion transporter activity. Some MF classes are defined, in part, by process context. For example, chemoattractant activity involves a single gene product binding and activating signaling receptor in context context of positive chemotaxis.

(See [ticket: Decide on new upper level classification in MF](https://github.com/geneontology/molecular_function_refactoring/issues/27) for further discussion.)


## The state of the molecular function hierarchy.

Molecular function is very poorly formalised.  Only 16% of terms have logical definitions (compared to 70% for biological_process) and only 10% of classifications are inferred (compared to 64% in biological_process). 

Multiple inheritance classification summary:

| No. Terms | No. parents | 
|-----------|-------------|
| 7672 | 1 |
| 2197 | 2 |
| 626  | 3 |
| 193 | 4 |
| 89  | > 4 |
(tested on 16:02:2017)

Given that manually maintained multiple inheritance heirarchies are typically incomplete, the true level of multiple inheritance is almost certainly higher. The propsed new classifications in this refactoring will increase this further.  This situation is not sustainable without the use of design pattern to drive inferred classification.  In the absence of this, errors an ommissions in the class heirarchy will inevitably accumulate.

Compound molecular functions are under-respresented in the current heirarchy. For example there is no single term in GO that can be used to record that calmodulin binds calcium and that the calcium binding regulates binding of calmodulin to other proteins. Instead we have seaprate terms for calcium binding and calcium-regulated protein-binding. Where compound functions are represented, they are not represented consistently.  For example,  'kinase receptor activities' are classified under kinase activity, and have their ligand recorded using either has_input or a has_part relationship to a binding term for the ligand.  Gene protein-coupled receptor activity classes in GO currently have no link to their effector function (GEF activity).  

LEGO annotation allows users to construct networks describing particular instances of biology via their component molecular functions and biological processes, recording a variety of relationships between these components including causal (regulatory) relationships and part relationships. The relations used are identical to those used in the ontology and the LEGO models themselves are recorded in OWL. Using standard reasoning software allowing us to use inference to enhance LEGO annotation. 

We need standard patterns for compound molecular functions, compatible with thosed ussed in the ontology.

  
## Aims:

- Clarify the definition of molecular function and its major subdivisions
- Add new intermediate nodes that provide biologically meaningful and useful ways for biologists to group functions 
- Allow curators to better capture the compound nature of functions
- Automate classification via the implementation of design patterns
- Add error checking in the form of disjointeness axioms
- Provide templates and automated inference to simplify and standardise LEGO curation

## Starting point:

Starting point: Paul Thomas' manual [refactoring proposal in WebProtege](https://webprotege.stanford.edu/#Edit:projectId=ea132f81-760a-43f2-b5a9-fbe763bb7eed)

### Top down approach:

Document usefulness and practicality of proposed new terms via [checklist](), recording whether the proposed new term:

* is useful grouping of molecular functions not provided by an existing biological process? 
* is useful for error checking (via disjointness axioms)?
* is useful for construction of design patterns?
* is useful for curators  - e.g. used in templates or guiding template usage?
* requires multiple inheritance classification
  * if so, can we use design patterns & reasoning to populate subclasses?

Many tickets on the [MF refactoring tracker](https://github.com/geneontology/molecular_function_refactoring/issues) follow this pattern.

### Bottom up approach: 

Work up from existing classes under Paul's new classes to try to find common design patterns & LEGO templates to automate classification.

### Outputs

* Edits are taking place on [a branched version of the GO](http://viewvc.geneontology.org/viewvc/GO-SVN/trunk/experimental/David/MF_refactoring/).

* Design patterns and templates are specified in the [GO design pattern repository](https://github.com/geneontology/design_patterns). Files here are automatically checked for validity using a [Travis job](https://travis-ci.org/geneontology/design_patterns).

* Documentation of standard practises for LEGO annotation to ensure correct and complete inference.  For example, [should binding between two gene products be recorded as one node or two?](https://github.com/geneontology/molecular_function_refactoring/issues/29).


## Strategies for defining design patterns and templates

Combined design-pattern / template docs will be specified using DOS-DPs, which now support specification of LEGO templates.

The components of compound molecular functions are causally linked.  Co-reference issues prevent these causal links from being represented on the class level, but we can represent them directly in LEGO templates.  We can also use conventions within design patterns to specify regulatory input and effector/output.

## Strategies for inference

In addition to the standard OWL-EL tools for inference that we use in the ontology, we can also use SRWL rules to drive inference within the LEGO model. This turns out to be particularly useful for reducing the burden on curators and maximising inference.

For example... TBA


Inference on the individual level can also take advantage of inverse relations.  We cannot safely conclude from that statment (all) head has\_part some hair that (all) hair part\_of some head.  But if we record in LEGO that if a receptor activity has 


## Classifying enzyme activities

As far as possible, we will leverage existing classifications such as EC and Rhea.  For details
see ticket: https://github.com/geneontology/molecular_function_refactoring/issues/22




