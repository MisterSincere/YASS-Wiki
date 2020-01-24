---


---

<h1 id="contents">Contents</h1>
<ol>
<li><a href="#1-introduction">Introduction</a></li>
<li><a href="#2-structure">Structure</a><br>
2.1. <a href="#21-staticfile-structure">Static/File Structure</a><br>
  2.1.1. <a href="#211-conventions">Conventions</a><br>
  2.1.2. <a href="#212-scene-manager">Scene Manager</a><br>
  2.1.3. <a href="#213-scene">Scene</a><br>
  2.1.4. <a href="#214-asset-database">Asset Database</a><br>
  2.1.5. <a href="#215-scene-nodes">Scene Nodes</a><br>
2.2. <a href="#22-loading-structure">Loading Structure</a><br>
  2.2.1. <a href="#221-asset-blackboxing">Asset Blackboxing</a><br>
  2.2.2. <a href="#222-loader-abstraction">Loader Abstraction</a><br>
  2.2.3. <a href="#223-consumer">Consumer</a><br>
  2.2.4. <a href="#224-factories">Factories</a><br>
  2.2.5. <a href="#225-scene-manager-loader">Scene Manager Loader</a><br>
  2.2.6. <a href="#226-scene-loader">Scene Loader</a><br>
  2.2.7. <a href="#227-asset-database-loader">Asset Database Loader</a></li>
<li><a href="#3-workflow">Workflow</a><br>
3.1. <a href="#31-namespace">Namespace</a><br>
3.2. <a href="#32-include-setup">Include Setup</a><br>
3.3. <a href="#33-loading-preparation">Loading: Preparation</a><br>
  3.3.1. <a href="#331-memory">Memory</a><br>
  3.3.2. <a href="#332-scene-manager-consumer">Scene Manager Consumer</a><br>
3.4. <a href="#34-loading-start">Loading: Start</a><br>
3.5. <a href="#35-cleanup">Cleanup</a></li>
<li><a href="#4-assets">Assets</a><br>
4.1. <a href="#41-expansion">Expansion</a><br>
4.2. <a href="#42-featured">Featured</a><br>
  4.2.1. <a href="#421-shaders">Shaders</a><br>
  4.2.2. <a href="#422-materials">Materials</a><br>
  4.2.3. <a href="#423-meshes">Meshes</a><br>
  4.2.4. <a href="#424-fonts">Fonts</a><br>
  4.2.5. <a href="#425-animations">Animations</a></li>
<li><a href="#5-attachments">Attachments</a><br>
5.1. <a href="#51-expansion">Expansion</a><br>
5.2. <a href="#52-core">Core</a><br>
  5.2.1. <a href="#521-renderable">Renderable</a><br>
  5.2.2. <a href="#522-relative-transform">Relative Transform</a><br>
5.3. <a href="#52-featured">Featured</a><br>
  5.2.1. <a href="#521-area-trigger">Area Trigger</a><br>
  5.2.2. <a href="#522-audio">Audio</a><br>
  5.2.3. <a href="#523-audio-cycler">Audio Cycler</a><br>
  5.2.4. <a href="#524-debug-details">Debug Details</a><br>
  5.2.5. <a href="#525-camera-dolly">Camera Dolly</a><br>
  5.2.6. <a href="#526-light-source">Light Source</a><br>
  5.2.7. <a href="#527-navmesh">Navmesh</a><br>
  5.2.8. <a href="#528-navmesh-agent">Navmesh Agent</a><br>
  5.2.9. <a href="#529-screen-fade">Screen Fade</a><br>
5.4. <a href="#54-text">Text</a><br>
  5.4.1. <a href="#541-text">Text</a><br>
  5.4.2. <a href="#542-cursor">Cursor</a></li>
<li><a href="#6-input">Input</a><br>
6.1. <a href="#61-generic-input-provider">Generic Input Provider</a><br>
6.2. <a href="#62-yaml-input">YAML Input</a></li>
<li><a href="#7-scripted-interactions">Scripted Interactions</a><br>
7.1. <a href="#71-the-message">The Message</a><br>
  7.1.1. <a href="#711-event-context">Event Context</a><br>
  7.1.2. <a href="#712-scene-context">Scene Context</a><br>
  7.1.3. <a href="#713-scene-manager-context">Scene Manager Context</a><br>
7.2. <a href="#72-declaration">Declaration</a><br>
7.3. <a href="#73-examples">Examples</a></li>
<li><a href="#8-console">Console</a><br>
8.1. <a href="#81-expansion">Expansion</a><br>
8.2. <a href="#82-featured-commands">Featured Commands</a></li>
</ol>
<p>​</p>
<p>​</p>
<h1 id="introduction">1. Introduction</h1>
<p>The <strong>YA</strong>ML-<strong>s</strong>cene-<strong>s</strong>tructured <strong>c</strong>ontent-<strong>we</strong>althy <strong>en</strong>vironment (YASS CWEEN) is a YAML- and JSON-( ͡° ͜ʖ ͡°)based entity-component system for the Gospel graphics library. It features an easily extendable attachment- and asset structure for dynamic virtual environments alongside an asynchronous content management system for reduced loadtime.</p>
<h1 id="structure">2. Structure</h1>
<p>In general a YASS CWEEN’s structure is divided into two different structures.</p>
<p>One is the <em>Static Structure</em> which is composed of components which hold either the scene data or do everything that is needed to manage a scene (e.g. scene swapping, ticking, time management). It is also coherent to the YAML file structure, meaning that there is one component for every file type and vice versa. This comes naturally with the fact that the files describe the content of the <em>Static Structure</em>. So the <em>File Structure</em> can be seen as a representation of the <em>Static Structure</em> to the user.</p>
<p>The <em>Loading Structure</em> on the other hand contains other components which read in the YAML-files and load the specified assets and scene structure. It can be said that the <em>Static Structure</em> is basically the result and description (<em>File Structure</em>) of the query, which is processed by the <em>Loading Structure</em>.</p>
<p>​</p>
<h2 id="staticfile-structure">2.1. Static/File structure</h2>
<p>At its core, a YASS CWEEN is composed of three different file formats:</p>
<ul>
<li>
<p>one <a href="#212-scene-manager"><em>Scene Manager</em></a></p>
</li>
<li>
<p>a collection of <a href="#214-asset-database"><em>Asset Databases</em></a></p>
</li>
<li>
<p>a collection of <a href="#213-scene"><em>Scenes</em></a></p>
</li>
</ul>
<p>​</p>
<p><img src="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/aefc2d812fe447cbf8ff0d3980a28211b0f3ca97/wiki/static_structure.png" alt="Static structure"></p>
<p>​</p>
<h3 id="conventions">2.1.1. Conventions</h3>
<p>Each YAML (or JSON) file is required to provide two pieces of meta information to ensure proper loading.</p>
<pre><code>
format: &lt;type&gt;

version: &lt;version&gt;

</code></pre>
<p>​</p>
<h3 id="scene-manager">2.1.2. Scene Manager</h3>
<p>The <em>Scene Manager</em> defines the start point of the specified application. It also maintains a <em>Shared Asset Database</em> that may be referred to by all Scenes it owns, except for the Loading Scene, which is initialized independently and is allowed to complete loading before the <em>Shared Asset Database</em>.</p>
<p>​</p>
<h4 id="format">Format</h4>
<p>The meta head for a <em>Scene Manager</em> is as follows:</p>
<pre><code>
format: SceneManager

version: 0.1  # Check current version here

