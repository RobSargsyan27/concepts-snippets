# Kimball’s methodology

One of the ways to design a data warehouse is to follow `Kimball’s methodology` which includes several steps:

## **Project Planning**

- **Identify Scope and Requirements:** Understand and document business objectives, business processes, and data availability.
- **Assess Feasibility:** Evaluate technical and resource feasibility.
- **Prepare Project Plan:** Define the overall strategy, scope, deliverables, timelines, and roles.

## **Requirements Definition**

- **Gather Requirements:** Engage closely with business users to capture reporting and analytical needs.
- **Define Business Questions and Metrics:** Document critical business questions, dimensions, facts, and measures clearly.

## **Dimensional Modeling**

- **Develop Star Schema Designs:** Define fact tables (measures) and dimension tables (descriptive attributes) to support analytical queries.
- **Design Conformed Dimensions:** Ensure dimensions are reusable across various subject areas to ensure consistency (conformed dimensions).

## **Physical Design**

- **Establish Technical Infrastructure:** Choose platforms, database systems, indexing strategies, and storage management.
- **Create Physical Schemas:** Define indexes, partitions, and aggregations to ensure performance.

## **Data Warehouse ETL Design and Development**

- **Extract:** Identify and extract source data.
- **Transform:** Cleanse, integrate, aggregate, and restructure data.
- **Load:** Populate the dimensional schema, including incremental updates.

## **Deployment**

- **Install and Configure:** Implement the dimensional models, ETL processes, and reporting tools.
- **User Acceptance Testing (UAT):** Validate results with end-users and iterate until acceptance criteria are met.
- **Deploy to Production:** Move systems and processes into production, train users, and establish support mechanisms.

## **Maintenance and Growth**

- **Monitor and Optimize:** Continuously track system performance, usage patterns, and user satisfaction.
- **Manage Change and Expansion:** Incrementally add new dimensions, facts, or subject areas as business needs evolve.

## Phases

The Kimball’s methodology is business-oriented. All set of steps can be divided into three main phases. 

1. Identifying requirements and scope. Checking resources feasibility.
2. Database modeling. ETL desing.
3. Deployment and maintenance.  

### Phase 1: Identify Requirements and Scope & Check Resources Feasibility

During the first phase the following action should be done:

- **Stakeholders identification**: The stakeholders are everyone who are involved in data warehousing project. The usual set of stakeholders include: business-oriented people, IT staff, end-users, executives.
- **Interviews and workshops**: Both interviews and workshops aim to reveal stakeholders’ functional and non-functional requirements and understand their needs from the data warehousing project. By asking right questions, one can understand the scope of the data that needs to be included in data warehousing.
- **Data analysis**: Identify data sources (both internal and external), check data consistency and feasibility, check how different data entities relate to each other.
- **Prototyping and mockups**: Create a small demo-able version with UI and mock data. This is required to make sure that the proposed solution fits with functional and non-functional requirements.
- **Documentation**: Document gathered functional and non-functional requirements. The documentation aims to provide shared understanding between all involved parties.
- **Validation and sing-off**: Review the gathered requirements with stakeholders to make sure that they align with business requirements and objectives.
- **Iterative process**: The requirements can be changed or added during the development and maintenance of data warehousing project. The above mentioned steps should be considered in such cases.
