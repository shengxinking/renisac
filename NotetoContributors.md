# Requested Sections #

This area indicates pages in the NetFlow Cookbook we would like contributors to work on. Please remove items from this as they are finished. Make sure to remove any mention of the (Requested) tag when finished. When working on a page, replace (Requested) to (WIP) to indicate a work in progress.

<br>

<blockquote><i>Part 2: AppFlow Layer</i> - This section contains a general overview of IPFIX based AppFlow. To our knowledge, there are currently no open source tools available that have this capability. We expect as the standard gains more popularity tools like NFDump will adopt AppFlow support in the future. We also expect open source tools which can generate AppFlow in the future. As the standard develops and more tools become available, we would like contributors to expand <b>Part 2</b> much like <b>Chapter 1-3</b>.</blockquote>

<blockquote><i><a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Integrating_Data_from_CIF'>7.2.1.3.1.2 Integrating Data from CIF (Requested)</a></i> - For future work, we would like to integrate the CIF dataset into the NFSen botnet plugin. Refer to  <a href='https://code.google.com/p/renisac/wiki/Botnet_Install#Importing_Datasets'>7.2.1.3.1 Importing Datasets</a> for more information.</blockquote>

<blockquote><i>Part 5: Cache Layer</i> - In this area we would like a full writeup on the Cache Layer presented on the Main wikipage <a href='https://code.google.com/p/renisac/wiki/Netflow_Cookbookv1'>NetFlow Cookbookv1</a> <b>Figure A.</b> Tiered Model for Traffic Logging. This area should also include a manual on Cacti much like that presented in <b>Part 1</b> and <b>Part 4</b>. We would also like for this section to include instructions on integrating NFSen as a tab in Cacti.</blockquote>

<blockquote><i><a href='https://code.google.com/p/renisac/wiki/Traffic_Classification'>7.1 Traffic Classification: Profiles, Channels, and Plugins</a></i> - Has an unresolved problem under known issues. Please update this problem as more information is discovered. Change the status to Resolved when your confident the problem is fixed.</blockquote>

<br>

<h1>Formatting</h1>

This area addresses formatting guidelines for the NetFlow Cookbook. We kindly request all contributors adhere to the formatting guidelines presented in this page. The goal of this page is to create a consistent and uniform format to be used throughout the entire NetFlow Cookbook.<br>
<br>
Click the edit button to view the wiki syntax for any formatting guidelines covered in this page. Manual on wiki syntax is available here: <a href='http://code.google.com/p/support/wiki/WikiSyntax'>Wiki Syntax</a>.<br>
<br>
<h2>Known Issues Section</h2>
<blockquote>Any page that includes configuration and/or installation should include a known issues section. Replace lines with application specific titles with something more appropriate for the application being written about (in this case replace any mention of NFSen or NetFlow if writing about something other than those tools). Feel free to copy and paste the template below. The format is as follows:</blockquote>

<h1>Known Issues</h1>

This section is for any known issues or problems encountered. Feel free to comment about any errors/bugs/problems not covered in this section. If possible, indicate the following: version of NFSen, the problem that was experienced, conditions which caused the problem to happen, known cause of the problem, solutions found, and any additional information which helps describe the problem. Even if you encountered a problem and have not found a solution, please let us know. We will post any discovered issues in this section. Your comments and issues will help make this the most comprehensive NetFlow manual out there.<br>
<br>
<h2>Error Message Goes Here</h2>

<b>Status:</b>

<b>NFSen Version:</b>

<b>Plugin Name:</b>

<b>Plugin Version:</b>

<h4>Condition:</h4>

<h4>Cause:</h4>

<h4>Additional Information:</h4>

<h4>Solution:</h4>

<br><br>

<h3>Table of Contents</h3>

<blockquote>Each wiki page should include the TableOfContents sidebar. The syntax is as follows: #sidebar TableOfContents. An example can be found at the top of this page in edit mode.</blockquote>