</code></pre>
<p>The file body supports the following descriptors:</p>
<ul>
<li>
<p><code>shared-assets</code> is a <strong>list of paths</strong> to asset database files which are merged into the <em>Shared Asset Database</em> upon loading.</p>
</li>
<li>
<p><code>loading-scene-path</code> is the <strong>path</strong> to a scene file which is independent to the <em>Shared Asset Database</em>.</p>
</li>
<li>
<p><code>initial-scene</code> sets the <strong>name</strong> which scene should be activated immediately upon completion of its load process.</p>
</li>
<li>
<p><code>scenes</code> contains a <strong>set of scene definitions</strong> according to the following pattern:</p>
</li>
</ul>
<p><code>"&lt;scene-name&gt;": { "yaml-file": "&lt;path to scene yaml&gt;", "load-on-startup": "&lt;true/false&gt;" }</code></p>
<p>Where <code>load-on-startup</code> determines wether the scene should be loaded and the structure built upon startup (true), or upon first activation (false).</p>
<ul>
<li><code>inputs</code> see <a href="#62-yaml-input">YAML Input</a> for more details</li>
</ul>
<p>​</p>
<p>​</p>
<h3 id="scene">2.1.3. Scene</h3>
<p>A <em>Scene</em> file fully describes a cohesive environment comprised of different <em>Scene Nodes</em> and their properties/attachments. Each scene may also include a number of <em>Asset Databases</em> which are merged into its <em>Private Asset Database</em>. This database will pass all requests it cannot satisfy to the <em>Shared Asset Database</em>. Communication between different <em>Private Asset Database</em> is forbidden.</p>
<p><strong>Welcome</strong> to the Wiki of our YASS CWEEN.</p>
<h1 id="contents-1">Contents</h1>
<ol>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#1-introduction">Introduction</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#2-structure">Structure</a><br>
2.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#21-staticfile-structure">Static/File Structure</a><br>
  2.1.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#211-conventions">Conventions</a><br>
  2.1.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#212-scene-manager">Scene Manager</a><br>
  2.1.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#213-scene">Scene</a><br>
  2.1.4. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#214-asset-database">Asset Database</a><br>
  2.1.5. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#215-scene-nodes">Scene Nodes</a><br>
2.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#22-loading-structure">Loading Structure</a><br>
  2.2.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#221-asset-blackboxing">Asset Blackboxing</a><br>
  2.2.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#222-loader-abstraction">Loader Abstraction</a><br>
  2.2.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#223-consumer">Consumer</a><br>
  2.2.4. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#224-factories">Factories</a><br>
  2.2.5. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#225-scene-manager-loader">Scene Manager Loader</a><br>
  2.2.6. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#226-scene-loader">Scene Loader</a><br>
  2.2.7. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#227-asset-database-loader">Asset Database Loader</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#3-workflow">Workflow</a><br>
3.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#31-namespace">Namespace</a><br>
3.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#32-include-setup">Include Setup</a><br>
3.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#33-loading-preparation">Loading: Preparation</a><br>
  3.3.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#331-memory">Memory</a><br>
  3.3.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#332-scene-manager-consumer">Scene Manager Consumer</a><br>
3.4. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#34-loading-start">Loading: Start</a><br>
3.5. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#35-cleanup">Cleanup</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#4-assets">Assets</a><br>
4.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#41-expansion">Expansion</a><br>
4.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#42-featured">Featured</a><br>
  4.2.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#421-shaders">Shaders</a><br>
  4.2.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#422-materials">Materials</a><br>
  4.2.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#423-meshes">Meshes</a><br>
  4.2.4. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#424-fonts">Fonts</a><br>
  4.2.5. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#425-animations">Animations</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#5-attachments">Attachments</a><br>
5.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#51-expansion">Expansion</a><br>
5.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#52-core">Core</a><br>
  5.2.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#521-renderable">Renderable</a><br>
  5.2.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#522-relative-transform">Relative Transform</a><br>
5.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#52-featured">Featured</a><br>
  5.2.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#521-area-trigger">Area Trigger</a><br>
  5.2.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#522-audio">Audio</a><br>
  5.2.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#523-audio-cycler">Audio Cycler</a><br>
  5.2.4. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#524-debug-details">Debug Details</a><br>
  5.2.5. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#525-camera-dolly">Camera Dolly</a><br>
  5.2.6. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#526-light-source">Light Source</a><br>
  5.2.7. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#527-navmesh">Navmesh</a><br>
  5.2.8. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#528-navmesh-agent">Navmesh Agent</a><br>
  5.2.9. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#529-screen-fade">Screen Fade</a><br>
5.4. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#54-text">Text</a><br>
  5.4.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#541-text">Text</a><br>
  5.4.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#542-cursor">Cursor</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#6-input">Input</a><br>
6.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#61-generic-input-provider">Generic Input Provider</a><br>
6.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#62-yaml-input">YAML Input</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#7-scripted-interactions">Scripted Interactions</a><br>
7.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#71-the-message">The Message</a><br>
  7.1.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#711-event-context">Event Context</a><br>
  7.1.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#712-scene-context">Scene Context</a><br>
  7.1.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#713-scene-manager-context">Scene Manager Context</a><br>
7.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#72-declaration">Declaration</a><br>
7.3. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#73-examples">Examples</a></p>
</li>
<li>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#8-console">Console</a><br>
8.1. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#81-expansion">Expansion</a><br>
8.2. <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#82-featured-commands">Featured Commands</a></p>
</li>
</ol>
<h1 id="introduction-1">1. Introduction</h1>
<p>The <strong>YA</strong>ML-<strong>s</strong>cene-<strong>s</strong>tructured <strong>c</strong>ontent-<strong>we</strong>althy <strong>en</strong>vironment (YASS CWEEN) is a YAML- and JSON-( ͡° ͜ʖ ͡°)based entity-component system for the Gospel graphics library. It features an easily extendable attachment- and asset structure for dynamic virtual environments alongside an asynchronous content management system for reduced loadtime.</p>
<h1 id="structure-1">2. Structure</h1>
<p>In general a YASS CWEEN’s structure is divided into two different structures.<br>
One is the <em>Static Structure</em> which is composed of components which hold either the scene data or do everything that is needed to manage a scene (e.g. scene swapping, ticking, time management). It is also coherent to the YAML file structure, meaning that there is one component for every file type and vice versa. This comes naturally with the fact that the files describe the content of the <em>Static Structure</em>. So the <em>File Structure</em> can be seen as a representation of the <em>Static Structure</em> to the user.<br>
The <em>Loading Structure</em> on the other hand contains other components which read in the YAML-files and load the specified assets and scene structure. It can be said that the <em>Static Structure</em> is basically the result and description (<em>File Structure</em>) of the query, which is processed by the <em>Loading Structure</em>.</p>
<h2 id="staticfile-structure-1">2.1. Static/File structure</h2>
<p>At its core, a YASS CWEEN is composed of three different file formats:</p>
<ul>
<li>one <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#212-scene-manager"><em>Scene Manager</em></a></li>
<li>a collection of <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#214-asset-database"><em>Asset Databases</em></a></li>
<li>a collection of <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#213-scene"><em>Scenes</em></a></li>
</ul>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/aefc2d812fe447cbf8ff0d3980a28211b0f3ca97/wiki/static_structure.png"><img src="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/aefc2d812fe447cbf8ff0d3980a28211b0f3ca97/wiki/static_structure.png" alt="Static structure"></a></p>
<h3 id="conventions-1">2.1.1. Conventions</h3>
<p>Each YAML (or JSON) file is required to provide two pieces of meta information to ensure proper loading.</p>
<pre><code>format: &lt;type&gt;
version: &lt;version&gt;

</code></pre>
<h3 id="scene-manager-1">2.1.2. Scene Manager</h3>
<p>The <em>Scene Manager</em> defines the start point of the specified application. It also maintains a <em>Shared Asset Database</em> that may be referred to by all Scenes it owns, except for the Loading Scene, which is initialized independently and is allowed to complete loading before the <em>Shared Asset Database</em>.</p>
<h4 id="format-1">Format</h4>
<p>The meta head for a <em>Scene Manager</em> is as follows:</p>
<pre><code>format: SceneManager
version: 0.1  # Check current version here

