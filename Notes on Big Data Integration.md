# Notes  on Big Data Integration

[TOC]

Big Data Integration (BDI)

## Motivation: Challenges and Opportunities for BDI

### Traditional Data Integration

Goal: providing unified access to data residing in multiple, autonomous data sources.

Challenges: semantic ambiguity, instance representation ambiguity, and data inconsistency.

Major steps: Schema Alignment -> Record linkage -> Data fusion.

> Definition (Schema Alignment) Consider a set of source schemas in the same domain, where different schemas may describe the domain in different ways. Schema alignment generates three outcomes.
>
> 1. A mediated schema that provides a unified view of the disparate sources and captures the salient aspects of the domain being considered.
> 2. An attribute matching that matches attributes in each source schema to the corresponding attributes in the mediated schema.
> 3. A schema mapping between each source schema and the mediated schema to specify the semantic relationships between the contents of the source and that of the mediated data. 

> Definition (Record Linkage) Consider a set of data sources, each providing a set of records over a set of attributes. Record linkage computes a partitioning of the set of records, such that each partition identifies the records that refer to a distinct entity.

> Definition (Data Fusion) Consider a set of data items, and a set of data sources each of which provides values for a subset of the data items. Data fusion decides the true value(s) for each data item. 



### BDI: Challenges

**The "V" Dimensions**:  Volume, Velocity, Variety, Veracity

### BDI: Opportunities

1. Data Redundancy
2. Long Data
3. Big Data Platforms

## Shema Alignment

###Traditional Schema Alignment 

The traditionl approach for schema alignment contains three steps: creating a mediated schema, attribute matching, and schema mapping.

**Mediated Schema** providing a unified and virtual view of the disparate sources and capturing the salient aspects of the domain being considered. The mediated schema may not contain every piece of information from every source.

**Attribute Matching** attributes in each source schema are matched to the corresponding attributes in the mediated schema.

**Schema Mapping** is built between each source schema and the mediated schema, specifying the semantic relationships between the contents of different data sources and would be used to reformulate a user query on the mediated schema into a set of queries on the underlying data sources. 

The three types of schema mapping:

1. global-as-view (GAV), obtain data in the mediated schema by querying the data in source schemas
2. local-as-view (LAV), specify the source data as a view of the mediated data, make it easy to add a new data source with a new schema
3. global-local-as-view (GLAV), specify both the mediated data and the local data as views of data of a virtual schema

**Query Answering** Users query the underlying data in a data-integration system by formulating queries over the mediated schema.

### Addressing the Variety and Velocity Challenges

This section describes a **dataspace support platform** that addresses the variety and velocity of data by **pay-as-you-go** data management: provide some services from the outset and evolve the schema mappings between the different sources on an as-needed basis.

#### Probabilistic Schema Alignment

**Probabilistic Mediated Schema** The mediated schema is the set of schema terms (e.g. relational table, attribute names) on which queries are posed. It describes the aspects of the domain that are important for the integration application.

Consider a set of source schemas $\{S_1,\dots,S_n \}$. Denote the attributes in schema $S_i$, $i\in[1,n]$, by $\overline{A}(S_i)$, and the set of all source attributes as $\mathcal{A}$. That is, $\mathcal{A}=\overline{A}(S_1)\cup\cdots\cup\overline{A}(S_n)$. Denote a mediated schema for the set of sources $\{S_1,\dots,S_n \}$ by $\mathrm{Med}=\{\mathbf{A}_1,\dots,\mathbf{A}_m\}$, where each $\mathbf{A}_i$, $i\in[1,m]$, is called a *mediated attribute*. The mediated attributes are sets of attributes from the sources, (i.e., $\mathbf{A}_i\subseteq\mathcal{A}$); for each $i,j\in[1, m]$, $i\neq j$, it holds that $\mathbf{A}_i\cap\mathbf{A}_j=\empty$. As stated before, if a query contains an attribute $A\in \mathbf{A}_i$, $i\in[1,m]$, then when answering the query $A$ is replaced everywhere with $\mathbf{A}_i$.

> Definition (Probabilistic Mediated Schema) Let $\{S_1,\dots,S_n \}$ be a set of schemas. A *probabilistic mediated schema (p-med-schema)* for $\{S_1,\dots,S_n\}$ is a set
> $$
> \mathrm{pMed}=\{(\mathrm{Med}_1, \mathrm{Pr}(\mathrm{Med_1})),\dots,(\mathrm{Med}_l, \mathrm{Pr}(\mathrm{Med}_l))\}
> $$
> Where for each $i\in[1,l]$, $\mathrm{Med}_i$ is a mediated schema for $S_1,\dots, S_n$, and for each $i, j\in[1,l]$, $i\neq j$, $\mathrm{Med}_i$ and $\mathrm{Med}_j$ correspond to different clusterings of the source attributes in $\mathcal{A}$; and $\mathrm{Pr}(\mathrm{Med}_i)\in (0, 1]$, and $\sum_{i=1}^l\mathrm{Pr}(\mathrm{Med}_i)=1$.

A possible solution for probabilistic mediated schema is, first construct the multiple mediated schemas $\mathrm{Med}_1,\dots,\mathrm{Med}_l$ in $\mathrm{pMed}$, and then assign each of them a probability. Two pieces of information available in the source schemas can serve as evidence for attribute clustering: (1) pairwise similarity of source attributes; and (2) statistical co-occurrence properties of source attributes.

**Probabilistic Schema Mapping** Schema mapping describe the relationship between the contents of the sources and that of the mediated data. A probabilistic schema mapping describes a probability distribution of a set of possible schema mappings between a source schema and a target schema.

> Definition (Probabilistic Mapping) Let $S$ and $T$ be relational schemas, each containing a single relational table. A *probabilistic mapping (p-mapping)*, $pM$, between source $S$ and target $T$ is a set $pM=\{(M_1,\mathrm{Pr}(M_1)),\dot, (M_l,\mathrm{Pr}(M_l))\}$, such that 
>
> - for $i\in [1,l]$, $\mathrm{M}_i$ is a one-to-one attribute matching between $S$ and $T$, and for every $i, j\in[1,l]$, $M_i$ and $M_j$ are different; and
> - $\mathrm{Pr}(M_i)\in (0, 1]$ And $\sum_{i=1}^l\mathrm{Pr}(M_i)=1$.

> Definition (Consitent P-mapping) A p-mapping $pM$ is constant with a weighted matching $m_{i,j}$ between a pair of source attribute and target attribute if the sum of the probilities of all mappings $M\in pM$ containing matching $(i,j)$ equals $w_{i,j}$; that is,
> $$
> w_{i,j}=\sum_{M\in pM, (i,j)\in M}\mathrm{Pr}(M).
> $$
> A p-mapping is consistent with a set of weighted matchings $\mathbf{m}$ if it is consistent with each weighted matching in $m\in\mathbf{m}$.

**Query Answering**



### Addressing the Variety and Volume Challenges

This section describes reccent progress in integration two types of structured data on the web.

#### Integrating Deep Web Data

The **deep web** refers to data stored in underlying databases and queried by HTML forms.

#### Integrating Web Tables