<h3>Use ! to dereference wiki page names</h3>
<blockquote>Wiki syntax will try to hyperlink a wikipage (even if it doesn't exist) if the words are not separated by spaces. Ex. NetFlow vs. Net Flow.</blockquote>

<blockquote>To dereference a wikipage add a ! in from of the word. Ex. NetFlow vs NetFlow.</blockquote>

<h3>HTML Tags</h3>

<blockquote>HTML tags work to an extent. Mainly are only used for formatting. Only use when necessary. Try to use the wiki syntax when possible. Caveat being when newlines are needed or when an html table is needed.</blockquote>

<h3>Editing Table of Contents</h3>

<blockquote>Use an asterick to generate a bulleted list</blockquote>

<ul><li>Each<br>
<ul><li>Space<br>
<ul><li>Is<br>
<ul><li>A<br>
<ul><li>New<br>
<ul><li>Level</li></ul></li></ul></li></ul></li></ul></li></ul></li></ul>

<blockquote>Number Chapters, Sections, and Sub-Sections as follows.</blockquote>

<ul><li>Chapter 6 Title<br>
<ul><li>6.1 Title<br>
<ul><li>6.1.1 Title<br>
<ul><li>6.1.1.1 Title<br>
<ul><li>6.1.1.1.1 Title<br>
<ul><li>6.1.1.1.1.1 Title</li></ul></li></ul></li></ul></li></ul></li></ul></li></ul>

<blockquote>Use hyper links for entries which are meant to link directly to a wikipage (except in <b>Part</b> and <b>Chapter</b> Titles). Example of linking to a subsection in a WikiPage: <a href='https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&updated=NotetoContributors#Editing_Table_of_Contents'>https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&amp;updated=NotetoContributors#Editing_Table_of_Contents</a></blockquote>

<ul><li>Chapter 6 Title<br>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&updated=NotetoContributors#Editing_Table_of_Contents'>6.1 Title</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&updated=NotetoContributors#Editing_Table_of_Contents'>6.1.1 Title</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&updated=NotetoContributors#Editing_Table_of_Contents'>6.1.1.1 Title</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&updated=NotetoContributors#Editing_Table_of_Contents'>6.1.1.1.1 Title</a>
<ul><li><a href='https://code.google.com/p/renisac/wiki/NotetoContributors?ts=1374521074&updated=NotetoContributors#Editing_Table_of_Contents'>6.1.1.1.1.1 Title</a></li></ul></li></ul></li></ul></li></ul></li></ul></li></ul>

<h3>Configuration/Installation Guides</h3>

<blockquote>Each step should begin with a line resembling the following</blockquote>

<b>Step 1:</b>

<b>Step 2:</b>

<b>Step 3:</b>

<h3>References Section</h3>

<blockquote>We generally require a references section. For Configuration and Installation guides we consider this as optional. We would like to leave whether to include this section on a wikipage up to the contributor. Ultimately, if you grab something from another webpage, book, or article you should cite it. An example template for references is below:</blockquote>

<h1>References</h1>

<ol><li><a href='http://www.first.org/global/practices/Netflow.pdf'>http://www.first.org/global/practices/Netflow.pdf</a>
</li><li><a href='http://netflow.caligare.com/index.htm'>http://netflow.caligare.com/index.htm</a>
</li><li><a href='http://www.solarwinds.com/it-management-glossary/what-is-a-netflow-collector.aspx'>http://www.solarwinds.com/it-management-glossary/what-is-a-netflow-collector.aspx</a></li></ol>

<blockquote>Separate references by vendor, if including documentation by multiple vendors, or by topic area. Example template is below:</blockquote>

<h1>References</h1>
<h3>SPAN vs TAP Articles</h3>
<ol><li><a href='http://www.networkinstruments.com/includes/popups/taps/tap-vs-span.php'>http://www.networkinstruments.com/includes/popups/taps/tap-vs-span.php</a>
</li><li><a href='http://www.gigamon.com/stuff/contentmgr/files/1/4f5cd4f7078839df7a1e6f2a8e09e9db/download/span_port_or_tap_web.pdf'>http://www.gigamon.com/stuff/contentmgr/files/1/4f5cd4f7078839df7a1e6f2a8e09e9db/download/span_port_or_tap_web.pdf</a></li></ol>

<h3>Cisco Documentation</h3>
<ol><li><a href='http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml'>http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml</a>
</li><li><a href='http://www.cisco.com/en/US/docs/switches/lan/catalyst3550/software/release/12.1_19_ea1/configuration/guide/swspan.html'>http://www.cisco.com/en/US/docs/switches/lan/catalyst3550/software/release/12.1_19_ea1/configuration/guide/swspan.html</a>
</li><li><a href='http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml'>http://www.cisco.com/en/US/products/hw/switches/ps708/products_tech_note09186a008015c612.shtml</a></li></ol>

<h3>HP Documentation</h3>
<ol><li><a href='http://bizsupport2.austin.hp.com/bc/docs/support/SupportManual/c02640590/c02640590.pdf'>http://bizsupport2.austin.hp.com/bc/docs/support/SupportManual/c02640590/c02640590.pdf</a>
</li><li><a href='http://www.terena.org/activities/campus-bp/pdf/gn3-na3-t4-cbpd111.pdf'>http://www.terena.org/activities/campus-bp/pdf/gn3-na3-t4-cbpd111.pdf</a>
</li><li><a href='http://www.hiddenone.net/hp-procurve/local-port-mirroring/'>http://www.hiddenone.net/hp-procurve/local-port-mirroring/</a>
</li><li><a href='http://www.sysadmintutorials.com/tutorials/hp/hp-procurve-advanced-cli-commands-reference/'>http://www.sysadmintutorials.com/tutorials/hp/hp-procurve-advanced-cli-commands-reference/</a></li></ol>

<h3>Juniper Documentation</h3>
<ol><li><a href='http://www.juniper.net/techpubs/en_US/junos10.1/topics/usage-guidelines/policy-configuring-port-mirroring.html'>http://www.juniper.net/techpubs/en_US/junos10.1/topics/usage-guidelines/policy-configuring-port-mirroring.html</a>
</li><li><a href='https://kb.juniper.net/InfoCenter/index?page=content&cmid=no&id=KB10878&cat=EX3200_1&actp=LIST'>https://kb.juniper.net/InfoCenter/index?page=content&amp;cmid=no&amp;id=KB10878&amp;cat=EX3200_1&amp;actp=LIST</a></li></ol>

<br><br>

<h3>Redirecting Back from Another Page</h3>

<blockquote>When using a WikiPage as a hub for other WikiPage (see example <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies'>Deployment Strategies</a>, link back from the other wiki page to the hub using the template below. Place at the bottom of page or at the bottom of each section of the WikiPage.</blockquote>

<br>
<h4><- <a href='https://code.google.com/p/renisac/wiki/DeploymentStrategies?ts=1370391879&updated=DeploymentStrategies'>Return to Deployment Strategies</a></h4>

<h3>Discussion Pages</h3>

<blockquote>Treat discussion pages like a research paper. Write as much as possible and engage the reader. Include all references used in the <a href='https://code.google.com/p/renisac/wiki/NotetoContributors#References_Section'>format</a> shown above. Examples of discussion pages: <a href='https://code.google.com/p/renisac/wiki/NetFlow'>What is NetFlow?</a> <a href='https://code.google.com/p/renisac/wiki/FlowDataExport'>Flow Data Export</a> <a href='https://code.google.com/p/renisac/wiki/SwitchConfiguration#SPAN_vs_TAP'>SPAN vs TAP</a> <a href='https://code.google.com/p/renisac/wiki/PacketCapture'>Packet Capture</a></blockquote>

<h3>In-Text Citation</h3>

<blockquote>When paraphrasing or grabbing a direct quote use the IEEE style of in-text citations. Note how we link to the corresponding section in the reference template shown above. Example for referencing a SPAN vs TAP article <a href='https://code.google.com/p/renisac/wiki/NotetoContributors#SPAN_vs_TAP_Articles'>[2</a>].</blockquote>

<h3>Pictures</h3>

<blockquote>Pictures require an html link ending with the extension of the image (ex. <img src='http://www.something.com/something.jpg' />). In order to have an image show up on the wiki all that's required is to include the html link to the image. <b>Generally, you should submit a request to the project owner to store and provide an html link to the image.</b> We require this so images will still be available regardless whether another hosting site is up or down.</blockquote>

<h3>Use Code Tag for Command Line and Source Code</h3>

<blockquote>The code tag can be unpredictable sometimes. Always indent the start of the code tag by one space. When text doesn't line up properly, try adding the start off a code on a new line. If the formatting is still acting weird try adding spaces at different parts of the text wrapped in the code tag. Examples:</blockquote>

<blockquote><pre><code>$ sudo echo "This is a test"</code></pre></blockquote>

<blockquote>Example of unpredictable code tag:</blockquote>

<blockquote><pre><code>A<br>
B<br>
C<br>
D<br>
E<br>
F</code></pre></blockquote>

<blockquote>This  next example is a possible fix for the previous example. Note how we changed the formatting by starting the beginning of the text on a newline. We added a space before "A" so each line starts on the same column. Experiment with adding spaces to get things to line up correctly and looking nice.</blockquote>

<blockquote>Before adding the space before the "A" character:</blockquote>

<blockquote><pre><code><br>
A<br>
B<br>
C<br>
D<br>
E<br>
F</code></pre></blockquote>

<blockquote>After adding the space before the "A" character:</blockquote>

<blockquote><pre><code><br>
A<br>
B<br>
C<br>
D<br>
E<br>
F</code></pre></blockquote>

<blockquote>This is only one demonstration of how unpredictable the code tag can be. Make your best effort to line everything up correctly to keep the formatting looking consistent and nice.