</code></pre>
<p>The file body supports the following descriptors:</p>
<ul>
<li><code>shared-assets</code> is a <strong>list of paths</strong> to asset database files which are merged into the <em>Shared Asset Database</em> upon loading.</li>
<li><code>loading-scene-path</code> is the <strong>path</strong> to a scene file which is independent to the <em>Shared Asset Database</em>.</li>
<li><code>initial-scene</code> sets the <strong>name</strong> which scene should be activated immediately upon completion of its load process.</li>
<li><code>scenes</code> contains a <strong>set of scene definitions</strong> according to the following pattern:<br>
<code>"&lt;scene-name&gt;": { "yaml-file": "&lt;path to scene yaml&gt;", "load-on-startup": "&lt;true/false&gt;" }</code><br>
Where <code>load-on-startup</code> determines wether the scene should be loaded and the structure built upon startup (true), or upon first activation (false).</li>
<li><code>inputs</code> see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#62-yaml-input">YAML Input</a> for more details</li>
</ul>
<h3 id="scene-1">2.1.3. Scene</h3>
<p>A <em>Scene</em> file fully describes a cohesive environment comprised of different <em>Scene Nodes</em> and their properties/attachments. Each scene may also include a number of <em>Asset Databases</em> which are merged into its <em>Private Asset Database</em>. This database will pass all requests it cannot satisfy to the <em>Shared Asset Database</em>. Communication between different <em>Private Asset Database</em> is forbidden.</p>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/8599d4f1a527babc8d79079d756e1a6f97e97ad6/wiki/Scene.png"><img src="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/8599d4f1a527babc8d79079d756e1a6f97e97ad6/wiki/Scene.png" alt="Static structure"></a></p>
<h4 id="format-2">Format</h4>
<p>The meta head for a Scene is as follows:</p>
<pre><code>format: Scene
version: 0.1 #Check current version here

