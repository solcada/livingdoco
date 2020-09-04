# Living documentation
A guide to organisational information management using a Living Documentation approach

If you would like to support the on-going development of this, [please buy me a coffee](https://www.buymeacoffee.com/andrewd)
<br />

pages 

##  Problem Definition
Every organization has to answer the question how do we document and support our IT systems?

At most companies you find a disorganized, duplicated, dis-joined, incoherent, semi complete, inaccessible, unmaintained wasteland of share-point sites, word documents, tooling, and wiki's.

This usually occurs because of the nature of projects (which is related to the nature of funding models).  Documentation is created through the viewpoint of a project, then once the project closes the maintenance of the documentation ceases.  It's also the effect of 'working' documentation i.e processing required to analyze and design a discrete problem being used for on going support documentation.

The other main issue is the incoherence of documentation. For example different users will refer to different truths and granularity of documentation, example architects referring to EA Sparx, Developers referring to code, Business Analysts to word documents.

The answer is to create minimum, coherent, up to date documentation that anyone can access and amend. Most importantly it needs to follow a 'meta-model' which can describe IT systems, scale, and be at a level of granularity which is useful to architects, developers, testers, operations.

Where this can go wrong, is when a an untested, arbitrary information classification approach is used. If you create a structure that does not classify information in a useful, obvious, discoverable and scalable way, users will follow the path of least resistance. They won't use it because they can't find the information and instead will create their own silo's of 'notes'. They will put information where ever they can, because the convention used is not intuitive or doesn't allow for some deviation.

One of the approaches I have used for the last 10 years is what I refer to as ‘living documentation’.  It works well, scales across an organisation and provides a gradual, concise way to document an organisation and its IT systems.

To document, model and support systems at an organisation the meta-model needs to address the audience (business analysis, developer, operations, architecture), it also needs to be at the right level of granularity (too detailed and it is technical noise, too high level and it is abstract waffle).  Below is a convention based approach to documenting the IT systems at a company.

Below is a tutorial of how to categorize and structure your organizations IT information using the Living Documentation approach I have been using since 2010.  Each section will have a 'rationale' link to help explain why the approach works.


 ## Part 1 - Inventory your applications and their dependencies

### Defining your applications
We are going to define an Application as something which is running in an environment and can be packaged and versioned.  

For example your organizations website, its API's, its intranet.  Don't worry if you get this wrong, Living Documentation is iterative and we will add more criteria to help define what an Application is.

Start anywhere and capture a list of Applications at your organisation.
- Company Website
- Payments API
- Customers API
- Products API

We are not defining as an application are 
- Things which are not installed or actively running (i.e software sitting on a shelf)
- Commodity software which has not been modified (e.g Microsoft word, excel).  This is noise which can be captured in a CMDB.

Once we have a rough list of your companies application, we will create pages for them under an Applications section.

Each page should contain the following, which we will add to

#### Overview 
A description of the Application which contains a business defintion so that it is useful for all audiences.

#### Used by
A link to other Application pages which consume this application.  e.g Customers API is used by the Company Website

#### Depends On
A link to other Application pages which this application depends on.  e.g Company Website depends on the Customer API

#### Source Control
A link to the source control repository of this application.

Repeat step 1 with every important application you interact with and this will build up a graph of dependencies within your organisation.  This is called discoverable documentation.  You can land anywhere in the organisations graph and have visibility of the dependencies between applications.

The rule is, if something is significant then document it, if it is not significant, dont.
 

 ## Part 2 - Define your logical environments

In most companies you have multiple different environments for the purposes of development and testing.  In an ideal world every environment has minimal deviation and propagating a feature through environments is timely.  However, most companies have budget constraints, complexity constraints and release speed constraints.  This results in the need for different environments which serve different purposes such as Quality Assurance, Integration and Development.

Here is a template for defining a few Logical environments and their purposes.

| Environment  | Local  | Development  | Test  | Quality Assurance  | Production | 
|---|---|---|---|---|--| 
| Definition | A physical or virtual machine which developers use to develop software on | A sub-set of Applications used for developing a feature | A environment which multiple teams use to integrate and test their features | A production like environment | Production (includes Disaster recovery) |
| Purpose  | Developing features | Integrating features with other components and teams  | Integrating features with other components and teams | Replicating production tests | Serving end users |
| Number of Instances  | 1> | 1> | 1> | 1 | 1 |
| Deviation from production  | Large (contains mocks, consolidated infrastructure, enabled feature toggles) | Large | Medium | Small | N/A |
| Users  | Developers | Developers and 3rd parites | Developers and Testers | Testers and the business | End Users |
| Rebuild frequency | 2 Weekly | 2 Weekly | 2 Weekly | 2 Weekly | N/A |

Note: Trunk based development and feature toggles removes the need for proliferation of environments and environment contention.

If you haven't done a excerise to define your logical environments you can use the template above.  

The importance in defining your logical environments is because by not doing it, developers, testers and the business will be tempted to utilize an environment for the wrong purpose.  Such as giving a 3rd party access to a Quality Assurance environment that has sensitive data or having a developer run 1/2 completed features against test environment introducing data corruption and false negative defects.
 
The reason these are separated from physical environments is that it is possible to have.

## Part 3 - Inventory your Databases

Your organisation is going to have systems of records.  They are databases, fileshares, blob storage where information is persisted.
Start anywhere and capture a list of Databases at your organisation.

We are capturing logical definitions only.  You might have 1 or more copies of database in each one of your logical environments but we only need to document it once.

#### Overview 
A description of the Database which contains a business defintion so that it is useful for all audiences.

#### Used by
A link to other Application pages which consume this Database.  e.g The Customers Database is used by the Customers API

#### Source Control
If this Database is managed through source control is versioned when changes are deployed, link to the repository that contains the changes.

## Part 4 - Define your physical environments and versioning your software

Now that we have a logical map of some of our Applications we can start documenting their physical deployments.  Operations, Developers, Testers and Architects are interesting in the infrastructure.

We take the previous matrix we have done and manually enter the applications, their versions, their DNS / urls.  In the future we will derive this information automatically.

#### Environment Matrix

| Application  | Local  | Development  | Test  | Quality Assurance  | Production | 
|---|---|---|---|---|--| 
| Company Website | local.company.com | dev.company.com | test.company.com | qa.company.com | www.company.com | 
| Customers API | local.customers-api.company.com | dev.customers-api.company.com | test.customers-api.company.com | qa.customers-api.company.com | customers-api.company.com | 
| Customers Database | local.customers-database.company.com | dev.customers-database.company.com | test.customers-database.company.com | qa.customers-database.company.com | customers-database.company.com |
| ... |  | |  |  |  | | 

#### Versions

| Application  | Local  | Development  | Test  | Quality Assurance  | Production | 
|---|---|---|---|---|--| 
| Company Website | 3.0.157.0 | 2.1.117.0 | 2.1.102.0 | 2.0.122.1 | 2.0.122.1 | 
| Customers API | 2.3.12.0 | 2.3.12.0 | 2.3.12.0 | 2.0.8.0 | 2.0.8.0 | 
| Customers Database | 4.2.99.4 | 4.3.1.4 | 4.1.0.1 | 4.1.0.1 | 4.1.0.1 | 
| ... |  | |  |  |  | | 

Note: DNS for databases / internal API's would only be available internally.

Physical environment documentation
Most cloud systems (Azure, AWS, Google Cloud) automatically provide an interface which describes your physical environment.  i.e all your servers, their IP addresses so it is redundant to documentat the same information.  What is important though is to map how those physical environments relate to your logical environments.  It is likely a few of your environments are breaking the rules and conventions.

#### Physical Envrionments

| Physical/Instance Environment  | Local  | Development  | Test  | Quality Assurance  | Production | 
|---|---|---|---|---|--| 
| Azure | AzureDev  | AzureSIT | AzureUAT | AzureUAT | AzureProd1, AzureDR | 
| | Local Machines |   | AWS-US-Dev02, AWS-US-Dev01(old) |  | AWS-US-Prd | 
| |  |   | Melbourne Datacenter (SIT)  |  | Melbourne Datacenter (PRD) | 

## Part  5 - Set up an Area for Issues, Risks and Decisions

"Until you make the unconscious conscious, it will direct your life and you will call it fate." - Carl Jung.

#### Issue log

You may already have a IT system for tracking issues, the key thing here is that you have a process and a scope for the the issues.  e.g project issues, product issues, organisational issues and that these issues are esclated if they are at a level where the person being affected by it, cannot resolve it.  Its important to have a clear owner for an issue.

| Id | Description  | Impact  | Issue Owner | Date | Status | Resolution | 
|---|---|---|---|---|--| 
| 1 | Bandwidth bottlenecks causing site to run slow | High | Stephen | 4-09-2020 | Open |  |

#### Risk log
| Id  | Description  | Probability | Impact | Prevent/Reduce/Accept/Transfer/Contingency | Risk Owner | 
|---|---|---|---|---|--|--|
|  ||||

#### Decision log

| Id | Decision |  | | | | 
|---|---|---|---|---|--| 
| 1 | 3.0.157.0 | 2.1.117.0 | 2.1.102.0 | 2.0.122.1 | 2.0.122.1 | 

Organisations and projects have the need to capture Issues, Risks and Decisions.

#### Advanced concepts
- Infrastructure as Code
- Database Refresh
- Environmenth Health
- Naming conventions, configuration and automation
- Visualizing dependencies
- Three main apporaches to documentation
- Documentation and Convention
- Interrogation and Reflection
- Projection
- Tagging your cloud instances
- Inferring your dependencies
- API Documentation (Swagger)