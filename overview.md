# Molecular function refactoring - Overview

## Introduction

Molecular function is very poorly formalised.  Only 16% of terms have logical definitions (compared to 70% for biological_process) and only 10% of classifications are inferred (compared to 64% in biological_process).  

Multiple inheritance classification summary:

| No. Terms | No. parents | 
|-----------|-------------|
| 7672 | 1 |
| 2197 | 2 |
| 626  | 3 |
| 193 | 4 |
| 89  | > 4 |

Given that manually maintained multiple inheritance heirarchies are typically incomplete, the true level of multiple inheritance is almost certainly higher.  The propsed new classifications in this refactoring will increase this further.  This situation is not sustainable without use of design pattern to drive inference.  In the absence of this, errors an ommissions in the class heirarchy will inevitably accumulate.


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

Many tickets on the MD refactoring tracker follow this pattern

### Bottom up approach: 

Work up from existing classes under Paul's new classes to try to find common design patterns & LEGO templates to automate classification.

### Outputs

* Edits are taking place on [a branched version of the GO](http://viewvc.geneontology.org/viewvc/GO-SVN/trunk/experimental/David/MF_refactoring/). Diffs can be inspected here: 

* Design patterns and templates are specified in the [GO design pattern repository](https://github.com/geneontology/design_patterns). Files here are automatically checked for validity using a [Travis job](https://travis-ci.org/geneontology/design_patterns).

* Documentation of standard practises for LEGO annotation that maximise inference will be developed on this repostitory.

## What is a molecular function?

(See [ticket: Decide on new upper level classification in MF](https://github.com/geneontology/molecular_function_refactoring/issues/27))

A Molecular Function is any process carried out by a *single* gene product or complex, which that complex has evolved to carry out.

* General classes of processes carried out by a single gene product are molecular functions not biological processes
   * e.g. topiosomerase activity is_a DNA tertiary structure modifying activity (MF)
* Some molecular functions, including catalytic activity and binding, are simple - they are considered atomic/indivisible by GO.  We refer to these as biochemical activities.
* Some molecular functions are made up of multiple, causally linked simple functions: 
   * ligand binding regulates effector function in a receptor
   * Ca2+ binding regulates protein binding by calmodulin
   * ATPase activity provide energy for ion transporter activity
* Some MF classes are defined, in part, by process context.  For example, chemoattractant activity involves a single gene productbinding and activating signaling receptor in context context of positive chemotaxis.

On the basis of this, we define two, *non-disjoint* classes under molecular function: biochemical activity (simple functions); system component function (compound/MFs defined by process context).

TBD: adding a class for compound function.

## Strategies for defining design patterns and templates

Combined design-pattern / template docs will be specified using DOS-DPs, which now support specification of LEGO templates.

The components of compound molecular functions are causally linked.  Co-reference issues prevent these causal links from being represented on the class level, but we can represent them directly in LEGO templates.  We can also use conventions within design patterns to specify regulatory input and effector/output.

## Strategies for inference

In addition to the standard OWL-EL tools for inference that we use in the ontology, we can also use SRWL rules to drive inference within the LEGO model. This turns out to be particularly useful for reducing the burden on curators and maximising inference.

For example... TBA

## Classifying enzyme activities

See ticket: https://github.com/geneontology/molecular_function_refactoring/issues/22




