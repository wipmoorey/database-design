# Data Analysis for Database Design

- A database is a collection of non-redundant data shared between different applications.
- The simplest way to reduce inconsistent data is to eliminate unnecessary duplication.
- This implies that data should be shared as a common pool shared between many applications.
- Sharing data may ensure consistency but may incur maintenance issues where an application can access to too much data.
- Sharing data may increase the effect of erroneous data as it affects all applications using that data.

## Program data independence

- Addition of new columns should not affect any existing applications.
- Change in physical storage should be isolated from the calling application.
- Change in the order of the columns should not affect any existing applications.

## Database architecture

- They should be an interface between the global view of the data and local view of an application.
- __Conceptual Schema__ A description of all the data of interest to the enterprise (Tables).
- __External Schema__ A description of the data that is of interest to a particular application (View).
- __Internal Schema__ A description of how data is implemented at the storage level.
- __schema__ is a specification of a data view expressed in the language used by a particular database management system.
- __Data model__ Database independent description of the data

## Tables/Relations

- An attribute is a description of some data.
- An attribute value can be null which represents not yet known.
- Order of rows is not significant.
- Order of columns is not significant.
- A columns can only contain a single attribute.
- Each row must be distinct.
- Each table must be distinct.

## Duplicate and Redundant data

- __Duplicate data__ when an attribute has two or more identical values.
- __Redundant data__ is data that is unnecessarily duplicated.

e.g Nut is duplicated but not Redundant:

| Part No | Description |
| :------ | :---------- |
| P2      | Nut         |
| P3      | Bolt        |
| P4      | Nut         |

e.g. Nut is redundant, note the table design is bad.

| Supplier | Part No | Part Description |
| :------- | :------ | :--------------- |
| Ascots   | P2      | Nut              |
| Screwfix | P2      | Nut              |

When desiging a database you want to eliminate redundant data.

## Enterprise Rules

- Define the constraints on attributes
- Define the relations between columns
- Define the relations between tables

## Repeating Groups

- Is where you have the value of an attribute representing a list.

| Supplier | Part No  |
| :------- | :------- |
| ...      | ...      |
| Ascots   | P2,P3,P4 |
| ...      | ...      |

- Prevents ordering
- Difficult to join against
- Difficult to size
- Eliminate by moving into a link table

### Repeating groups of columns

- If there is meaning in the repeating columns then they are ok.
- If there is no meaning they should be moved to a link table.

e.g.

| Supplier | First Part No | Second Part No |
| :------- | :------------ | -------------- |
| ...      | ...           | ...            |
| Ascots   | P2            | P3             |
| ...      | ...           | ....           |

If "First Part No" represent some significant meaning over "Second Part No" like always try to use the part defined by the "First Part No" but if there is an issues getting that part the part defined by the "Second Part No" column can be used then the relation is well designed as it is preserving business meaning, although the table column names could probably be better.

## Determinants

If duplicate values of A always associate to the same value of B then A is a determinant of B.

- A determines B
- B is determined by A
- B is functionally dependent on A

## Superfluous attributes

If _part no_ determines _quantity_ it follows that {part no,description} also determines _quantity_ but __part no_ is sufficient so __description_ is
superfluous.

## Transitive determinants

- If A determines B and B determines C then A also determines C.

## Identities

- An individual row can always be identified by quoting the values of all its fields.

- An identity is any combination of attributes that is sufficient to uniquely identify a row.

- Useful identifiers should not contain any superfluous attributes.

## Normalised tables

- Tables that are structured so that they can not contain redundant data.

- The advantages are:

  - You do not get deletion side effects where you want to delete part of a row so you delete the row and remove some needed data.

  - You do not get insertion side effects where you have to wait until you have all the pieces before you can insert a row.  So you end up with losely coupled highly cohesive tables.

## Normal forms

Each stage of the normalisation rules eliminates an undesirable feature of the data strucutre:

1. Eliminate repeating groups.
2. Eliminate non-identifying attributes that are not functionally dependent on the whole identifier.
3. Eliminate functional dependencies between non-identifying attributes.

There are five normal forms but usually the first three are used. They can be summarised as "The key, the whole key, and nothing but the key".

## Entity relationship mapping

- Is a top down technique.
- Select the entities and relations between them that are of interest to the enterprise.
- Assign attributes to the entities.
- Each occurrence of an attribute must have a unique identifier.
- A relationship identifier is a composite of the identifiers of the the entities that it relates.

## Entity relationship diagrams

- Rectangles are entities.
- Diamonds are relations
- With relation names you need to be aware of the directional nature of the name.

## Degrees of relations

- 1:1
  - A is at most one B
  - B is at most one A

- 1:N
  - A may have many B
  - B has at most one A

- M:N
  - A may have many B
  - B may have many A

## Membership class

Determines whether every entity must participate in the relationship.

- 1:1 extends to have the following:
  - 1:0
  - 0:1
  - 0:0

- 1:N
  - 1:0..n
  - 1:1..n
  - Note you can not enforce n via schema alone.

## N:M decomposition

Any N:M relationship can be decomposed into two 1:N relations but this loses some of essential information as the two relations are independent of each other so each side of the relation only represents half of the picture.  The relationship should be represented as link table to keep the entire relation as one cohesive object.

## Connections Traps

These are traps caused by not fully understanding the the meaning of a relationship.  All relationships should be fully understood in terms of their domain purpose.

## Fan trap

Two or more relationships occurrences of the same type fan out from the same entity.

- many:1/1:many

## Chasm trap

Where the ERD suggest a path but it is not always present for each occurrence.

- Division -< Employee >- Department

## Entity relation model

- A set of full normalised tables.

- 1:1 Relationships the entities should be mapped to the same table if membership is obligatory otherwise they should be in separated out into different tables.  If neither entity is obligatory the relationship should be represented in a table.

- 1:M non-obligatory the relationship should be mapped to a table that represents the relationship otherwise the relationship should be mapped to a table.

- N:M should be mapped to a table.