#Rails introduction

Rails is a great backend web framework written on top of Ruby. 

This is not an exhaustive guide. Not even close. For full docs, go [here](http://api.rubyonrails.org/)

##Basic Terminal Commands

All commands can be run with ```-h``` or ```---help``` to list more information.

For a full guide, see [Rails Official Commandline Guide](http://guides.rubyonrails.org/command_line.html)

To create a new app, use the command

```rails new <app-name>```

To open up the Ruby WEBrick server to host your rails app, run:

```rails server```

To generate specific scaffolding or use specific generators, run:

```rails generate <options>```

To interact with the rails app from command line, you can use the rails console via:

```rails console```


##Basic Structure of a Rails app

Excerpt from <cite>Rails: Learn rails Programming in 24 Hours!: The Ultimate Rails Course in 93 Pages or Less!</cite>

<table>
	<thead>
		<tr>
			<td>Folder/File</td>
			<td>Purpose</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>app/</td>
			<td>Contains the models, controllers, helpers, views, assets and mailers for your application. Our focus will be on this folder in this guide.</td>
		</tr>
		<tr>
			<td>bin/</td>
			<td>Includes the rails script will start up your app and may contain other types of scripts used by you to deploy/ run your application.</td>
		</tr>
		<tr>
			<td>config/</td>
			<td>Configure your application's database, routes, and more. This is will covered in detail later on in this book.</td>
		</tr>
		<tr>
			<td>config.ru</td>
			<td>Contains the Rack configuration for servers that are Rack-based and are used to start an app.</td>
		</tr>
		<tr>
			<td>db/</td>
			<td>Includes your current database schema along with the database migrations.</td>
		</tr>				
		<tr>
			<td>Gemfile,<br>Gemfile.lock</td>
			<td>Using these files, you can specify the gem dependencies that your application needs. The Bundler gem uses these files.</td>
		</tr>
		<tr>
			<td>lib/</td>
			<td>Contains extended modules for applications</td>
		</tr>				
		<tr>
			<td>log/</td>
			<td>Contains application log files.</td>
		</tr>
		<tr>
			<td>public/</td>
			<td>The only folder that can be viewed by the world. Includes compiled assets and static files.</td>
		</tr>
		<tr>
			<td>Rakefile</td>
			<td>This particular file locates & loads tasks that are able to run from the command line. Throughout components of Rails, the task definitions are defined. Instead of making any changes to the Rakefile, you should consider adding your own tasks by adding files to the lib/ tasks directory of your app.</td>
		</tr>				
		<tr>
			<td>test/</td>
			<td>Contains fixtures, tests, and numerous other types of test apparatus</td>
		</tr>		
		<tr>
			<td>tmp/</td>
			<td>Temporary files such as pid, cache, and session files.</td>
		</tr>				
		<tr>
			<td>vendor/</td>
			<td>A folder that contains all third-party code. This includes vendored gems in a typical Rails application.</td>
		</tr>	
	</tbody>
</table>


##Naming Convention

Given the database table name ```tweets``` with the data below:

<table>
	<thead>
		<tr>
			<td>id</td>
			<td>status</td>
			<td>name</td>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>1</td>
			<td>Where can I get a good bite to eat?</td>
			<td>Ash</td>
		</tr>
		<tr>
			<td>2</td>
			<td>I just ate some serious brains</td>
			<td>Jim</td>
		</tr>
	</tbody>
</table>

To access it, the naming convention would be an uppercase and singular class ```Tweet.find(1)```.