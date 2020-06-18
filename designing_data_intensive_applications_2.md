---


---

<h1 id="designing-intensive-data-applications---2">Designing Intensive Data Applications - 2</h1>
<p>By toki</p>
<h2 id="table-of-contents">Table of contents</h2>
<ul>
<li><a href="#chapter-2-data-models-and-query-languages">Chapter 2: Data Models and Query Languages</a>
<ul>
<li><a href="#relational-model-vs-document-model">Relational Model vs Document Model</a></li>
<li><a href="#query-languages-for-data">Query Languages for Data</a></li>
<li><a href="#graph-models">Graph Models</a></li>
</ul>
</li>
<li><a href="#extra">Extra</a></li>
<li><a href="#references">References</a></li>
</ul>
<h2 id="chapter-2-data-models-and-query-languages">Chapter 2: Data Models and Query Languages</h2>
<h2 id="relational-model-vs-document-model">Relational Model vs Document Model</h2>
<h3 id="for-a-data-structure-like-a-résumé-which-is-the-most-appropiate-representation-of-information-and-why">1. For a data structure like a <em>résumé</em>, which is the most appropiate representation of information and why?</h3>
<blockquote>
<p>A <em>résumé</em> is mostly a self-contained document, a JSON representation<br>
can be quite appropriate. Let’s see an example:</p>
</blockquote>
<pre class=" language-json"><code class="prism  language-json"><span class="token punctuation">{</span><span class="token string">"user_id"</span><span class="token punctuation">:</span> <span class="token number">251</span><span class="token punctuation">,</span> 
<span class="token string">"first_name"</span><span class="token punctuation">:</span> <span class="token string">"Bill"</span><span class="token punctuation">,</span> 
<span class="token string">"last_name"</span><span class="token punctuation">:</span> <span class="token string">"Gates"</span><span class="token punctuation">,</span> 
<span class="token string">"summary"</span><span class="token punctuation">:</span> <span class="token string">"Co-chair of the Bill &amp; Melinda Gates... Active blogger."</span><span class="token punctuation">,</span> 
<span class="token string">"region_id"</span><span class="token punctuation">:</span> <span class="token string">"us:91"</span><span class="token punctuation">,</span> 
<span class="token string">"industry_id"</span><span class="token punctuation">:</span> <span class="token number">131</span><span class="token punctuation">,</span> 
<span class="token string">"photo_url"</span><span class="token punctuation">:</span> <span class="token string">"/p/7/000/253/05b/308dd6e.jpg"</span><span class="token punctuation">,</span>
<span class="token string">"positions"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> 
	<span class="token punctuation">{</span><span class="token string">"job_title"</span><span class="token punctuation">:</span> <span class="token string">"Co-chair"</span><span class="token punctuation">,</span> <span class="token string">"organization"</span><span class="token punctuation">:</span> <span class="token string">"Bill &amp; Melinda Gates Foundation"</span><span class="token punctuation">}</span><span class="token punctuation">,</span> 
	<span class="token punctuation">{</span><span class="token string">"job_title"</span><span class="token punctuation">:</span> <span class="token string">"Co-founder, Chairman"</span><span class="token punctuation">,</span> <span class="token string">"organization"</span><span class="token punctuation">:</span> <span class="token string">"Microsoft"</span><span class="token punctuation">}</span>
<span class="token punctuation">]</span><span class="token punctuation">,</span>
<span class="token string">"education"</span><span class="token punctuation">:</span> <span class="token punctuation">[</span> 
	<span class="token punctuation">{</span><span class="token string">"school_name"</span><span class="token punctuation">:</span> <span class="token string">"Harvard University"</span><span class="token punctuation">,</span> <span class="token string">"start"</span><span class="token punctuation">:</span> <span class="token number">1973</span><span class="token punctuation">,</span> <span class="token string">"end"</span><span class="token punctuation">:</span> <span class="token number">1975</span><span class="token punctuation">}</span><span class="token punctuation">,</span>
	<span class="token punctuation">{</span><span class="token string">"school_name"</span><span class="token punctuation">:</span> <span class="token string">"Lakeside School, Seattle"</span><span class="token punctuation">,</span> <span class="token string">"start"</span><span class="token punctuation">:</span> <span class="token keyword">null</span><span class="token punctuation">,</span> <span class="token string">"end"</span><span class="token punctuation">:</span> <span class="token keyword">null</span><span class="token punctuation">}</span> 
<span class="token punctuation">]</span><span class="token punctuation">,</span> 
<span class="token string">"contact_info"</span><span class="token punctuation">:</span> <span class="token punctuation">{</span>
	<span class="token string">"blog"</span><span class="token punctuation">:</span> <span class="token string">"http://thegatesnotes.com"</span><span class="token punctuation">,</span>
	<span class="token string">"twitter"</span><span class="token punctuation">:</span> <span class="token string">"http://twitter.com/BillGates"</span> <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>The JSON representation has better locality than the multi-table<br>
schema RDMS. If you want to fetch a profile in the relational example,<br>
you need to either perform multiple queries (query each table by<br>
user_id) or perform a messy multiway join between the users table and<br>
its subordinate tables. In the JSON representation, all the relevant<br>
information is in one place, and one query is sufficient.</p>
</blockquote>
<h3 id="how-do-we-express-hierarchy-in-document-models">2. How do we express hierarchy in document models?</h3>
<blockquote>
<p>Document databases reverted back to the hierarchical model in one<br>
aspect: storing nested records (one-to-many relationships, like<br>
positions, education, and contact_info) within their parent record<br>
rather than in a separate table.</p>
<p>Relational and document databases are not fundamentally different: in<br>
both cases, the related item is referenced by a unique identifier,<br>
which is called a <em>foreign key</em> in the relational model and a<br>
<em>document reference</em> in the document model.</p>
</blockquote>
<h3 id="list-the-pros-of-document-and-relational-models">3. List the pros of document and relational models</h3>
<ul>
<li>Document
<ul>
<li>Schema flexibility</li>
<li>Better performance due to locality</li>
<li>For some applications it is closer to the data structures used by the application</li>
</ul>
</li>
<li>Relational
<ul>
<li>Better support for joins</li>
<li>Many-to-one and many-to-many relationships</li>
</ul>
</li>
</ul>
<h3 id="which-data-model-leads-to-simpler-application-code">4. Which data model leads to simpler application code?</h3>
<blockquote>
<p>If the data in your application has a document-like structure (i.e., a<br>
tree of <strong>one-to many relationships</strong>, where typically the entire tree<br>
is loaded at once), then it’s probably a good idea to use a document<br>
model.</p>
<p>If your application does use <strong>many-to-many relationships</strong>, the<br>
document model becomes less appealing.</p>
<p>It’s not possible to say in general which data model leads to simpler<br>
application code; it depends on the kinds of relationships that exist<br>
between data items. For <strong>highly interconnected data</strong>, the document<br>
model is awkward, the relational model is acceptable, and graph models<br>
are the most natural.</p>
</blockquote>
<h3 id="imagine-you-want-to-change-the-format-of-your-data-in-a-given-application.-for-example-say-you-are-currently-storing-each-user’s-full-name-in-one-field-and-you-instead-want-to-store-the-first-name-and-last-name-separately.-how-would-yo-do-it-in-both-document-and-relational-schema">5. Imagine you want to change the format of your data in a given application. For example, say you are currently storing each user’s full name in one field, and you instead want to store the first name and last name separately. How would yo do it, in both, document and relational schema?</h3>
<blockquote>
<p>In a document database, you would just start writing new documents<br>
with the new fields and have code in the application that handles the<br>
case when old documents are read. For example:</p>
</blockquote>
<pre class=" language-json"><code class="prism  language-json"><span class="token keyword">if</span> <span class="token punctuation">(</span>user <span class="token operator">&amp;&amp;</span> user<span class="token punctuation">.</span>name <span class="token operator">&amp;&amp;</span> <span class="token operator">!</span>user<span class="token punctuation">.</span>first_name<span class="token punctuation">)</span> 
<span class="token punctuation">{</span> 
<span class="token comment">// Documents written before Dec 8, 2013 don't have first_name </span>
user<span class="token punctuation">.</span>first_name <span class="token operator">=</span> user<span class="token punctuation">.</span>name<span class="token punctuation">.</span><span class="token function">split</span><span class="token punctuation">(</span><span class="token string">" "</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span><span class="token punctuation">;</span> 
<span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>On the other hand, in a “statically typed” database schema, you would<br>
typically per‐ form a migration along the lines of:</p>
</blockquote>
<pre class=" language-sql"><code class="prism  language-sql"><span class="token keyword">ALTER</span> <span class="token keyword">TABLE</span> users <span class="token keyword">ADD</span> <span class="token keyword">COLUMN</span> first_name <span class="token keyword">text</span><span class="token punctuation">;</span> 
<span class="token keyword">UPDATE</span> users <span class="token keyword">SET</span> first_name <span class="token operator">=</span> split_part<span class="token punctuation">(</span>name<span class="token punctuation">,</span> <span class="token string">' '</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">-- PostgreSQL </span>
<span class="token keyword">UPDATE</span> users <span class="token keyword">SET</span> first_name <span class="token operator">=</span> substring_index<span class="token punctuation">(</span>name<span class="token punctuation">,</span> <span class="token string">' '</span><span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span> <span class="token comment">-- MySQL</span>
</code></pre>
<h3 id="how-do-different-database-services-manage-their-data-locality-for-queries">6. How do different database services manage their data <em>locality</em> for queries?</h3>

<table>
<thead>
<tr>
<th>Service</th>
<th>Management</th>
</tr>
</thead>
<tbody>
<tr>
<td>MongoDB</td>
<td>A document is usually stored as a single continuous string, encoded as JSON, XML, or a binary variant thereof (such as MongoDB’s BSON).</td>
</tr>
<tr>
<td>Google’s Spanner</td>
<td>Google’s Spanner database offers the same locality properties in a relational data model, by allowing the schema to declare that a table’s rows should be interleaved (nested) within a parent table</td>
</tr>
<tr>
<td>Oracle</td>
<td>Oracle allows the same, using a feature called multi-table index cluster tables</td>
</tr>
<tr>
<td>Cassandra &amp; HBase</td>
<td>The column-family concept in the Bigtable data model (used in Cassandra and HBase) has a similar purpose of managing locality</td>
</tr>
</tbody>
</table><h2 id="query-languages-for-data">Query languages for Data</h2>
<h3 id="create-an-imperative-query-to-get-the-movie-titles-directed-by-quentin-tarantino-that-where-released-after-2010-and-lasted-more-than-120-minutes.">7. Create an imperative query to get the movie titles directed by <em>Quentin Tarantino</em> that where released after 2010 and lasted more than 120 minutes.</h3>
<pre class=" language-json"><code class="prism  language-json"><span class="token keyword">function</span> <span class="token function">GetTitles</span><span class="token punctuation">(</span>movies<span class="token punctuation">)</span> <span class="token punctuation">{</span>
 <span class="token keyword">var</span> TarantinoMovies <span class="token operator">=</span> <span class="token punctuation">[</span><span class="token punctuation">]</span><span class="token punctuation">;</span>
 <span class="token keyword">for</span> <span class="token punctuation">(</span><span class="token keyword">var</span> i <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">,</span> i <span class="token operator">&lt;</span> movies<span class="token punctuation">.</span>length<span class="token punctuation">,</span> i<span class="token operator">++</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
     <span class="token keyword">if</span> <span class="token punctuation">(</span>movies<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">.</span>director <span class="token operator">===</span> <span class="token string">"Quentin Tarantino"</span><span class="token punctuation">)</span> <span class="token operator">&amp;&amp;</span> <span class="token punctuation">(</span>movies<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">.</span>release_date <span class="token operator">&gt;</span> <span class="token number">2010</span><span class="token punctuation">)</span> <span class="token operator">&amp;&amp;</span> <span class="token punctuation">(</span>movies<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">.</span>duration <span class="token operator">&gt;</span> <span class="token number">120</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
        TarantinoMovies<span class="token punctuation">.</span><span class="token function">push</span><span class="token punctuation">(</span>movies<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
       <span class="token punctuation">}</span>
     <span class="token punctuation">}</span>
 <span class="token keyword">return</span> TarantinoMovies<span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="use-mongodbs-mapreduce-feature-to-get-a-report-of-how-many-yellow-volkswagen-you-have-seight-per-month">8. Use MongoDB’s MapReduce feature to get a report of how many yellow volkswagen you have seight per month</h3>
<pre class=" language-json"><code class="prism  language-json">db<span class="token punctuation">.</span>observations<span class="token punctuation">.</span><span class="token function">mapReduce</span><span class="token punctuation">(</span> 
	<span class="token keyword">function</span> <span class="token function">map</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">{</span> 
		<span class="token keyword">var</span> year <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>observationTimestamp<span class="token punctuation">.</span><span class="token function">getFullYear</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span> 	
		<span class="token keyword">var</span> month <span class="token operator">=</span> <span class="token keyword">this</span><span class="token punctuation">.</span>observationTimestamp<span class="token punctuation">.</span><span class="token function">getMonth</span><span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token operator">+</span> <span class="token number">1</span><span class="token punctuation">;</span> 
		<span class="token function">emit</span><span class="token punctuation">(</span>year <span class="token operator">+</span> <span class="token string">"-"</span> <span class="token operator">+</span> month<span class="token punctuation">,</span> <span class="token keyword">this</span><span class="token punctuation">.</span>numYellowVolkswagens<span class="token punctuation">)</span><span class="token punctuation">;</span> 
		<span class="token punctuation">}</span><span class="token punctuation">,</span> 
	<span class="token keyword">function</span> <span class="token function">reduce</span><span class="token punctuation">(</span>key<span class="token punctuation">,</span> values<span class="token punctuation">)</span> <span class="token punctuation">{</span> 
		<span class="token keyword">return</span> Array<span class="token punctuation">.</span><span class="token function">sum</span><span class="token punctuation">(</span>values<span class="token punctuation">)</span><span class="token punctuation">;</span> 
	<span class="token punctuation">}</span><span class="token punctuation">,</span> 
	<span class="token punctuation">{</span> 
		query<span class="token punctuation">:</span> <span class="token punctuation">{</span> type<span class="token punctuation">:</span> <span class="token string">"YellowVolkswagens"</span> <span class="token punctuation">}</span><span class="token punctuation">,</span> 
		out<span class="token punctuation">:</span> <span class="token string">"MonthlyYellowVolkswagensReport"</span> 
	<span class="token punctuation">}</span>
<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h2 id="graph-models">Graph models</h2>
<h3 id="describe-a-graph">9. Describe a graph</h3>
<blockquote>
<p>A graph consists of two kinds of objects: <em>vertices</em> (also known as<br>
nodes or entities) and <em>edges</em> (also known as relationships or arcs).<br>
Many kinds of data can be modeled as a graph. Typical examples<br>
include:</p>
<ul>
<li>Social graphs</li>
<li>Web graphs</li>
<li>Road networks</li>
</ul>
</blockquote>
<h3 id="list-the-proporties-of-vertices-and-edges">10. List the proporties of vertices and edges</h3>

<table>
<thead>
<tr>
<th>Vertex</th>
<th>Edges</th>
</tr>
</thead>
<tbody>
<tr>
<td>Each vertex consists of: a unique identifier, a set of outgoing edges, a set of incoming edges, a collection of properties (key-value pairs).</td>
<td>Each edge consists of: a unique identifier, the vertex at which the edge starts (the tail vertex), the vertex at which the edge ends (the head vertex), a label to describe the kind of relationship between the two vertices, a collection of properties (key-value pairs)</td>
</tr>
</tbody>
</table><h3 id="investigate-3-graph-databases">11. Investigate 3 graph databases</h3>

<table>
<thead>
<tr>
<th>Database</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>ArangoDB</td>
<td>NoSQL native multi-model database system developed by ArangoDB Inc. The database system supports three important data models (key/value, documents, graphs) with one database core and a unified query language called AQL (ArangoDB Query Language).</td>
</tr>
<tr>
<td>Neo4j</td>
<td>Open-source, supports ACID, has high-availability clustering for enterprise deployments, and comes with a web-based administration that includes full transaction support and visual node-link graph explorer</td>
</tr>
<tr>
<td>OrientDB</td>
<td>Second-generation distributed graph database with the flexibility of documents in one product (i.e., it is both a graph database and a document NoSQL database).</td>
</tr>
</tbody>
</table><h3 id="investigate-5-graph-query-languages">12. Investigate 5 graph query languages</h3>
<blockquote>
<ul>
<li><strong>Declarative</strong> 	* <strong>Cypher:</strong> Cypher is a declarative query language for property graphs, created for the Neo4j graph database. 	*<br>
<strong>SPARQL:</strong> Pronounced sparkle is an RDF query language—that is, a semantic query language for  databases—able to retrieve and manipulate<br>
data stored in Resource Description Framework (RDF) format. 	*<br>
<strong>Datalog:</strong> Datalog is a declarative logic programming language that syntactically is a subset of Prolog. It is often used as a query<br>
language for deductive databases. In recent years, Datalog has found<br>
new application in data integration, information extraction,<br>
networking, program analysis, security, cloud computing and machine<br>
learning.</li>
<li><strong>Imperative</strong> 	* <strong>Gremlin:</strong> Gremlin is a graph traversal language and virtual machine developed by Apache TinkerPop of the Apache<br>
Software Foundation. Gremlin works for both OLTP-based graph databases<br>
as well as OLAP-based graph processors.</li>
</ul>
</blockquote>
<h2 id="references">References</h2>
<p>Kleppmann, M. (2017). <em>Designing Data Intensive Applications: Part 1. Foundations of Data Systems: Reliable, Scalable, and Maintainable Applications</em>. O’Reilly, 1st Edition. Pp. 27-63</p>
<p>Rodriguez, M.A., “<a href="https://arxiv.org/abs/1508.03843">The Gremlin Graph Traversal Machine and Language</a>,” Proceedings of the ACM Database Programming Languages Conference, October, 2015.</p>
<p><a href="http://www.w3.org/TR/rdf-sparql-query/">SPARQL 1.0 Query language</a></p>
<p>Huang, Green, and Loo, “Datalog and Emerging applications”,  <a href="http://www.cs.ucdavis.edu/~green/papers/sigmod906t-huang.pdf"><em>SIGMOD 2011</em></a>  (PDF), UC Davis.</p>
<p>Leonard, Anghel (2013). <em>Pro Hibernate and MongoDB</em>. Apress. p. 384. <a href="https://en.wikipedia.org/wiki/ISBN_(identifier)" title="ISBN (identifier)">ISBN</a>  <a href="https://en.wikipedia.org/wiki/Special:BookSources/9781430257943" title="Special:BookSources/9781430257943">9781430257943</a> – via Google Play. Multimodel databases (…, ArangoDB and more)</p>

