# Model Relationships in MongoDB

# Table of Contents

- [Embedding](#embedding)
- [References](#references)
- [Linking Method by Scenario](#linking-method-by-scenario)
- [Denormalizing Entities in Referenced Relationships](#denormalizing-entities-in-referenced-relationships)

# Embedding

## One-To-One Relationship

In MongoDB there are two main ways to define relationship between two documents (aka entities). We can either embed
child entity to parent entity or reference first entity from the second entity. The choice between embedding and
referencing entity depends on number of factors which we are going to discuss below.

**Embedding document can be beneficial for a number of reasons:**

- Single read query to get target document with all related data, thus no joins.
- Storing entities that have aggregate relationship together.

Generally, storing entities that needs to be fetched together, or needs to be updated/created together is considered a
good practice. It simplifies the database schema as well as enhances performance.
From my own experience, I can tell, that if the entities have a parent-child relationship but there is no strong
connection between them, so that they aren’t necessarily being updated together, or there are such use cases when we
need only parent node without child node, or the document can at some point exceed the document size limit, or there is
a situation when the child’s parent can be changed, it is considered to store child entities in a separate collection.

## One-To-Many Relationship

MongoDB official documentation suggests that it is fine to have embedded array of child entities within parent entity.
For example, country containing array of cities, authors contains array of books. Documentation provides the same
advantages as for one-to-one relationship, but overlooks disadvantages of this method.

Here is the real difference between relational and non-relational databases, as we are now forced to choose between
referencing and embedding. From my personal experience, I can tell that having embedded array of child entities is
beneficial rarely. It can be considered beneficial only when:

- We have a implicit limit on the size of array
- When the embedded array is only being fetched with parent entity.
- The embedded array’s entity is small and doesn’t contain any embedded array or object
- According to the use cases, there is no complex aggregate pipeline that depends on array’s entities. (In some cases,
  this may not work as MongoDB not only good at handling queries on embedded array entities.)

# References

## One-To-One Relationship

References can be beneficial over embedding in cases when the entity is being references from multiple documents. In
such cases if we embed the entity, then we need to update it in multiple places which is not efficient and can introduce
data corruption. Additionally as mentioned earlier it can be useful when the array doesn’t have any size limits and can
lead to document size exceed error.

# Linking Method by Scenario

| **Scenario**                                                                                        | **Linking Method** |
|-----------------------------------------------------------------------------------------------------|--------------------|
| Keeping related data together will lead to a simpler data model and code.                           | Embedding          |
| You have a "has-a" or "contains" relationship between entities.                                     | Embedding          |
| Your application queries pieces of information together.                                            | Embedding          |
| You have data that's often updated together, i.e entities lifecycle is coupled                      | Embedding          |
| You have data that should be archived at the same time.                                             | Embedding          |
| The child side of the relationship doesn’t make sense without parent node                           | Embedding          |
| Data duplication is too complicated to manage and not preferred.                                    | Referencing        |
| The combined size of your data takes up too much memory or transfer bandwidth for your application. | Referencing        |
| Your embedded data grows without bounds.                                                            | Referencing        |
| Your data is written at different times in a write-heavy workload.                                  | Referencing        |
| For the child side of the relationship, your data can exist by itself without a parent.             | Referencing        |
| The child side is being independently queried a lot                                                 | Referencing        |
| The child side is being written frequently in comparison with parent side                           | Referencing        |
| The child can have multiple parents                                                                 | Referencing        |
| The child nodes needs to support extensive search with pagination                                   | Referencing        |
| The child nodes are being updated repeatedly                                                        | Referencing        |

**Note:** The child node doesn’t make sense only if one field is required from parent node then it is better to consider
denormalization and referencing rather than embedding.

# Denormalizing Entities in Referenced Relationships

Entities relationship denormalization is an important aspect for the database modeling especially when those entities
are get query heavy. For example, we will take a real-word use case where we want to have a predefined list of
task-titles and we want to map them with tasks in one-to-many relationship respectively. The answer may be
straightforward - just store the foreign key in task entity, but in this case we are limiting are querying performance.
In case somebody wants to get all tasks there title is “ABC”, then we need to do an additional JOIN or match in
aggregate pipeline (in case of MongoDB) to get the desired result. In most cases additional JOIN drops the performance
of the query significantly. Better to store both the titleId and titleText in task entity.

This may seem counterintuitive but it actually makes sense. First thing first, there is no additional JOIN for get
queries, secondly the users now can update the title, thus we also support free form text and finally, in case someone
updates the task-title we can bulk update the tasks using the titleId. So this hybrid approach get the best from both
worlds. This pattern is especially useful when you want to support both custom field and predefined field for the same
notion.
