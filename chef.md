*Configuration management :
If I want to create a user in one machine then I need to login to that particular server and I create but if I want to  create in a multiple servers then I should manually login and create in all the servers in those type of scenarios I  can also use  scripting  but scripting language requires some standard tasks and I need to right 10-15 lines of code and it may work or may not work in some operating systems.
In this conditions we can use  automation tool , like with 3-4 lines of code we can run a operation.

Imperative vs Declarative
Imperative :  [complex scripting]Writing a lot of code like if I want to deploy a package then I should check previous package isthere or not if it is there then I should write some code for that if it doesn’t have the package then I should write some other operation so like this I should write lot of code

Declarative : [No-complex scripting]With a simple code or  minimum code we can deploy the package.

Configuration Management tools are two types  :
Push Based  and Pull based 
Push Based Model : In Pushed  based Deployment model The Master server push the configurations or softwares or package to the individual servers.
Ex :  Ansible and SaltStack

Pull Based Model  :  In pull  based deployment model the individual servers contact with the master server and download the configurations  and softwares then configure themselves accordingly 
Ex : Chef and Puppet

IDEMPOTENCY :   When we use a chef automation tool to perform a operation suppose if I want to create a user the result of the operation will be same if user already exist or not. If user doesn’t exist then it create a user .

Chef is a ruby based 
Deployment Models : Chef-Zero or Server Client 
Operating Systems : Chef Server - linux only
			    chef client - Linux and Windows.
						
						Example :  Create a User 
						User 'sam'  do
						        Action :create
						end
						
						File '/tmp/seena/login.java' do
						        Content "maklalal"
						        Action :create
						end
						
						
Chef Server Offering :
1.Open Source 
2.On -Premise Enterprise Chef[chef server with support and premium features]
3.Multi-tenant cloud solution [hosted chef server]

Hosted chef server :  https://manage.chef.io/signup
Download chefdk from :   https://downloads.chef.io/chefdk/3.1.0
Search for Red Hat Enterprise Linux  7 >> copy the link and download from Workstation node.
Cd /home/centos >> mkdir downloads >> cd downloads >> wget paste the link >> go to the chefdk package downloaded and run the command >> rpm -ivh chefdk package  >> chef --version >> knife --version

Knife :   Knife  is ued to communicate between workstation and chef server.

====Chef Architecture================== 
Chef workstation      Software Installed	Chefdk   
	- Create
	- Valiadte 
	- apply 

Chef Server
	- Opensource
	- On-premise
	- Hosted chef server
Nodes : yum install chef -y
	- Virtual
	- Cloud
	- Network device
RSA_Public keypairs are used to communicate between workstation==Chef Server====nodes

Every time chef node/client runs and gather information about the node with dedicated utility called OHAI and establish the connectivity with chef server once the connection is authenticated client gets a copy of node object 
 
Chef Terminologies :
Chef Resources : are the fundamental built in functions executed in the background.
Ex :  User package or file related operations.

Recipe :
Multiple or Group of  operations/resources are together in a file called recipe.
Ex : web-server.rb
Cookbook :
Multiple/group of   recipes are called cookbookEx : recipes, templates or other dependencies.
---reusable and shareable units,
Ex : mysql, Docker 
Runlist : Runlist defines the order of execution.
Ex :  When we have a multiple recipes and cookbooks that need to be applied  to a node.


Resources : There are two type of resources
	- Built-in Resources 
	- Custom Resources
Version 12.0 will allows custom resources as well.
https://docs.chef.io  :   we can get all  the information about the resources.

Resource syntax  :
Will be like 4 components :   
	- Type                                            
	- Name                                          
	- Attribute/properties[one or more] and a value for that
	- Action[one or more]
Note :  if we are not providing a action to the resource so every resource will be having default action.
				Ex :
				Type 'name'  do
				     attribute 'value'
				     action :type of action
				End
				Chef DSL :
				<Resource Type>  'Name'   do
					<Attribute> 'Value' 
					<Attribute>  'Value
					<Action> :<value>
				End


				Example
							File '/var/www/html/index.html' do
								Content 'this is a initial file'
								Action :create
							end