</code></pre>
<p>The Scene file consists of two top-level entries:</p>
<ul>
<li><code>assets</code> is a <strong>list of relative paths</strong> to asset database files which are merged into the Scene’s <code>Private Asset Database</code> upon loading.</li>
<li><code>scene-structure</code> provides a <strong>list of <em>Scene Nodes</em></strong></li>
</ul>
<h3 id="asset-database">2.1.4. Asset Database</h3>
<p>An <em>Asset Database</em> provides different types of assets to the scene files’ <code>scene-structure</code>. The assets can be accessed via an id. If an <em>Asset Database</em> is linked within a scene manager file its assets are <em>shared</em> available to all scenes of this <em>Scene Manager</em>. If it was linked within a scene file its assets are <em>private</em> and can only be accessed by that very <em>Scene</em>.<br>
The <em>Asset Database</em> manages by default:</p>
<ul>
<li><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#421-shaders">Shaders</a></li>
<li><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#422-materials">Materials</a></li>
<li><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#423-meshes">Meshes</a></li>
<li><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#424-fonts">Font Assets</a></li>
<li><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#425-animations">Animations</a> [[deprecated]]</li>
</ul>
<p>But it may be expanded to support any type of Asset (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home#41-featured">Asset Expansion</a>).</p>
<h4 id="format-3">Format</h4>
<p>The meta head of an <em>Asset Database</em> is as follows:</p>
<pre><code>format: AssetDatabase
version: 0.1  #Check current version here

</code></pre>
<p>Every entry in this format refers to one type of asset. Check out the <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#42-featured">default assets</a> in the Workflow chapter to get more informations about the default assets named above and the <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#41-expansion">Asset Expansion</a> section to learn how to add your own asset type.</p>
<h3 id="scene-nodes">2.1.5. Scene Nodes</h3>
<p>A <em>Scene Node</em> is defined within the <code>scene-structure</code> segment of a scene file as follows:</p>
<pre><code>"&lt;path&gt;/&lt;name&gt;":
    create-on-demand: "&lt;true/false&gt;"       # optional, default: true
    "&lt;attachment-type&gt;":
        &lt;properties&gt;
    &lt;...&gt;

</code></pre>
<p>Each <em>Scene Node</em> is named by the full path at which it should be placed in the <em>Scene</em>. The <em>Scene Node</em> itself is internally managed as an <code>Gospel::AdvancedSceneGraphNode</code>. If you want the <em>Scene Node</em> to render something you can specify under <code>mesh</code> the name of the mesh or shape or even down to the submesh (mesh.shape.submesh). <code>materials</code> can then hold a list of materials, where the first references the first mesh and so on. If more meshes than materials are defined, every next mesh will be using the last material defined in the list.<br>
Each object referenced in the path must have been created prior within the file: <code>parent/child</code> requires a preceding definition of <code>parent</code>.<br>
An <em>Attachment</em> is attached to its parent and open for any shenanigans you want to do with it. Some featured attachments are:</p>
<ul>
<li>Area Trigger</li>
<li>Audio</li>
<li>Audio Cycler</li>
<li>Debug Details</li>
<li>Camera Dolly</li>
<li>Light Source</li>
<li>Navmesh</li>
<li>Navmesh Agent</li>
<li>Screen Fade</li>
<li>Text</li>
</ul>
<h2 id="loading-structure">2.2. Loading structure</h2>
<p>The main thing of the <em>Loading Structure</em> is… well you guessed it: Loaders! First we will show you two classes that abstract the loaders. Since you will need to implement them when adding your own asset types. YASS CWEEN does implement the loaders for the <em>Scene Manager</em>, <em>Scene</em>, <em>Asset Database</em> and the default assets (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#42-featured">4.2</a>). The <code>SceneManagerLoader</code> is the one you will start by yourself and is then going to trigger:</p>
<ul>
<li><code>AssetDatabaseLoader</code> for every asset database file specified in <code>shared-assets</code> which will be merged into one asset database called the <em>Shared Asset Database</em>.</li>
<li><code>SceneLoader</code> for the <em>Loading Scene</em>, <em>Initial Scene</em> and every other scene in the <code>scene-structure</code> segment with <code>load-on-startup</code> set to true.
<ul>
<li>Every <em>Asset Database</em> specified in the scene file will trigger an <code>AssetDatabaseLoader</code> and will be merged into the current scene’s <em>Private Asset Database</em> on completion.</li>
<li>When all assets have been loaded the <code>SceneLoader</code> will build the structure of the scene and let the <code>SceneManager</code> consume the scene on completion or for the <em>Loading Scene</em> the <code>SceneManagerLoader</code>.</li>
</ul>
</li>
</ul>
<p>An <code>AssetDatabaseLoader</code> will search for the first registered <code>AssetLoaderFactory</code> that knows the current asset type that occured while reading the file. If a factory knows the asset type, we will use this factory to construct an <em>Asset Loader</em> for that asset type and start it. If no factory was found to know this type it will skip this asset type. An <code>AssetDatabaseLoader</code> is finished once all its <em>Asset Loaders</em> are finished.</p>
<p>Lets give a little overview:</p>
<p><a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/a6044e0415aad2348f8a03c6ae6e8f2ad5431d20/wiki/loading_structure.png"><img src="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/raw/a6044e0415aad2348f8a03c6ae6e8f2ad5431d20/wiki/loading_structure.png" alt="Loading Structure"></a></p>
<p>The registered factories are accessed via the static atttributes that the factories hold themselves.</p>
<h3 id="asset-blackboxing">2.2.1. Asset Blackboxing</h3>
<p>Assets, something that is missing in the above overview. But kind of on purpose since an asset representation isn’t really part of a loading structure. The reason for this chapter is that every asset is a total blackbox to the loaders, which is why we need you to wrap every instance of your asset.</p>
<pre><code>class IAsset {
public:
    IAsset(std::string const&amp; name) : name(name) {}
    std::string getName() { return name; }
    
    inline virtual std::type_index getStorageType() const = 0;
    inline virtual std::string getStorateTypeName() const = 0;
    
private:
    std::string name;
};

</code></pre>
<p>You will find this abstract type all over the place during loader implementation, since the abstract loader version does know only very little of your asset type. Informations that are needed when e.g. trying to access an asset or serializing an asset, need to be implemented:</p>
<ul>
<li><code>getStorageType()</code> should return the <a href="https://en.cppreference.com/w/cpp/types/type_index">type_index</a> of your asset type</li>
<li><code>getStorageTypeName()</code> should return a unique id string representing your asset type in e.g. yaml files</li>
</ul>
<p>The following class is the one you should use in your <em>Asset Loader</em>. It provides default implementation that wraps your asset pointer and gives information on your asset type:</p>
<pre><code>template&lt;typename AssetType&gt;
class Asset
    : public IAsset&lt;AssetType&gt; {
    Asset(std::string const&amp; name, std::string const&amp; assetTypeName, AssetType* value)
        : IAsset(name)
        , m_pValue(value)
        , m_typeSpec(TypeSpecT&lt;AssetType&gt;(assetTypeName))
    {}
    
    virtual ~Asset() {
        RELEASE_S(m_pValue);
    }
    
    // Standard override done here
    inline virtual std::type_index getStorageType() const override {
        return m_typeSpec.index;
    }
    inline virtual std::string getStorageTypeName() const override {
        return m_typeSpec.name;
    }
    
    // Will be used when the name of this Asset was mentioned in a file
    inline virtual AssetType* getValue() const {
        return m_pValue;
    }
    
protected:
    AssetType* m_pValue;
    TypeSpecT&lt;AssetType&gt; m_typeSpec;
};

</code></pre>
<p>You can see that all methods of <code>IAsset</code> have been implemented so you can just instantiate it to wrap your asset:</p>
<pre><code>YourAssetType* pYourAsset = new YourAssetType(...);
// After you are done creating your asset wrap it (#noplastic)
IAsset* pAsset = new Asset&lt;YourAssetType&gt;(name, pYourAsset);

</code></pre>
<p><code>TypeSpecT</code> is just a struct that helps YASS keeping track of name &lt;=&gt; type relations. Names are mostly used during loading from yamls and types are useful at runtime to store/categorize and serialize.</p>
<h3 id="loader-abstraction">2.2.2. Loader Abstraction</h3>
<p>Let’s dive right into the first abstraction of a loader for <code>ResultType</code>.</p>
<pre><code>class Interruptable {
public:
    Interruptable() { Interruptables.insert(this); }
    virtual ~Interruptable() { Interruptables.erase(this); }
    
    // Just abort everything, kind of like reset() but in every case
    virtual void interrupt() = 0;
    
    static void interruptAll(bool del = false);
    
protected:
    static std::set&lt;Interruptable*&gt; Interruptables;
};


template&lt;typename ResultType&gt;
class Loader
    : public Interruptable {
public:
    // This is the method were you create your asset.
    // It will be called e.g. when this loader was provided for a specific asset type occured
    // an asset database yml file
    virtual bool startLoading(App* application = nullptr, Consumer&lt;T&gt;* callMeMaybe = nullptr) = 0;
    
    // Implement a strategy to reset the loader to a state at which startLoading() can be used again.
    virtual bool reset() = 0;
    
    // Returns the resulting asset
    virtual ResultType* getResult() = 0;
    
    virtual bool isResultReady() const {
      return getState() == LoaderState::RESULT_READY;
    }

    virtual bool isIdle() const {
      return getState() == LoaderState::IDLE;
    }

    virtual bool isBroken() const {
      return getState() == LoaderState::BROKEN;
    }

    virtual bool isBusy() const {
      return getState() == LoaderState::BUSY;
    }
    
    // Provides information on the current state of the loader
    virtual LoaderState getState() const = 0;

    // Provides a type string for this loader
    virtual std::string getTypeId() const = 0;
}    

</code></pre>
<p>The <code>startLoading(..)</code> method is what should manage the loading process. How and when you will actually load what part of <code>T</code> is totally in your hands. But once you are finished you need to either call the <code>Consumer&lt;T&gt; callMeMaybe</code> or set the state to <code>RESULT_READY</code> if you know that another procedure will ping this class later to get the result. Internally we rely mostly on the <code>Consumer&lt;T&gt;</code> strategy, what is important to have in mind, when it comes to expanding your YASS CWEEN. Another problem with this loader could be that you need more informations (e.g. a filepath, a YAML node etc.). For that purpose a subclass of <code>Loader</code> named <code>ParameterizedLoader</code> was declared:</p>
<pre><code>template&lt;typename ResultType, class ... Parameters&gt;
class ParameterizedLoader
    : public Loader&lt;ResultType&gt; {
public:
    // This method can take any parameters you want to
    virtual bool configure(Parameters... parameters) = 0;
    
    // Typical implementation:
    // return configure(parameters) &amp;&amp; startLoading(application, callback);
    virtual bool startConfigured(Parameters... parameters, App* application = nullptr, Consumer&lt;T&gt;* callMeMaybe = nullptr) = 0;
};

</code></pre>
<p>So with that abstract class you have an opportunity to give the loader more details on how to load <code>ResultType</code> e.g. the YAML node. Let’s have an example for a generic asset loader:</p>
<pre><code>class AssetLoader
    : public ParameterizedLoader&lt;IAsset, std::string const&amp; /* name */, std::string const&amp; /* filePath */&gt;
    , Gospel::Updateable
{
public:
    virtual IAsset* getResult() { return m_pResult; }
    
    virtual bool configure(std::string const&amp; name, std::string const&amp; filePath) override {
        // store informations
    }
    
    virtual bool startLoading(App* application, Consumer&lt;IAsset&gt;* callback = nullptr) override {
        m_loaderState = LoaderState::LOADING;
        application-&gt;addUpdateable(this);
        // store callback if not nullptr
        // prepare loading process
    }
    
    virtual bool startConfigured(std::string const&amp; name, std::string const&amp; filePath, App* application, Consumer&lt;IAsset&gt;* callback = nullptr) override {
        return configure(name, filePath) &amp;&amp; startLoading(application, callback);
    }
    
    virtual bool reset() override {
        // delete already created asset details
        // reset filepath etc.
        m_loaderState = LoaderState::IDLE;
    }
    
    virtual std::string getTypeId() const override {
        return "myAsset";
    }
    
    virtual LoaderState getState() const override {
        return m_loaderState;
    }
    
    virtual void update(Gospel::OGLContext* context, double globalGameTime) override {
        // make your loading count... and done
        if(ready) {
            m_pResult = new Asset&lt;YourAssetType&gt;(nameToFindUnder, m_pYourAssetResult);
            
            // If we have a consumer let him consume
            if(callback) {
                callback-&gt;onEvent(m_pResult, this);
            }
            
            // We are done here
            application-&gt;removeUpdateable(this);
        }
    }
    
private:
    LoaderState m_loaderState;
    YourAssetType* m_pYourAssetResult;
    IAsset* m_pResult;
    // ...
};  

</code></pre>
<p>Did you mind the complicated asset storing? See <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#226-asset-blackboxing">AssetBlackboxing</a> for more details if you like.</p>
<h3 id="consumer">2.2.3. Consumer</h3>
<p><code>startLoading(...)</code> does not return any result immediately. This way you do not need to load everything within a single tick. But because of that we need another strategy of how to get the result. One would be to ask the loader every tick if he has finished loading. But in this chapter we look at <em>Consumer</em> which can be passed into <code>startLoading(...)</code> at the beginning and will be called per convention once the loader has finished.</p>
<pre><code>template&lt;class... EventParameters&gt;
class Listener {
    virtual void onEvent(EventParameters... params) = 0;
};

template&lt;typename T&gt;
class Consumer
    : Listener&lt;T*, Loader&lt;T&gt;*&gt; {
public:
    virtual App* getApplication() const = 0;
    
    virtual void consume(T* t, Loader&lt;T&gt;* loader) = 0;
    
    inline void onEvent(T* t, Loader&lt;T&gt;* loader) override {
        ...
    }
};

</code></pre>
<p>Where:</p>
<ul>
<li><strong>T</strong> is the type you want to consume (e.g. the <em>Scene Manager</em>).</li>
<li><strong>consume(…)</strong> can do whatever you want to do with <code>T</code>.</li>
<li><strong>onEvent(…)</strong> will deligate to <code>consume(...)</code> next tick.</li>
<li><strong>getApplication()</strong> needs to be implemented in order to make <code>onEvent(...)</code> work.</li>
</ul>
<p>Why scheduling <code>consume(...)</code> with <code>onEvent(...)</code>?<br>
The loader needs to be deleted somewhere and this can be anywhere since we do not even know if you will consume. Maybe you do not even use a <em>Consumer</em> and the loader needs to exist a little longer. So when using a <em>Consumer</em> the best way would be that this <em>Consumer</em> deletes the loader. But a normal callstack would look like this:<br>
<em>Loader::update()<br>
-&gt; Consumer::consume()<br>
----&gt; delete Loader<br>
-&gt; return</em><br>
But now it cannot return since the loader it wants to return to is invalid memory!<br>
That is why <code>onEvent(..)</code> is called instead of directly calling <code>consume(..)</code>. That way you can delete the loader since it does not appear in the current call stack. Of course you do need to unregister it as updateable, otherwise it will find again invalid memory.</p>
<h3 id="factories">2.2.4. Factories</h3>
<p>Sometimes we need a little bit more of these fine onion slices of abstraction to make even more expansion possible.</p>
<h4 id="asset-loader-factory">Asset Loader Factory</h4>
<p>An <em>AssetDatabaseLoader</em> will iterate through the static <code>AssetLoaderFactory::RegisteredFactories</code> attribute to find a factory that knows the current asset type. This <code>AssetLoaderFactory</code> will then be asked to construct an <code>AssetLoader</code> for that specific asset; hence the name.</p>
<pre><code>class AssetLoaderFactory
    : public YGenericSerializer&lt;IAsset&gt; {
public:
    // Here you can register your own factories and can find
    // factories for the default assets
    static AssetLoaderFactories RegisteredFactories;
    
public:
    // Placeholder implementation that spits out a warning if this factory doesn't implement a serialization
    virtual bool serialize(YAML::Emitter&amp; em, IAsset const* pAsset, std::type_index const&amp; subType) const override;
    
    // Will be asked by the AssetDatabaseLoader if this factory knows
    // the name that was found in the file
    virtual bool knowsAssetLoaderType(std::string const&amp; id) const;
    
    // Will be asked by the AssetDatabase if it was told to serialize itself
    // hence its assets, which it only knows under the typeid
    virtual bool knowsAssetLoaderType(std::type_index const&amp; tI) const;
    
    // Return type ids that should be completed before the type with the passed in
    // id can be loaded
    virtual std::vector&lt;std::string&gt; checkLoadRequirements(std::string const&amp; id) = 0;
    
    // Will be called if this factory knows the asset type
    // Gets some informations to configure the loader
    // @note do not call startLoading(..) here!
    virtual Loader&lt;IAsset&gt;* constructAssetLoader(std::string const&amp; type, std::string const&amp; name,
                                                 YAML::Node const&amp; node, AssetDatabase const&amp; otherAssets)
    {
        // Default implementation will search m_supportedAssetLoader for a corresponding type and
        // call its deposited function pointer
    }
                                                 
    // Typical declaration for a function that creates an asset loader
    typedef Loader&lt;IAsset&gt;* (*CreateAssetLoader_Func)(std::string const&amp;, YAML::Node const&amp;, AssetDatabase const&amp;);
    
protected:
    TypeCollectionF&lt;IAsset, CreateAssetLoader_Func&gt; m_supportedAssetLoader;
};

</code></pre>
<h4 id="attachment-factory">Attachment Factory</h4>
<p>Some things need a factory and a loader, somethings only a loader… Well an <em>Attachment</em> needs only a factory. The difference between the loading procedure for an asset and the creation of an attachment is that an attachment is attached to its parent and after that completely on its own. So while an asset is added into a static <em>AssetDatabase</em> the attachment hangs itself onto an <code>Gospel::AdvancedSceneGraphNode</code> and can modify it during runtime.</p>
<pre><code>class AttachmentFactory {
public:
    virtual knowsAttachmentType(std::string const&amp; id) = 0;
    
    // Create the attachment here:
    // YourAttachmentType* pAttachment = owner-&gt;addAttachment&lt;YourAttachmentType&gt;(isRenderable);
    virtual bool createAttachment(std::string const&amp; id, Gospel::AdvancedSceneGraphNode* owner,
                                  YAML::Node const&amp; node) = 0;
}

</code></pre>
<p>Smarty eyes will realize that the attachment needs to have a predefined constructor. This is a standard convention that is needed to avoid a lot of casting and manage the different types. Last one wouldn’t be possible with polymorphism (see <a href="https://en.cppreference.com/w/cpp/language/typeid">typeid operator</a>), so we determined the constructor to take a <code>Gospel::AdvancedSceneGraphNode</code> that is its parent</p>
<h3 id="scene-manager-loader">2.2.5. Scene Manager Loader</h3>
<h4 id="start">Start</h4>
<p>Is started by the user when calling <code>startLoading(...)</code> or <code>startConfigured(...)</code>.</p>
<h4 id="loading">Loading</h4>
<p>Will start:</p>
<ul>
<li><code>AssetDatabaseLoader</code> for every specified <em>Asset Database</em> with the <code>shared-assets</code> keyword will be started</li>
<li><code>SceneLoader</code> for the <em>Loading Scene</em>, <em>Initial Scene</em> and every scene that has <code>load-on-startup</code> set to true will be started to.
<ul>
<li>the <em>Scene Manager</em> will know the path to every <em>Scene</em> that can be loaded later.</li>
<li>the <em>Loading Scene</em> is not relying on the <em>Shared Asset Database</em> but every other <em>Scene</em> needs to wait with building its structure until the shared assets are loaded completey</li>
</ul>
</li>
</ul>
<h4 id="finish">Finish</h4>
<p><code>RESULT_READY</code>:</p>
<ul>
<li>when the <em>Loading Scene</em> specified by <code>loading-scene-path</code> has finished loading sucessfully.</li>
<li><em>Consumer</em> will be called if one was specified</li>
</ul>
<p><code>BROKEN</code>:</p>
<ul>
<li>… never Kappa</li>
</ul>
<h3 id="scene-loader">2.2.6. Scene Loader</h3>
<h4 id="start-1">Start</h4>
<p>Started by the <code>SceneManagerLoader</code> when it loads the <em>Initial Scene</em>, <em>Loading Scene</em> or a load on startup <em>Scene</em>. Otherwise it may be triggered by the <em>Scene Manager</em> when a switch to this scene is desired.</p>
<h4 id="loading-1">Loading</h4>
<p>An <code>AssetDatabaseLoader</code> will be started for every <em>Asset Database</em> file specified for this <em>Scene</em>. When the assets have been loaded the scene tree will be build and every occured attachment will trigger a request for an existing <code>AttachmentFactory</code> for this attachment type, which will then create the attachment during scene build-up.</p>
<h4 id="finish-1">Finish</h4>
<p><code>RESULT_READY</code>:</p>
<ul>
<li>all <code>AssetDatabaseLoader</code>s for the <em>Scene’s</em> <em>Private Asset Database</em> have finished loading (successfull or not)</li>
<li>the scene structure has been build</li>
<li><em>Consumer</em> will be called if one was specified</li>
</ul>
<p><code>BROKEN</code>:</p>
<ul>
<li>… never Kappa</li>
</ul>
<h3 id="asset-database-loader">2.2.7. Asset Database Loader</h3>
<h4 id="start-2">Start</h4>
<p>Start for the <em>Shared Asset Database</em> from the <code>SceneManagerLoader</code> or for the <em>Private Asset Database</em> from the <code>SceneLoader</code>.</p>
<h4 id="loading-2">Loading</h4>
<p>Searches the static <code>AssetLoaderFactory::RegisteredFactories</code> attribute for a factory that knows the current asset type. Once it finds one it checks if the required asset types for that loader are finished. If yes it requests an asset loader and call <code>startLoading(...)</code>, if not it will queues this asset type for later.</p>
<h4 id="finish-2">Finish</h4>
<p><code>RESULT_READY</code>:</p>
<ul>
<li>all started asset loader have finished loading (successfull or not)</li>
</ul>
<p><code>BROKEN</code>:</p>
<ul>
<li>… never Kappa</li>
</ul>
<h1 id="workflow">3. Workflow</h1>
<p>Once you know the overall structure you can fill it with content. This chapter focuses on how to start a YASS CWEEN query. A more practical approach of how to work with your YASS CWEEN, once you have your YAML files (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#21-staticfile-structure">2.1. File Structure</a> for that).</p>
<h2 id="namespace">3.1. Namespace</h2>
<p>Every component mentioned during this Wiki is within the <code>YASS</code> namespace, except a component’s name was preceeded by another namespace like e.g. the <code>Gospel::AdvancedSceneGraphNode</code>.</p>
<h2 id="include-setup">3.2. Include Setup</h2>
<p>To get the YASS CWEEN you need to include it:</p>
<pre><code>#include &lt;yass.h&gt;

</code></pre>
<p>You can make some defines prior to manipulate what is to be included:</p>
<ul>
<li><code>YASS_STATIC_ONLY</code> the complete <em>Loading Structure</em> is not included and the <em>Static Structure</em> is slightly manipulated to compile without the <em>Loading Structure</em> (<a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/issues/3">TODO</a>).</li>
<li><code>YASS_FAST_USE</code> will include classes that are kinda dirty but can make it faster/easier to begin with YASS</li>
<li><code>YASS_INCLUDE_YAML</code> will include the yaml-cpp header definitions too</li>
</ul>
<h2 id="loading-preparation">3.3. Loading: Preparation</h2>
<p>You should consider some preparation for the loading, since it can have an influence on loading speed and how you aquire the <em>Scene Manager</em>.</p>
<h3 id="memory">3.3.1. Memory</h3>
<p>We pool the memory for the <code>SceneLoader</code> and <code>AssetDatabaseLoader</code>. Once a <em>Loader</em> has finished loading its <em>Consumer</em> will free the loader’s memory into the pool so it can be reused. So in theory you can set the pool size to one, but it will slow down the loading process a lot since no parallel loading will be done. Default values are:</p>
<pre><code>#define DEFAULT_SCENE_LOADER_POOL_SIZE 29
#define DEFAULT_ASSETDB_LOADER_POOL_SIZE 29*2

</code></pre>
<p>But it is recommended to set it to a more precise size according to your setup:</p>
<pre><code>SceneManagerLoader::DefaultSceneLoaderPoolSize = &lt;size_t&gt;;
SceneManagerLoader::DefaultAssetDbLoaderPoolSize = &lt;size_t&gt;;

</code></pre>
<p>You need to do that before starting the loading process, because the loading process will create the pools with what ever size was set prior.</p>
<h3 id="scene-manager-consumer">3.3.2. Scene Manager Consumer</h3>
<p>When you start the loading process you can pass a consumer for the <em>Scene Manager</em>, which is basically the main result (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home#223-consumer">2.2.3. Consumer</a> for details). So you need to implement a <em>Consumer</em> that does whatever you want to do with the <em>Scene Manager</em> in <code>consume(..)</code>.</p>
<h4 id="alternative">Alternative</h4>
<p>You could also continously ping/annoy the <code>SceneManagerLoader</code> if it is ready and if so acquire the <em>Scene Manager</em>:</p>
<pre><code>if (pSceneManagerLoader-&gt;isResultReady()) {
    pSceneManager = pSceneManagerLoader-&gt;getResult();
    // do your thing here...
} else if (pSceneManagerLaoder-&gt;isBroken()) {
    // do your error handling...
}

</code></pre>
<p>But we do <strong>not</strong> consider this a pro gamer move!</p>
<h4 id="fast-use">Fast Use</h4>
<p>When working within Gospel you can use one of the following defines as the <em>Consumer</em>:</p>
<pre><code>#define SCENEMGR_CONSUMER_DUMMY_STACK(appPtr) (&amp;(::YASS::StdSceneManagerConsumer(appPtr)))
#define SCENEMGR_CONSUMER_DUMMY_HEAP(appPtr) (new ::YASS::StdSceneManagerConsumer(appPtr))

</code></pre>
<p><code>appPtr</code> needs to be of type <code>App*</code>. The <code>StdSceneManagerConsumer</code> will register the <em>Scene Manager</em> as an <code>Gospel::Updateable</code> to the application.</p>
<p><strong>Note:</strong> You need to enable <em>Fast Use</em> (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#32-include-setup">3.2. Include Setup</a>).</p>
<h2 id="loading-start">3.4. Loading: Start</h2>
<p>From the <em>Loading Structure</em> overview you should have learned that the start of the whole loading process is the <code>SceneManagerLoader</code>. So that is the only thing you will need to start and all other loading process will be started according to your <em>Scene Manager</em> file.</p>
<pre><code>YASS::SceneManagerLoader* pSceneManagerLoader = new YASS::SceneManagerLoader;
if (!pSceneManagerLoader-&gt;startConfigured(path, app, consumer) {
    // your wonderful error handling...
}

</code></pre>
<p>Where:</p>
<ul>
<li><strong>path</strong> is the absolute path to your <em>Scene Manager</em> file</li>
<li><strong>app</strong> is the pointer to the current application of type <code>App*</code></li>
<li><strong>consumer</strong> is the pointer to the instance that will consume the <em>Scene Manager</em> once it has been finished. Can be one of the <em>Fast Use</em> defines.</li>
</ul>
<p>If <strong>consumer</strong> is nullptr you will need to <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#alternative">ping</a> the <code>SceneManagerLoader</code>.</p>
<h2 id="cleanup">3.5. Cleanup</h2>
<p>You also need to cleanup after yourself. For that every loader needs to implement its <code>interrupt()</code> method. After this method was called a loader should not rely on anything except maybe the scenemanager to still exist. To start the cleanup you need to store your <em>SceneManager</em> result somewhere and delete it. When working with fast use it could look like this:</p>
<pre><code>YASS::StdSceneManagerConsumer* pConsumer = SCENEMGR_CONSUMER_DUMMY_HEAP(&amp;application);
//
// ... start loading and do runtime stuff
//
delete pConsumer-&gt;getResult();
YASS::SceneManager::cleanup();

</code></pre>
<p>This will start the cleanup of all scenes, assets, attachments …</p>
<h1 id="assets">4. Assets</h1>
<h2 id="expansion">4.1. Expansion</h2>
<p>In this chapter you will learn how to add a type of asset to YAML. Every top-level name in an <em>Asset Database</em> file refers to a specific type of an asset. The basics step to tell YAML about your asset are:</p>
<ol>
<li>Implement an <code>AssetLoader</code> for the asset you want to add.</li>
<li>Implement an <code>AssetLoaderFactory</code> which associates the top-level name read from the YAMl file with the <code>AssetLoader</code> that loads this asset type.</li>
<li>Push back an instance of this <code>AssetLoaderFactory</code> to the static <code>AssetLoaderFactory::RegisteredFactories</code> attribute.</li>
</ol>
<h2 id="featured">4.2. Featured</h2>
<p>Expansion is nice, but we provide some default asset types so that you don’t need to work from scratch. Mind that these default assets represent specific types. So they depend on each other and work only with each other and don’t know your custom assets. So they can’t interact with your asset types, but your asset types can work with them. You can disable the default assets by defining <code>YASS_NO_DEFAULT_ASSETS</code>. Best way to do that is with a compile flag in CMake: <code>add_definitions("-DYASS_NO_DEFAULT_ASSETS")</code><br>
or since CMake 3.12.4: <code>add_compile_definitions("YASS_NO_DEFAULT_ASSETS")</code>.</p>
<h3 id="shaders">4.2.1. Shaders</h3>
<p><code>shader</code> contains a <strong>set of shader definitions</strong> according to the following pattern:</p>
<pre><code>&lt;shader name&gt;:
    &lt;shader stage&gt;: "&lt;path to shader file&gt;"
    &lt;...&gt;
    compile-params:
        &lt;shader stage&gt;:
            "&lt;key&gt;": "&lt;value&gt;"
            &lt;...&gt;
        &lt;...&gt;
    render-priority: &lt;string&gt;

</code></pre>
<p>Replace <code>shader stage</code> with one of the following:</p>
<ul>
<li>vertex</li>
<li>fragment</li>
<li>geometry</li>
<li>tesselation-control</li>
<li>tesselation-evaluation</li>
<li>compute</li>
</ul>
<p>Under <code>compile-params</code> for <code>key</code> will be replaced with <code>value</code> for the according <code>shader stage</code>.<br>
Valid <code>render-priorities</code> are:</p>
<ul>
<li>background</li>
<li>geometry</li>
<li>transparent-geometry</li>
<li>overlay</li>
<li>overlay-2d</li>
</ul>
<p>Overlay-2D shader will get an orthogonal projection matrix and base view matrix.</p>
<h3 id="materials">4.2.2. Materials</h3>
<p>‘material’ contains a <strong>set of material definitions</strong> according to the following pattern:</p>
<pre><code>&lt;material-name&gt;:
    shader: "&lt;shader-name&gt;"   # obligatory
    gl-options:
        &lt;option&gt;: &lt;value&gt;     # optional
        &lt;...&gt;
    uniforms:
        &lt;type&gt;:
            &lt;name&gt;: &lt;value&gt;   # optional

</code></pre>
<p>Every material uses some kind of shader progam which is defined by the Shader Asset (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#421-shaders">4.2.1.</a>). Available OpenGL options are:</p>
<ul>
<li>… TODO</li>
</ul>
<p>Under <code>uniforms</code> the uniform with the according name and type will be set to <code>value</code> before receiving the draw call.</p>
<h3 id="meshes">4.2.3. Meshes</h3>
<p>‘mesh’ contains a <strong>set of mesh definitions</strong> according to the following pattern:</p>
<pre><code>&lt;mesh-name&gt;: "&lt;.obj/.blend&gt;"

</code></pre>
<p>Will create a <code>Gospel::OGLObject</code> for every object defined within the .obj file and split each into multiple submeshes if it has different materials per face. It also computes tangents and bitangents. If the file is loaded the first time it will dump a binary file named <code>&lt;file-name&gt;.blob</code>, which will be loaded the next time. That way the loadtime can be reduced to a fraction of the normal loadtime.</p>
<h3 id="fonts">4.2.4. Fonts</h3>
<p><code>font</code> contains a <strong>set of font definitions</strong> according to the following pattern:</p>
<pre><code>&lt;font-name&gt;:
    path: &lt;string&gt;         # obligatory
    resolution: [int, int] # optional, defaults to: [100, 100]
    charset: "&lt;string&gt;"    # optional, defaults to: standard

</code></pre>
<p>or the short variant:</p>
<pre><code>&lt;font-name&gt;: &lt;string&gt;      # just the path

</code></pre>
<p>The short variant uses the default values for the non-defined parameters.</p>
<ul>
<li><code>path</code>: string describing the relative path to the font file; internally FreeType is used so consult FreeType for supported font formats (some are TTF, WOFF, CFF and OpenTypeFonts).</li>
<li><code>resolution</code>: a list where the x is the pixel width and y the pixel height; note that big resolution create big font texture and can clutter your GPU.</li>
<li><code>charset</code>: predefined keywords standard and reduced; check FontLoader.h for information about what characters are contained. Use FontLoader::addCharSet(name, chars) to add your own ones and reference them inside your YAML.</li>
</ul>
<h3 id="animations">4.2.5. Animations</h3>
<p>[[deprecated]] <code>animation</code> contains a <strong>set of animation definitions</strong> according to the following pattern:</p>
<pre><code>"&lt;animation name&gt;":
    channels: ["&lt;target&gt;/&lt;type&gt;", &lt;...&gt;]
    interpolation: "&lt;interpolation style&gt;"
    length: "&lt;duration in seconds&gt;"
    frames:
        &lt;timestamp in secs&gt;:
            "&lt;channel&gt;": &lt;value&gt;

</code></pre>
<p>Channel types may be any of <code>translation</code> (3d vector), <code>rotation</code> (quaternion as 4d vector), <code>scale</code> (3d vector) or <code>visible</code> (boolean). Currently, interpolation styles are <code>none</code> (jump), <code>linear</code> (duh) and sine (smooooth) are supported.<br>
An animation <strong>must have</strong> a frame at times <code>0</code>and <code>&lt;length&gt;</code> with values for all registered channels. Otherwise, frames are not required to define a value for every channel.</p>
<h1 id="attachments">5. Attachments</h1>
<h2 id="expansion-1">5.1. Expansion</h2>
<p>First what is an <em>Attachment</em>:<br>
An <em>Attachment</em> itself can be literally anything and is attached to its <em>Scene Node</em> called the <em>Owner</em>, hence the name. In code:<br>
The <em>Owner</em> is represented by the <code>Gospel::AdvancedSceneGraphNode</code> class; in code we also stuck to the convention seeing the <em>Owner</em> as the <code>parent</code>, you can see these two as equivalent during this chapter. Moreover every <em>Attachment</em> type needs to be a subclass of <code>Gospel::IAttachment</code> and will be created with the following constructor:</p>
<pre><code>IAttachment(Gospel::AdvancedSceneGraphNode* parent) : m_pParent(parent) {}

</code></pre>
<p>The <code>m_pParent</code> member is private, but you can get it via <code>getParent()</code>. The instantiation is done by the <em>Owner</em> via calling the following templated method:</p>
<pre><code>template&lt;typename T&gt;
T* Gospel::AdvancedSceneGraphNode::addAttachment() {
    T* ia = new T(this);
    m_attachments[typeid(T)] = ia;
    return ia;
}

</code></pre>
<p><code>addAttachment</code> does take care of the instantiation, passes itself as the <em>Owner</em> to the attachment and handles type management. An example for an attachment creation would look like this:</p>
<pre><code>CameraDolly* dolly = owner-&gt;addAttachment&lt;CameraDolly&gt;();
dolly-&gt;setActive(node["active"].as&lt;bool&gt;(true));

</code></pre>
<p>This is the creation of the <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#525-camera-dolly">camera dolly</a> that is featured within the default attachments.</p>
<p>Now lets get to the where of the creation; from where will you get the <em>Owner</em>. The <em>Scene Loader</em> will search the static <code>AttachmentFactories::RegisteredFactories</code> attribute for a factory that says it knows the current attachment id. So you have to implement an <code>AttachmentFactory</code>. See <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#224-factories">2.2.4. Factories</a> for more details on what is to implement, but your attachment creation should take place in <code>createAttachment(...)</code> which is getting the owner of the attachment and the YAML node so you can read in whatever details you need for the attachment. Every <code>AttachmentFactory</code> can know more than just one attachment type, which is why <code>createAttachment(...)</code> does also get the id.<br>
So once you have your own <code>AttachmentFactory</code> that creates your own <em>Attachment</em>, it is time to register this factory:</p>
<pre><code>AttachmentFactory::RegisteredFactories.push_back(new YourAttachmentFactory);
</code></pre>
<p>Now every time the id of your attachment type is found within a scene file your factory will say: “Hey, I know that type!” and <code>createAttachment(...)</code> will be called.</p>
<h2 id="core">5.2. Core</h2>
<p><em>Core Attachments</em> are implemented for rendering on the Dome with a <em>Linear Renderer</em> in full support with gui and the console. You can disable these, but keep in my mind that all basic implementation to render a scene will be lost.<br>
To disable the <em>Core Attachments</em> <code>YASS_NO_CORE_ATTACHMENTS</code> needs to be defined. Best way to do that is with a compiler flag in CMake:<br>
Before 3.12.4:<br>
<code>add_definitions("-DYASS_NO_CORE_ATTACHMENTS")</code><br>
Since 3.12.4:<br>
<code>add_compile_definitions("YASS_NO_CORE_ATTACHMENTS")</code></p>
<h3 id="renderable">5.2.1. Renderable</h3>
<h3 id="relative-transform">5.2.2. Relative Transform</h3>
<p>Applies a transform relativ to the scene node’s parent. Properties:</p>
<ul>
<li>‘translation: [x,y,z]’ where x,y,z describe the offset to the parent</li>
<li>‘rotation’ describes the rotation of the scene node. The rotation excepts multiple values:
<ul>
<li>Euler angles: if delivered a sequence of 3 entries, each entry represents the rotation around its axis in degrees</li>
<li>Quaternion: if delivered a sequence of 4 entries, these will be interpreted as a quaternion: [w, x, y, z]. These entries can also be described as a map like 'w: ’ for each entry.</li>
</ul>
</li>
<li>‘scale: [x,y,z]’ additional scale.</li>
</ul>
<h2 id="featured-1">5.3. Featured</h2>
<p>YASS features some default <em>Attachments</em> for general use cases. You can disable these by defining <code>YASS_NO_DEFAULT_ATTACHMENTS</code>. Best way to do that is with a compiler flag in CMake: <code>add_definitions("-DYASS_NO_DEFAULT_ATTACHMENTS")</code> or since CMake 3.12.4: <code>add_compile_definitions("YASS_NO_DEFAULT_ATTACHMENTS")</code>.</p>
<h3 id="area-trigger">5.3.1. Area Trigger</h3>
<h3 id="audio">5.3.2. Audio</h3>
<h3 id="audio-cycler">5.3.3. Audio Cycler</h3>
<h3 id="debug-details">5.3.4. Debug Details</h3>
<h3 id="camera-dolly">5.3.5. Camera Dolly</h3>
<p>The camera will stick to whatever <em>Scene Node</em> has the camera dolly attached to it.</p>
<pre><code>attachments: 
    camera-dolly:
        active: &lt;bool&gt; # defaults to true
    &lt;...&gt;

</code></pre>
<h3 id="light-source">5.3.6. Light Source</h3>
<h3 id="navmesh">5.2.7. Navmesh</h3>
<h3 id="navmesh-agent">5.2.8. Navmesh Agent</h3>
<h3 id="screen-fade">5.2.9. Screen Fade</h3>
<h2 id="text">5.4. Text</h2>
<h3 id="text-1">5.4.1. Text</h3>
<p>Defines text that is positioned within the owner’s space and can be defined like the following:</p>
<pre><code>attachments:
    text:
        font: &lt;string&gt;               # obligatory
        material: &lt;string&gt;           # obligatory
        text: &lt;string&gt;               # obligatory
        texture-bind-name: &lt;name&gt;    # optional, defaults to: "tex_"&lt;font-name&gt;
        space-width: &lt;float&gt;         # optional, defaults to: max letter width of font
        letter-spacing: &lt;float&gt;      # optional, defaults to: 0.0f
        line-spacing: &lt;float&gt;        # optional, defaults to: 0.0f
        scale: &lt;float&gt;               # optional, defaults to: 10.0f
        render-2d: &lt;bool&gt;            # optional, defaults to: false
        cursor: &lt;bool&gt;               # optional, defaults to: false
        wrapping: [&lt;float&gt;, &lt;float&gt;] # optional, defaults to: [-1.0f, -1.0f], meaning no wrapping
        dynamic: &lt;bool&gt;              # optional, defaults to: false

</code></pre>
<ul>
<li><code>font</code>: the name of the font asset that this text should use (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#424-fonts">4.2.4</a>).</li>
<li><code>material</code>: shader program definitions to use (see <a href="https://git.cg.cs.tu-bs.de/Teamprojekte/YASS/wiki/Home/_edit#422-materials">4.2.2.</a>). Note: If you want to render 2d set the render priority of the according shader to <code>overlay-2d</code>.</li>
<li><code>text</code>: String describing the text you want to render; for new lines type “\n”</li>
<li>`texture-bind-name’: the name of the uniform under which the font texture should be bound to</li>
<li><code>space-width</code>: distance made for every space</li>
<li>'letter-spacing`: additional distance to the bearing between characters: 1 means max letter width.</li>
<li><code>line-spacing</code>: additional distance between lines</li>
<li><code>scale</code>: scaling factor that is applied when rendering</li>
<li><code>render-2d</code>: If set to true the text will use only the x and y position of the owner</li>
<li><code>cursor</code>: If set to true the cursor methods are enabled</li>
<li><code>wrapping</code>: the amount of world space for width and height, negative ones mean no wrapping for that one. Make sure that horizontal wrapping values need to allow always one character: bigger than the max letter width times the scale, and vertical wrapping values bigger than the scale.</li>
<li><code>dynamic</code>: If set to true no static mesh will be created for the text and every character will be rendered on its own -&gt; slow rendering process but immediate rendering of the text. If set to false, the text is first rendered dynamically and once the static mesh has been builded statically.</li>
</ul>
<h3 id="cursor">5.4.2. Cursor</h3>
<h1 id="input">6. Input</h1>
<h2 id="generic-input-provider">6.1. Generic Input Provider</h2>
<h2 id="yaml-input">6.2. YAML Input</h2>
<p>You can setup some aliases within your <em>SceneManager</em> file:</p>
<pre><code>inputs:
    your-key-alias:
        keycodes: &lt;int&gt; or [&lt;int&gt;, ...]  # must be at least one valid glfw keycode
        require: "&lt;any/all&gt;"             # optional, defaults to any
        on-event: "[&lt;KeyState&gt;, ...]"    # optional, list of key states on which an event is triggered: "event://input#your-key-alias?key-state"

</code></pre>
<p><em>any</em> means that only one of the defined keycodes needs to be hit, so that <em>your-key-alias</em> is hit, and <em>all</em> means that all defined keycodes need to be hit in order for <em>your-key-alias</em> to get the state hit. So <em>any</em> is a max operation over the defined key states and <em>all</em> a min operation.</p>
<h1 id="scripted-interactions">7. Scripted Interactions</h1>
<p>Before we can fully understand the <em>Scripted Interactions</em> we need to look into the tool they are using: <em>Messages</em>. After this the declaration of the <em>Scripted Interaction</em> will be easy and to wrap things up there will be an example.</p>
<h2 id="the-message">7.1. The Message</h2>
<p>Within the YAMLs a message is basically a string that will be internally parsed into four substrings:</p>
<pre><code>context://objectId#attachmentId?yourmessage
</code></pre>
<p><strong>Constraints:</strong> The order of the delimiter must be ensured:</p>
<ol>
<li><code>://</code>: postfix for the context</li>
<li><code>#</code>: prefix for the attachment id</li>
<li><code>?</code>: prefix for the message</li>
</ol>
<h3 id="event-context">7.1.1. Event Context</h3>
<p>The event context is mainly meant as a trigger for the <em>Scripted Interactions</em>. Since we don’t want to check if the event fired is one from YASS itself, it is possible to fire your own events. But we do not recommend that, since events will always be send to everything within the scene (Scripted Interactions, Scene Tree).</p>
<p><strong>Predefined events:</strong></p>
<ul>
<li><code>event://input#input-alias?key-state</code> for every key-state that was set as <code>on-event</code> for the input-alias</li>
<li><code>event://?sceneActivate</code></li>
<li><code>event://?sceneDeactivate</code></li>
</ul>
<h3 id="scene-context">7.1.2. Scene Context</h3>
<p>The scene context can be used to directly send commands to objects and/or their attachments of the active scene.</p>
<ul>
<li><strong>objectId</strong> can be an objects name of the parental scene or empty</li>
<li><strong>attachmentId</strong> can be a name to which attachments of <code>objectId</code> can decide if they are meant</li>
<li><strong>yourmessage</strong> the message that will be send to all resulting attachments of <strong>objectId</strong> and <strong>attachmentId</strong></li>
</ul>
<p><strong>Predefined commands:</strong></p>
<ul>
<li><code>scene://glOptions#wireframe?toggle</code>: Toggles the wireframe</li>
</ul>
<p><strong>Duplicate use:</strong> It should be avoided to create an object with the same name as any of the predefined objects (like glOptions) within your scene. Otherwise the message will be interpreted by the object itself and the according predefined scene-object.</p>
<h3 id="scene-manager-context">7.1.3. Scene Manager Context</h3>
<p>This context cannot at all be expanded by yourself. It exists to make options above the scene context visible, so that it is not needed to write an attachment for simple scene manager operations like scene switching:</p>
<ul>
<li><code>scenemanager://switcher?scene-name</code> can be defined in the script and will switch to <em>scene-name</em> if a scene was defined for that name in the <em>SceneManager</em> file.</li>
<li><code>scenemanager://switcher#prepare?scene-name</code> stores the <em>scene-name</em> so you can switch to it later</li>
<li><code>scenemanager://switcher?execute</code> switches to whatever scene was last prepared</li>
</ul>
<h2 id="declaration">7.2. Declaration</h2>
<p>A <em>Scripted Interaction</em> can be declared like the following within the <em>Scene</em> YAML:</p>
<pre><code>scripted-interactions:
    your-interaction:
        triggered-by:
            - "&lt;your-message&gt;"      # A valid message, should be within the event context
            ...
        script:
            - "&lt;your-message&gt;"      # A valid message, should be within the scene or scenemanager context
            ...

</code></pre>
<p>If one of the trigger-messages was fired every script-message will be send.</p>
<h2 id="examples">7.3. Examples</h2>
<h4 id="scene-switching-example">Scene Switching Example</h4>
<p>Let’s say you want to switch to a scene with the name “TakeMe” and have the following input defined in the <em>SceneManager</em> file:</p>
<pre><code>inputs:
    scene-switch:
        keycodes: 294 # f5

</code></pre>
<p>The following scripted interaction will allow you to switch from the scene where you put it to “TakeMe”:</p>
<pre><code>scripted-interactions:
    scene-switch:
        triggered-by: "event://input#scene-switch?HIT"
        script: "scenemanager://switcher?TakeMe"

</code></pre>
<h1 id="console">8. Console</h1>
<h2 id="expansion-2">8.1. Expansion</h2>
<h2 id="featured-commands">8.2. Featured Commands</h2>

