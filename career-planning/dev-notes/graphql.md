___
### Graph QL
___

API Driven Design (web APIs)
- design from consumer pov (how will they use it, what do they have and what will they want to get?)
  - For example, which id should we return?
- schema first design helps with this

Practical Design tips
Entities:
- small, clearly defined
- real-world, or abstract
  - user, account, transaction, merchant
- break sub-things apart into own entities
  - ```user -> account```

Input types:
- every mutation should take a single input, return a single response object 
  - ```<MutationName>Response```
  - ```<MutationName>Input```

- FOr extensibility, allows you to add metadata later on

Extending Types
- Part of federation
- directives 
  - @key and @external
  - extend type
- add whatever properties and ub-objects you want to the extended type
- you can only extend a type once per service (recently changed in new version of Apollo Server/FEderation)

Nesting
- Nest a sub-object where there is a relationship
- replace foreign key with the actual object
  - foreign keys and data normalization are database specific concerns
- don't nest if relationship is already in graph
  - avoid cycles

Top-level queries
- what are useful entry points into your graph?
  - things that will be asked for independently
    - user, merchant, cardprogram

Edges
- can contain data about relationship, metadata
- model type of connection
  - Person -> follows -> Person B

Pagination
- use cursors

Designing the API for Performance

N+1 Data Loader Problem
- Resolver functions build in the data for a section
- Every user has an account, will call 100 accounts
- User data loader, n+1 to 2 queries
- To do a simple query, 
<span style="color:#fbb195; font-weight:900">Schemas</span>
<ul>
<li style="color:#f67280">Allow clients to know which <i>operations</i> can be performed by the server</li>
<li style="color:#f67280">Schema definition language (SDL)</li>
<li style="color:#f67280">Root types are the <code>query</code> type (mandatory), <code>mutation</code> type, and <code>subscription</code> type</li>
<li style="color:#f67280">We can define custom types in addition to our <code>Int, Float, Boolean, String, and ID</code></li>
</ul>

<span style="color:#fbb195; font-weight:900">Resolvers</span>
<ul>
<li style="color:#f67280">Define a resolver function for every field in the schema</li>
<li style="color:#f67280">Transforms the query from the client into actual result, moving through every field in the schema, and executing the “resolver” function to determine its result.</li>
<li style="color:#f67280"><code>function (root, args, context, info) { // function implementation }</code></li>
<ol>
<li><code>root</code> = "parent", contains result of previous resolver in call chain</li>
<li><code>args</code>: arguments provided to the field in the GraphQL query</li>
<li><code>context</code>: an object that every resolver can read from or write to</li>
<li><code>info</code></li>
</ol>
<li style="color:#f67280">We can define custom types in addition to our <code>Int, Float, Boolean, String, and ID</code></li>
</ul>

<span style="color:#fbb195; font-weight:900">Server</span>
<ul>
<li style="color:#f67280">import <code>GraphQLServer</code> to start <code>localhost:4000</code></li>
<li style="color:#f67280">needs <code>TypeDefs</code> (initialize/structure data), resolvers (how to get/filter data)</li>
</ul>

```
import { GraphQLServer } from 'graphql-yoga'

const typeDefs = ``; // Our definitions need to be in template literals

const resolvers = {};

const server = new GraphQLServer({ typeDefs, resolvers }) ;

server.start(() => console.log('server running'));
```