Code Creation process :
	- Create 
	- Check      >>>>>> cookstyle <Recipe Name>   is used for syntax check
	- Test        >>>>>>  chef-client --local-mode --why-run <recipe file>   if I want do a smoke test[smoke test is nothing but verification test]/if I want to check in my local .
	- Run

Chef-Zero :  chef zero is very useful to quickly test and validate the behaviour of the cookbooks. Recipes and runlists before uploading data to the actual chef server.


With chef-run, you can run the resource directly from the command-line, converging your targets with a single resource, without creating a cookbook or recipe:
chef-run myhost package ntp action=install

Example: Recipe execution on multiple targets
Run the default recipe from the defined cookbook against two resources: myhost1 & myhost2.
chef-run myhost1,myhost2 /path/to/my/cookbook

Cookbook directory structure : You should create Directory structure manually  
Chef generate  cookbook skypass :  This command will create a cookbook skeleton  structure.
Cookbook has well defined directory structure that stores recipes and other supporting templates and other related documents.
Readme.md : what this cookbook about and how to use it 
Metadata.rb :   contains the name of the cookbook version number and maintainers name and email address License info  and description.
Run-list :  chef-client --runlist "recipe[Cookbook-Name::recipe-name]" if we are not specifying the recipe name then it takes default.rb file.
	- Generate Cookbook Skeleton :  chef generate cookbook skypass
	- Syntax checks :   cookstyle <path to cookbook directory>
	- Dry-Run test :  chef-client  --local-mode --runlist "recipe[skypass-name::recipe-name]" --why-run
	- Real-run test :  chef-client --local-mode --runlist "recipe[skypass::recipe-name]"
If I want to run  multiple recipes then :    chef-client  --local-mode --runlist "recipe[skypass:recipename],recipe[enterprise:recipename]"
If I want to include all the recipes then 
	- You should go to default.rb and include the below lines
	Include_recipe 'skypass::login.'
	Include_recipe 'skypass::logout.'
After this run for syntax error :   cookstyle skypass
	- Create a another cookbook called enterprise and >> cd enterprise >> ls
	- Then you will see a file called metadata.rb  and there you add 
							Depends 'skypass'     
Imporatant :   default.rb will be in recipes folder  Ex :  cookbooks/skypass/recipes/default.rb
Metadata.rb will be in cookbook folder.   Ex : cookbooks/skypass/metadata.rb
Knife :  Knife is used to communicate between the chef server 
Should download  chef starter from hosted chef server
Commands  :  
Check chef-server connectivity from workstation ==> knife ssl check
To upload cookbook on chef-server  ==>  knife cookbook upload <cookbook name>
To check all uploaded cookbooks ==> knife cookbook list
To generate cook book ==> chef generate cookbook <cookbookname>
For recipe Syntax ==>  cookstyle <cookbook name>
For local smoke test ==> chef-client --local-mode  --why-run --runlist "recipe[skypass::default]"
For local run ==>  chef-client --local-mode --runlist "recipe[cookbook::recipe]"
For smoke-test ==>   chef-client --why-run
For real time chef run ==> chef-client 
Fetch the trusted certificates from the hosted chef server ==> knife ssl fetch
To upload a cookbook to the hosted chef server ==> knife cookbook upload <cookbook name>
To see the status ==> knife cookbook list


Bootstrapping : Bootstrapping is a installation process of the chef client/node package from workstation.
If I want to install the chef-client 
in the node as well to register with hosted chef server.
Knife bootstrap  <ipaddress of the node>  -N client.example.com[hostname] --ssh-user centos --sudo --identity-file /home/centos/download/chefsetup.pem
You should run this command from chef-repo/
Knife node list ==> used to check the node list
Knife node show <nodename>
Knife node --help
Knife node run_list  add <nodename> "recipe[skypass]"
Knife cookbook list
Knife ssh --help
If I want to run a command in node server from workstation ==>> knife ssh ssh-user centos --ssh-identity-file <path> 'name:client.example.com' 'sudo chef-client'


After successful installation of chef-client in node after bootstrapping.
Run the command :
Chef-client --version
Chef-client
Chef-client --why-run ==>> build verification test

Ohai : Written in JSON format 

*