___
### Graph QL
___


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