[![DOI](https://zenodo.org/badge/510770846.svg)](https://doi.org/10.5281/zenodo.7180828)
![GitHub issues](https://img.shields.io/github/issues/StructuralXplorationLab/Libra?logo=GitHub)
![GitHub closed issues](https://img.shields.io/github/issues-closed/StructuralXplorationLab/Libra?logo=GitHub)
![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/logoTitle_1200x1200.png)

# Table of contents

- [General info](#general-info)
- [Installation](#installation)
- [Tutorials](#tutorials)
- [Concept](#concept)
- [Usage](#usage)
- [Background theory](#background-theory)
- [Known issues](#known-issues)
- [Version history](#version-history)
- [Related publications](#related-publications)
- [Citation](#citation)
- [License](#license)
- [Collaborations](#collaborations)
- [Acknowledgements](#acknowledgements)

# General info

**_Libra_** is a Grasshopper plug-in operating in Rhinoceros 3D by McNeel. It has been developed by [Ioannis Mirtsopoulos](https://linkedin.com/in/ioannismirtsopoulos/) as part of his doctoral research at [Structural Xploration Lab (SXL)](https://www.epfl.ch/labs/sxl/) at [EPFL](https://www.epfl.ch/en/) and his postdoctoral stay at [Digital Structures](http://digitalstructures.mit.edu/) research group at [Massachusetts Institute of Technology (MIT)](https://www.mit.edu/), in USA. **_Libra_** is a _conceptual structural design tool_ that operates on a _topology grammar_, inspired by _Noam's Chomsky_ _"Syntactic Structures"_ and many others.

# Installation

**Operating System**: Windows _(preferred)_; Mac _(at your own risk)_

**Version**: Rhinoceros **8**

1. Run the _PackageManager_ command.
2. Look for _Libra_.
3. Click _install_.
4. Restart _Rhino_ to load the plugin.

Check the video tutorial [here](https://www.rhino3d.com/features/package-manager/)

# Tutorials

- [Libra Video Tutorials](https://vimeo.com/libragh)
- [Libra Manual](https://github.com/StructuralXplorationLab/Libra/blob/main/doc)

# Concept

Libra describes an incremental transformative process that allows the transition from an incomplete network (Fig. 1, stage 00) to a complete one (Fig. 1, stage 13). In other words, the incremental elimination of interim forces in the network. At every intermediate step the network is in static equilibrium (interim when interim forces—vectors in cyan—are still present, or global when no interim forces exist).

![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/IncrementalTransformativeProcess_1200x513.png)

_Fig. 1: Incremental transformative process._

# Usage

You can find **_Libra_**'s detailed manual [here](https://github.com/StructuralXplorationLab/Libra/blob/main/doc/Libra_User_Manual_1.0.0.pdf). Alternatively, below you have a brief to start with...

A primitive setup of the transformative design process with Libra is demonstrated below in Fig. 6. Each of the circled component groups accomplishes a certain task. Groups 1 and 2, provide the permanent input parameters and groups 3-6 describe the transformation policy through transient parameters. Group 7 applies the transformation in compliance with the built policy.

1. Construct design domain
2. Construct model
3. Select forces
4. Place new node
5. Set force indeterminacies
6. Construct policy
7. Apply transformation

Groups 3-6 describe design decisions that can be applied once or for the course of multiple transformations. The number of transformations is defined at group 7. If different decisions need to be made, groups 3-6 need to be redefined (the respective components must be introduced again on Grasshopper canvas) and the transformed model (output from group 7) should be provided as input for the upcoming transformations, if more interim forces exist.

![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/GrasshopperCanvasOverview_1200x517.png)

_Fig. 2: Grasshopper canvas overview of a complete trnasformation setup that transforms the interim network according to a policy defined via rules._

# Background theory

Structural design describes forms that synthesize structural behaviors—they ‘tell’ how a structure withstands and/or deforms under the application of loads. Structural behaviors are subject to various laws, among which static equilibrium of forces plays a central role. The concept of static equilibrium in structures is one that can be reduced to minimal, abstract equilibrium configurations that consist of vectors, nodes, and bars in compression or tension.

For given loading and boundary conditions, the number of arrangements of tension and compression elements in static equilibrium is theoretically infinite. In practice, only a small, finite subset of equilibrium configurations are explored and/or employed. **Design space exploration (DSE)** is a creative process that consists of incremental generation of multiple design variants (design options) and is chronologically framed in the early design stage. DSE helps designers to find high quality design alternatives and fights against premature design fixation, which notoriously results in the generation of resembling design variants.

## **Transformation step**

Each step describes a transformation that consists of the introduction of:

- a new **node** (P)
- one, two (or three) new **bars**
- one, two or three new **interim forces** imposed by the static equilibrium condition

Fig. 3 illustrates a simple example of a transformation step.

![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/BeforeAfterTransformation_1200x300.png)

_Fig. 3: Before/After transformation in space. Black arrows represent force actions that are applied externally to the (sub)system. Cyan arrows represent interim forces. Bars in compression are thick lines. Bars in tension are thin lines._

## **Design input**

Each transformation is controlled by four design decisions:

1. **The transformation impact**—the numerical difference of interim forces in the network before and after the transformation, described as the **entropy rate**. As a designer, you express your intention through a ternary decision among:

- _convergence_; decrease the number of interim forces
- _stagnation_; stagnate the number of interim forces
- _divergence_; increase the number of interim forces

**Hint**: This decision directly affects the speed of the transition to a complete network.

2. **The interim forces to eliminate**—the selection of the interim forces as the starting point of each transformation and the topology of the new sub-network, described as **force(s) selection**. According to Fig. 4, the possible topologies are:

- _in-between_
- _peripheral_
- _central_

![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/ConnectivityTopologies_1200x263.png)

_Fig. 4: The possible connectivity topologies._

The available **force(s) selection** options are:

- _monomial_; only one force vector is selected
- _binomial_; two force vectors are selected
- _trinomial_; three force vectors are selected (feature currently not activated)

**Attention**: Until v1.1.0, _trinomials_ are not activated. Subsequently, the possible topological configurations are two: _in-between_ and _peripheral/central_. The latter result in the same transformation result. 

As a designer, you make the selection _implicitly_ or _explicitly_.

- _explicitly_; manual selection of the interim forces and the topology
- _implicitly_; automated selection of forces and topology through a designer-chosen rule (there is a list of rules, additional parameters might be required)

3. **The transformation geometry**—the position of the new node P, described as **node placement**. As a designer, you do the placement _implicitly_ or _explicitly_.

- _explicitly_; manual placement of the node
- _implicitly_; automated placement through a designer-chosen rule (there is a list of rules; additional parameters might be required)

4. **The structural behavior of the transformation**—the type (compression/tension) and magnitude of the axial forces developed along the introduced bars, described as **force indeterminacies**. As a designer, you make the selection _implicitly_ or _explicitly_.

- _explicitly_; manual provision of axial forces (N1, N2, N3) for all two (or three) new bars (**Attention**: N3 is only necessary when working with _trinomials_.)
- _implicitly_; automated definition of axial forces (there is a list of self-explanatory rules; additional parameters might be required)

Fig. 5 illustrates all possible transformations in compliance with the design decisions made. The provided force diagram confirms the static equilibrium condition after the transformation.

**Hint**: Consider drafting the force diagram of each transformation while designing! It will give you a better understanding of the transformation final geometry.

![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/TransformationStep_1200x1755.png)

_Fig. 5: Transformation step for all combinations of entropy rates, numbers of interim forces and topological configurations. Black arrows represent force actions that are applied externally to the (sub)system; cyan arrows represent interim forces, all circumscribed within a primitive design domain (grey). Bars in compression are thick lines. Bars in tension are thin lines._

Libra provides a design workflow that ensures static equilibrium of the generated network. This condition is satisfied by constraining the placement of the new node within a specific geometric domain, named **entropy rate domain** (Fig. 6). In other words, the entropy rate domain describes all possible locations that node P can take, while the network retains static equilibrium. The size of this domain varies, from a single point in space to a volume. Do not worry, as a designer, you do not have to compute this domain. If you manually request the placement of node P at a location outside of the entropy rate domain, the algorithm will consider the closest projected location on the entropy rate domain, ensuring static equilibrium.

Between node P and the existing nodes of the network, new bars are introduced. Their construction implies that the bars are not interrupted by voids or non-convexities of the design domain. This condition is satisfied when certain visibilities between nodes are satisfied. The geometric domain that describes all possible locations that node P can have, while ensuring that the necessary bars can be constructed uninterrupted is named **constructability domain** (Fig. 6). Do not worry, as a designer, you do not have to compute this domain. The algorithm will check that. Be aware though that the geometric domain where node P can be safely introduced during each transformation is the intersection of the two domains and is named **feasibility domain**.

![alt text](https://github.com/StructuralXplorationLab/Libra/blob/main/img/EntropyRate%2BConstructabilityDomains_1200x1237.png)

_Fig. 6: Typologies of entropy rate and constructability domains. Black arrows represent force actions that are applied externally to the (sub)system; cyan arrows represent interim forces, all circumscribed within a primitive design domain (grey). Magenta regions on the left are the intersections of the design domain (a non-convex solid with a void) with the entropy rate domain. Magenta regions on the right are the intersections of the design domain with the constructability domain._

# Known issues

- The _Construct Force Selection Rule_ complonent (and some others) have **dynamic** number of inputs, which means that the number of input slots varies based on the desired rule ID. When shifting from force selection rule _Random_ or _ProximityToUserSelectedPoint_ to an other rule, the rule might not be constructed. Try to toggle some of the other input parameters and use a label component to make sure _ForseS_ outputs a rule.

- The _Construct domain_ component does **not** support multiple voids. If more than one are provided, only the first one will be considered.

- In the _Design Space Exploration_ component the population of each generation is clustered to 12 representative designs. The clustering is **genotype-based** and therefore the 12 representatives can be misleading, aka "hiding" designs that have different phenotype. For extensive design exploration it is highly recommended to browse through the entire population of each generation.

# Version history

https://github.com/StructuralXplorationLab/Libra/releases

# Related publications
- **Mirtsopoulos, I.**, Fivet, C., 2023. “Structural Topology Exploration through Policy-Based Generation of Equilibrium Representations.” _Computer-Aided Design_ 160 (July 1, 2023): 103518. <https://doi.org/10.1016/j.cad.2023.103518>

- **Mirtsopoulos, I.**, Fivet, C., 2022. "Exploration of static equilibrium representations; policies and genetic algorithms", in: _Structures and Architecture A Viable Urban Perspective?_ pp. 1137–1144. <https://doi.org/10.1201/9781003023555-136>

- **Mirtsopoulos, I.**, Fivet, C., 2021. "Grammar-based generation of bar networks in static equilibrium with bounded bar lengths", in: _Proceedings of International Association for Shell and Spatial Structures (IASS) Symposium 2020/21_. 23-27 August, Guilford, UK. <https://doi.org/10.15126/900337>

- **Mirtsopoulos, I.**, Fivet, C., 2020. "Design space exploration through force-based grammar rule". _archiDOCT_ 8, 50–64.

# Citation

```
@software{Mirtsopoulos_Libra_A_grammar_2024,
author = {Mirtsopoulos, Ioannis},
doi = {10.5281/zenodo.14292161},
month = dec,
title = {{Libra: A grammar for generating structural topologies}},
url = {https://doi.org/10.5281/zenodo.14292161},
version = {1.4.5},
year = {2024}
}
```

# License

[MIT](https://choosealicense.com/licenses/mit/)

# Collaborations

Collaborations and/or contributions are always welcome. Plus I am always curious to see how creative you have gone with _Libra_. Drop me a line at [ioannis@mirtsopoulos.xyz](mailto:ioannis@mirtsopoulos.xyz) if you want to brainstorm about generative design, to share your designs or just to say hi!

# Acknowledgements

Special thanks to Dr. John Harding for sharing the beauty of _Biomorpher_; a Grasshopper implementation of _Interactive Genetic Algorithms_. The components in the _Xploration_ tab (until v1.1.0) are taken by _Biomorpher_ and have been adjusted accordingly.
