**Puppet Training**

DEVOPS
-

### What Is DEVOPS?

DevOps is a culture, movement, or practice that emphasizes the collaboration and communication of both software developers and other information technology professionals while automating the process of software delivery and infrastructure changes.  It aims a establishing a culture and environment where building, testing, and releasing software can happen rapidly, frequently, and more reliably.

DevOps promises to juxtapose the best of all worlds, Development, Operations, and QA, into a single integrated workflow that streamlines deployment pipelines, operational safety and security, and automated testing.

The DevOps toolchain covers a multiplicty of disciplines such as:

> 1. SCM
> 2. CI
> 3. Deployment
> 4. Cloud/IAAS/PAAS
> 5. Monitoring
> 6. DB Management
> 7. Repo Management
> 8. Configuration Management and Provisioning
> 9. Release Management
> 10. Logging
> 11. Build
> 12. Testing
> 13. Containerization
> 14. Collaboration
> 15. Security

While each of these areas is a discipline within itself, the primary topic for the purposes of these materials is that of Configuration Management and Provisioning; Particularly with Puppet.

Ref: [DEVOPS](https://en.wikipedia.org/wiki/DevOps).

### What is Puppet?

Puppet is an Open Source Configuration Management platform.  It runs on many UNIX-like systems as well as on Microsoft Windows, and includes its own declarative language to describe system configuration known as a Domain Specific Language or "DSL".

> DSL - n.  A computer language specialized to a particular application domain

In essence, the Puppet DSL is a language derived for the sole purpose of use within the Puppet ecosystem and use in the Puppet Infrastructure, built specifically for working with resources and types as defined in the Puppet ecosystem of tools.

[Why Puppet](https://docs.puppet.com/guides/introduction.html#why-puppet)?<br>
[The Puppet Language](https://docs.puppet.com/puppet/latest/reference/lang_summary.html)<br>
[What is Puppet?](https://learn.puppet.com/elearning/what-is-puppet) (video)

#### Why a DSL?

A DSL provides a core, syntactic way of addressing the Puppet system that abstracts OS internals away from the task at hand in favor of a simpler, more cohesive method that allows for a lower "barrier to entry" for new and inexperienced users to both the Puppet system in particular and system administration in general.  This eases the transisiotn to Puppet, promoting team and company-wide "buy-in" and fast adoption. 

### How Does It Work?

The Puppet system can be run as standalone servers (masterless) or a Client-Server model (masterful) whereby code written in the Puppet DSL can be applied against a node or grouping of nodes, instantiating a "state" of operation and then maintaining that state automatically.  

Part of the operational characteristic of Puppet is that when it runs against a node to effect a change, that change only occurs once.  In subsequent runs, the Puppet daemon notices that the "desired state" of the system is in compliance with what Puppet is configured to do, and executes no change on the target system.

This characteristic of how Puppet runs is known as "Idempotency".

> Idempotency - n. The property of certain operations in mathematics and computer science that can be applied multiple times without changing the result beyond the initial application.

 Take, for example, an entry that needs to go into /etc/hosts.  If Puppet were to run periodically and place that entry every time, it wouldn't be long before you would have thousands of the exact same entry in the /etc/hosts file.  When an operation is idempotent, it only performs the change the first time, then maintains the state of that change infinitely thereafter until another change gets executed due to a change in the Puppet code.
 
[The Puppet Server](https://docs.puppet.com/puppetserver/2.5/services_master_puppetserver.html)<br>
[The Puppet Agent](https://docs.puppet.com/puppet/4.6/reference/services_agent_unix.html)
 
### Facter
 
The Puppet system implements a system profiling tool known as facter.  Facter is a program that utilizes information it can gain from the system, system utilities such as dmidecode and items from the filesystem and/or the /proc system, and presents them in a simple view and makes them available to your Puppet code as top-scope variables.  Facter is a complex subsystem, and you can learn more about it [here](https://docs.puppet.com/facter/3.4/). 

[Facter](https://docs.puppet.com/facter/3.3/)<br>
[An Intro to Facter](https://learn.puppet.com/elearning/an-introduction-to-facter) (video)

### Puppet Code (The DSL)

Puppet Code, known as the DSL, is a syntactic construct designed to "describe" the configuration of a target node.  This may include any setting, file, or service status that the node has as it's regular characteristic of operation.  As such, Puppet Code has a very well defined design from which to craft code elements to apply against a node.

#### Abstraction

When Luke Kaines (Puppet CEO) undertook to write the Puppet system, he realized that there would need to be a syntactical paradigm through which all code and associated system configuration elements must be utilized.  

First, he decided to use the Ruby programming language.  It was easy to learn, had a large, robust community, and was very fast.  All essential elements of a configuration management platform.

Next, he had to develop an idiom through which to address a system, and consider the creation of various design patterns users would conceptualize configuration into code through.  The main identity of this idiom is one of abstraction.  He understood that if abstractions could be applied in a disciplined, programmed way, that abstractions could be grouped and worked with in larger and smaller "chunks" at will.  These abstractions are the core of Puppet functionality.

#### Puppet Coding

Unlike other programming paradigms, Puppet has its own set of naming of things to refer to the various components that make up usable Puppet Code.  Unlike a Ruby program, a Bash script, or a Perl script, the Puppet ecosystem is divided into multiple parts.

##### Manifests

Manifests are the files on disk that contain Puppet Code.  These files carry a .pp extension, and contain Puppet resources, classes, and associated code elements.  

##### Classes

Classes are a programmatical construct within manifests.  If the manifest is the file on disk, a class is a code element contained within the file.  If you have a background in object oriented programming, you will have some experience with the term "class", but don't be fooled! A "class" in Puppet parlance is not and has nothing to do with object orientation or any sort of "class" you have used in the past.  

To Puppet, a class is simply a programmatical construct consisting of a named block of code, enclosure delimiters, and internal program elements.  A Puppet class follws this convention:

> class foo {<br>
> &nbsp;&nbsp; code<br>
> &nbsp;&nbsp; code<br>
> &nbsp;&nbsp; code<br>
>}

In this example, the name of the block of code is "foo".  The code itself is encapsulated by curly braces {} and can be any number of resources collected together within them.  By naming a section of code in this way, when wishing to use the code contained therein, you simply need refer to it as the class "foo", and you get all code contained therein.

Contained within a module, you can have many manifests containing classes as shown in the example above.  But, what exactly is a "module"?

[Classes](https://learn.puppet.com/elearning/classes) (video)

##### Modules

A Module is a named collection of all associated elements to an individual technology-type within your Puppet code.  If manifests are the files on disk that contain classes and code, mnodules are the directory structure that organizes and contains all items related to a particular technology type including (but not limited to) classes, files, templates, and data.

Modules are the collected structure that you can download from Puppet Labs' "Forge" (a code hsaring site), share with others, and deploy in an automated fashion across your environment and between various segments of your infrastructure.

#### Resources

The first and foremost abstraction the Puppet system operates on is one of resources.  

Resources are referentials that correspond to various types of items within an operating system.  For instance, in any operating system, you have files.  A file is a core resource of any system.  Another one would be a service.  Whether a Windows service or a UNIX daemon, services comprise a core resource type within a system.  Another would be a Package.  In Windows, there are .msi and .exe files and in Linux/UNIX you can have .rpm, .deb, .pkg, and other types of package resources.  

You can continue to break down resource abstractions in a numnber of different ways... Users, Disk mounts, scheduled jobs... as of this writing, there are more than 50 core resource types defined in the Puppet system!

[Resources](https://learn.puppet.com/elearning/resources) (video)<br>
[Resource Relationships](https://learn.puppet.com/elearning/relationships) (video)<br>

#####"The Trinity"

The most common implementation of Puppet resources you will find as you begin to work with the system is a combination of what Puppet users call "the trinity".  This is a combination of a file, a package, and a service resource to configure some subsystem.  For instance:

SSH requires three things to be functional.  First, it must be installed.  This would be known as the "Package" resource element.  Next, there are configuration files in /etc/ssh which would be manage via the "file" resource.  Finally, the SSH daemon must be running and listening on a port to accept external connections from users.  This would be managed by the "service" resource.

Anecdotally, it can be said that better than 70% of the items you need to configure and manage in a Puppet environment will fall into the classification of a File + Package + Service combination of configuration elements.

#### Anatomy of a Puppet Module

Puppet Modules, being a named directory hierarchy that encapsulates all associated components of a particlar technology type's configuration follow a very specific layout.  You can create this layout manually, or you can have Puppet create a boilerplate directory structure for you that you can then modify  and extend to fit your needs.  

The standard Puppet Module layout is:

> DirectoryName<br>
> |_ manifests<br>
> |_ files<br>
> |_ templates<br>
> |_ lib<br>

The main parent directory would be named for the technology type you are configuring.  So, in the case of a module to manage SSH, your "DirectoryName" would be "ssh".  For NTP, "ntp", and so on.

In each directory structure, the bare minimum requirement is to have a "manifests" directory.  You place all your manifest code here.  When placed in the right directory structure on your Puppet master, it becomes available for use by the Puppet master to apply against the nodes of your choosing.

Instead of manuall creating this directory structure, you can also have the Puppet binary dynamically generate it for you.  On a system that has the Puppet binary on it, you can simply run

> puppet module generate _username_-technology

So, in the event of a user named "Bob Jones" that wishes to create an SSH module, their command line to create their module boilerplate would be:

> puppet module generate bjones-ssh
 
Upon running the above command, you will generate a boilerplate Puppet module skeleton designed to get you started in writing a module. The "puppet" command will ask you several questions regarding your module to automatically generate a metadata.json file.  This file will contain important information related to your module that is also used when contributing to the [Puppet Forge](https://forge.puppet.com).  Example questions asked by the module generation routine are:

>What version is this module?  [0.1.0]
-->

This is the semantic versioning compliant version number of your module.  For more information on the format of the version, go to http://semver.org.

>Who wrote this module?  [bjones]
-->

When generating a module as above, this is automatically populated with the _username_ portion of the generate command.  Should you want or need to use a different username, you can supply it during the generate phase, or you can supply it here for the metadata.json file.

>What license does this module code fall under?  [Apache-2.0]
-->

What license are you using?  Apache2?  GPLv2?  Place that information here.  It should comply with the versions specified by [SPDX](http://spdx.org/licenses/).

>How would you describe this module in a single sentence?
-->

A short description of the module and its purpose.  It is here you state the reason you are writing your module, the expected function of it, and outcome of running your module on your company's systems.

>Where is this module's source code repository?
-->

The URL to the repository containing this project.  Most will utilize a GitHub URL.

>Where can others go to learn more about this module?
-->

The URL where a user can find the full documentation for your module.  Also generally a Wiki or documnetation set found at the GitHub project page for this module.

>Where can others go to file issues about this module?
-->

A URL to a ticket tracking & management location for your module.  Again, GitHub provides this for all hosted repos, and is the most common location for this item.


Finally, the generation routine will display the following based on the answers you provide:

>----------------------------------------<br>
>{<br>
>&nbsp; "name": "bjones-ssh",<br>
>&nbsp;  "version": "0.1.0",<br>
>&nbsp;  "author": "bjones",<br>
>&nbsp;  "summary": null,<br>
>&nbsp;  "license": "Apache-2.0",<br>
>&nbsp;  "source": "",<br>
>&nbsp;  "project_page": null,<br>
>&nbsp;  "issues_url": null,<br>
>&nbsp;  "dependencies": [<br>
>&nbsp;    {"name":"puppetlabs-stdlib","version_requirement":">= 1.0.0"}<br>
>&nbsp;  ],<br>
>&nbsp;  "data_provider": null<br>
>}<br>
>----------------------------------------<br>
><br>
>About to generate this metadata; continue? [n/Y]<br>
>--><br>


If you are happy with all the options you see in the metadata.json, you can simply press \<enter> here and Puppet will generate your module boilerplate for you.  In our example above, a directory "ssh" will be created with the following layout:


>ssh -- The outermost directory's name matches the name of the module<br>
>├── examples -- Contains examples showing how to declare the module's classes and defined types<br>
>│   └── init.pp<br>
>├── Gemfile -- Bundler compliant Gemfile to deliver a manifest of needed gems for installation<br>
>├── manifests -- Contains all of the manifests in the module<br> 
>│   └── init.pp -- Contains a class definition. <b>This class's name must match the module's name.</b><br>
>├── metadata.json -- Tracks important information about your module and can configure certain features.<br>
>├── Rakefile -- File that contains Ruby rake tasks for your module.<br>
>├── README.md -- The markdown formatted documentation file for your module.<br>
>└── spec - Contains spec tests fr any plugins in the lib directory.  May also contain full rspec, serverspec, and Beaker tests as well.<br>
>    ├── classes<br>
>    │   └── init_spec.rb<br>
>    └── spec_helper.rb<br>
 
This layout contains the beginnings of all you need to write modules for Puppet and the Puppet Forge.  For the purposes of this tutorial, we will be primarily concerned with the 'manifests' directory.

####Puppet Modules and Autoloading

Were you to begin to place code into your 'manifests' directory, regardless of the amount or quality of that code, Puppet would have no idea where to begin.  it is for this reason that Puppet Labs built an entry point mechanism into the action of loading modules.

In the 'manifests' directory, you may note that the 'puppet module generate' action created a file named 'init.pp'.  This file is the entry point to your module.  It carries the responsibility of:

A. Being present.<br>
B. Containing the main Class Name that matches the parent directory.<br>
C. Loading all code<br>

As we mentioned above, the module directory name (in this case 'ssh') must match the class name for the entry point of the module.  The 'init.pp' file also should carry the responsibility of loading all other classes it needs to perform the function the module was created to handle.  A good example of a layout that works well for autoloading might be:

><b>init.pp</b><br>
> class ssh {<br>
> &nbsp;&nbsp;include ssh::config<br>
> &nbsp;&nbsp;include ssh::install<br>
> &nbsp;&nbsp;include ssh::service<br>
> }

We have here the init.pp file containing the designated class name and 3 examples of include functions being used to load other code.

In the case of Puppet Labs' reasoning here, they used the thought processes contained within the UNIX boot system itself.  The parent process of all processes in UNIX is known as "init".  It is from this process all other processes get loaded, and it is governed by the "inittab" file.

In creating a loading mechanism as an entry point to your code, Puppet Labs followed this model by using the init.pp solely for loading everything else you will be using in your module.

You may note that the include function loads other classes, but the classes are oddly named (in contrast to simply 'class ssh { }'.  This is the core mechanism for class loading in Puppet.  In the same module, you can create as many manifests as you like, they just need to be "namespaced" within the class you have already set up with your 'puppet module generate' command.  

The general layout referenced in the above example closely aligns with the "file-Package-Service" trinity spoken of earlier.  You would create a new manifest with the name "config.pp" that would have within it a class named "ssh::config", an install.pp containing a class "ssh::install" and finally a service.pp containing a class "ssh::service".  Since you are loading each of these namespaces already in your init.pp file, any code you place within them will be loaded and subsequently run. These manifests (to go along with the init.pp) would look like thee following.

> <b>config.pp</b><br>
> class ssh::config {<br>
> &nbsp;&nbsp;code<br>
> &nbsp;&nbsp;code<br>
> &nbsp;&nbsp;code<br>
> }
> 
> <b>install.pp</b><br>
> class ssh::install {<br>
> &nbsp;&nbsp;code<br>
> &nbsp;&nbsp;code<br>
> &nbsp;&nbsp;code<br>
> }
> 
> <b>service.pp</b><br>
> class ssh::service {<br>
> &nbsp;&nbsp;code<br>
> &nbsp;&nbsp;code<br>
> &nbsp;&nbsp;code<br>
> }

When you tell Puppet to load a module, then, it looks for the module you've specified by name.  It descends into the manifests directory specifically looking for init.pp by default.  This is a core functionality built into the Puppet Server product known as "autoloading".

You can do further reading on Puppet modules and autoloading [here](https://docs.puppet.com/puppet/4.6/reference/modules_fundamentals.html) and [here](https://docs.puppet.com/puppet/latest/reference/lang_namespaces.html). And can learn more about the include function versus the "class" function [here](https://docs.puppet.com/puppet/latest/reference/lang_classes.html#declaring-classes).<br>

[Autoloading](https://learn.puppet.com/elearning/classes) (video)

#####Conclusion

Compiling the above information:

Manifests are the files on disk that contain Puppet code.  They carry a *.pp extension.  Modules are a named directory-based organizational structure on disk that contains all manifests, files, templates, and other associated code related to a particular technology type for which you are writing your Puppet code.  

Classes are contained within manifests and are the named code blocks NOT to be confused with Java or other object-oriented class declarations that contain Puppet resources, functions, and other code elements that are read by the Puppet master for the purposes of applying to end nodes in your environment.

Autoloading is the mechanism by which the Puppet Master reads in all your code by descending into your module directroy structure, then the 'manifests' directory and explicitly loads the 'init.pp' file which, in turn, loads all the other code & classes necessary for the proper operation of your Puppet module.


### **Coding**

We have covered the containers within which to hold code, basic types of code, how Puppet loads code, and must now discuss in detail the 'File-Package-Service' "trinity" referred to earlier.

In any subsystem you may wish to install, configure, and utilize, three things will most often be present.  You will have a package you would install into the system, a configuration file you would modify for your purposes, or a daemon/service that would run to service various sorts of requests on a system.  

Fortunately, Puppet has core resources to handle each type of configuration element, and is easy to use.  Take, for instance, the SSH subsystem.  

SSH comes distributed through the Yum package management system, has a configuration directory (once installed) in /etc/ssh that contains two configuration files 'sshd_config' and 'ssh_config', and runs the sshd daemon to service new login requests.  

To express this configuration in Puppet, you would create files to manage the configuration as follows:

**init.pp**

class ssh {<br>
&nbsp;&nbsp;include ssh::config<br>
&nbsp;&nbsp;include ssh::install<br>
&nbsp;&nbsp;include ssh::service<br>
}<br>

In this manifest, we are simply loading each of the classes represented by the following manifests.  It references the classes each manifest declares by class name.  **Please note that the filename before the '.pp' must match the second half of the class declaration!**  i.e., config.pp must have the class ssh::config declared within it to match its own name.

**config.pp**

class ssh::config {<br>
&nbsp;&nbsp;file { '/etc/ssh/sshd\_config':<br>
&nbsp;&nbsp;&nbsp;&nbsp;ensure => 'present',<br>
&nbsp;&nbsp;&nbsp;&nbsp;owner&nbsp;&nbsp;=> 'root',<br>
&nbsp;&nbsp;&nbsp;&nbsp;group&nbsp;&nbsp;=> 'root',<br>
&nbsp;&nbsp;&nbsp;&nbsp;mode&nbsp;&nbsp;=> '0400',<br>
&nbsp;&nbsp;&nbsp;&nbsp;source&nbsp;&nbsp;=> 'puppet:///modules/ssh/sshd\_config',<br>
&nbsp;&nbsp;&nbsp;&nbsp;require&nbsp;&nbsp;=> Package['openssh-server'],<br>
&nbsp;&nbsp;}<br>
}<br>

This manifest utilizes the "File Resource" provided by the Puppet DSL.  It gives you the opportunity to place the file in the apporpiate location, set its owner and permissions, and to declare a location from which to deliver the file on the Puppet Master via the "source" attribute.  More information on the file resource can be found [here](https://docs.puppet.com/puppet/latest/reference/type.html#file).

#####Source Declarations

Two topics on the file resource should be paid special attention to.  First, the "require" line.  You can set interdependencies between manifests, classes, resources, and even files by declaring them in the course of your writing DSL.  You may note that the require attribute is used above.  This simply allows you to make it a requirement that the package openssh-server has been run before placing the sshd\_config file on disk.  You can find more out regarding these metaparameters in the Puppet metaparameter reference found [here](https://docs.puppet.com/puppet/latest/reference/metaparameter.html).

Further information on the format and layout of the "source" metaparameter can be found in the Puppet documentation [here](https://docs.puppet.com/puppet/latest/reference/type.html#file-attribute-source).

#####Templates

In addition to providing static files for placement on a destination node, the source metaparameter can be replaced with the "content" metaparameter, and target a ruby template, allowing for dynamically generated files based on any amount of criteria.  The full documentation for utilizing templating in the Puppet system can be found [here](https://docs.puppet.com/puppet/latest/reference/lang_template.html).

**install.pp**

class ssh::install {<br>
&nbsp;&nbsp;package { 'openssh-server':<br>
&nbsp;&nbsp;&nbsp;&nbsp;ensure => 'installed',<br>
&nbsp;&nbsp;}<br>
}<br>

This manifest specifies the installation of the openssh-server package as you would install it via the package management system on the box.  For instance, were you doing this process manually, you would type:

> yum -y install openssh-server

By utilizing the package resource and naming the package after the system package manager appropriate name, you can install any package available to you from the default repos.  You can learn more about package resources [here](https://docs.puppet.com/puppet/latest/reference/type.html#package).


**service.pp**

class ssh::service {<br>
&nbsp;&nbsp;service { 'sshd':<br>
&nbsp;&nbsp;&nbsp;&nbsp;ensure => 'running',<br>
&nbsp;&nbsp;&nbsp;&nbsp;enable => true,<br>
&nbsp;&nbsp;}<br>
}<br>

The final manifest in this series manages the daemon that runs and listens for connections on port 22 for SSH connections.  It utilizes the "service" resource, and tells Puppet the state in which it should run.  The  service resource has many options you can explore via its documentation [here](https://docs.puppet.com/puppet/latest/reference/type.html#service).


##Variables

Simple coding alone for static resources or placement of particular files would engender significant "code creep", and Puppet would become quite hard to manage.  More efficient and reusable code could be garnered by utilizing variables and parameterization.  

In short, parameterized variables can be viewed like an API to your modules, and variables themselves are implemented in a tradtiional, well-recognized format.

###Basic Variables

Variables in Puppet that you would utilize in your classes follow very basic conventions.  A variable assignment looks as you would expect were you working in Bash or Perl:

> $ensure = 'directory'

to assign a string literal.  You can also utilize operating system components as string literals as well:

> $logdir = '/var/log'

...and then reference it elsewhere in code as a resource or a parameter value:

> file { $logdir:<br>
> &nbsp;&nbsp;ensure => $ensure,<br>
> }<br>

Much like any other scripting or programming construct, variables help by making a "placeholder" for information or values to be used later.

You can use variables as interpreted, literal, or boolean values:

>Literal:
>
>$string = 'My config file is $config_file.'<br>
>&nbsp;&nbsp;produces:
>&nbsp;&nbsp;&nbsp;&nbsp;My config file is $config\_file.<br>
>
>because the values between the single tics ' ' are a string literal.
>
>Interpolated:
>
>$string = "My config file is $config\_file."<br>
>&nbsp;&nbsp;produces:
>&nbsp;&nbsp;&nbsp;&nbsp;My config file is /usr/local/etc/config<br>
>
>because the contents of the "$config\_file" variable get interpolated and the value of that variable populates that space in the string.
>

#####Notes:

A variable's name can be used anywhere its data type can be used.  Puppet will simply replace the variable with its value.

By enclosing your variables with curly braces {}, you avoid ambiguity when coding, and make it clearer when using variables positioned immedately along non-whitespace characters:

> file { "${homedir}/.vim":<br>
> &nbsp;&nbsp;ensure => 'directory',<br>
> }<br>

Please note, however, that these curly braces are only allowed inside strings, and are used to set off a variable value from other non-variable items.  

##Variable Scoping

When you use variables in Puppet, your variables are not immediately global and follow very specific rules regarding visibility and reach.  This is known as "variable scope".  Scoping limits the reach of variables and resource defaults, but does not limit the reach of resource titles nor of resource references.  These are always global.

How is scope organized in the Puppet World?

There are three main scopes with the ability to continue scoping deeper with infinite combinations.  We will refer to these scopes as "local scopes" but will apply the same rules as the main three scope designations.

#####Top Scope

The "highest" scoping in Puppet is known as "Top Scope".  Top Scope consists of any code that is outside any class declaration, type definition, or node definition.  Variables and defaults declared at top scope are available everywhere in the Puppet ecosystem.  For instance, Facter facts and any variables you might declare in your site.pp for any particular environment would be an example of "top scope".

#####Node Scope

Code located inside a node definition (if classifying nodes on the command line) is considered node scope.  Since only one node definition can match a given node, only one node scope can exist at a time.

Variables and defaults declared at node scope are available everywhere **except** top scope.

#####Local Scopes

Code that lives within a class exists in local scope.  Variables and defaults located in a local scope are only available to that scope and its children.

No locally scoped variables and defaults can be "seen" or inherited "upward" toward top scope.  They are only available locally.  These behave similarly to Perl "my" variables.  They are only locally scoped, and cannot be seen outside their current function or usage context.

Since you can continue to build  child scopes to other child scopes infinitely, the same rules apply.  All parent scopes pass variables on to children, but children never upward towards parents.

To learn more regarding scoping behavior and mechanisms, refer to the Puppet Language reference on scope [here](https://docs.puppet.com/puppet/latest/reference/lang_scope.html).<br>

[Inheritance](https://learn.puppet.com/elearning/inheritance)<br>

###Hiera

Hiera is a key/value pair lookup tool based on YAML designed to provide you with a data library of externally accessed data to describe your site.

Hiera uses YAML files to store the data you wish to look up, and is environment aware, extensible, and configurable to your custom needs.  Hiera allows you to write generic, reusable code and re-use the code at will, only conditionally changing the data values you look up to transform how your code is running and/or the values it is using at run time.

One of the most clear ways you can do a Hiera lookup in your code is to mirror variable setting from other languages at the top of a manifest like so:

> $foo = hiera('bar')

...then you simply leverage $foo somewhere in your code later like so:

> file { $foo:<br>
> &nbsp;&nbsp;ensure => 'present',<br>
> }

To learn more about the Hiera lookup mechanism, the documentation at Puppet.com is the best place to go.

[Why Hiera?](https://docs.puppet.com/hiera/3.2/#why-hiera)<br>
[Configuring Hiera](https://docs.puppet.com/hiera/3.2/configuring.html)<br>
[Creating Hierarchies](https://docs.puppet.com/hiera/3.2/hierarchy.html#creating-hierarchies)<br>
[Puppet Labs' Interactive Hiera Demo](http://puppetlabs.github.io/hierademo/#)<br>
[An Introduction to Hiera](https://learn.puppet.com/elearning/an-introduction-to-hiera) (video)<br>

---

###Helpful Training on Puppet

[When to Hiera](http://garylarizza.com/blog/2013/12/08/when-to-hiera/)<br>
[Building a Functional Puppet Workflow - 1](http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-1/) - Puppet Module Structure<br>
[Building a Functional Puppet Workflow - 2](http://garylarizza.com/blog/2014/02/17/puppet-workflow-part-2/) - Roles & Profiles<br>
[Building a Functional Puppet Workflow - 3](http://garylarizza.com/blog/2014/02/18/puppet-workflow-part-3/) - R10k<br>
[Building a Functional Puppet Workflow - 3b](http://garylarizza.com/blog/2014/03/07/puppet-workflow-part-3b/) - More R10k<br>
[R10k and Environments](http://garylarizza.com/blog/2014/03/26/random-r10k-workflow-ideas/)<br>
[R10k and Directory Environments](http://garylarizza.com/blog/2014/08/31/r10k-plus-directory-environments/)<br>
[Workflows Evolved (2015 Update)](http://garylarizza.com/blog/2015/11/16/workflows-evolved-even-besterer-practices/)<br>
<br>
[Video to Explain all of the Above Concepts - "The Refactor Dance"](https://puppet.com/presentations/workshop-doing-refactor-dance-making-your-puppet-modules-more-modular-gary-larizza) - (video)<br>