# SystemSettings
A future library which will take responsibility for getting and validating settings for a system from a variety of different sources.

### The problem
I'm writing a .NET application but I want a simple and standardised way of consuming and using configuration settings. I need to validate mandatory settings exist and verify the value of settings where possible according to validation rules.

### A possible solution
The host application should be able to use pre-verified settings by referencing a set a read-only properties (or similar). The source(s) of settings should be abstracted away by the library. We should be able to change a setting to a different source without requiring a modification to the host application.

### Where do settings come from?
 * The web.config file on .NET Framework web applications.
 * The app.config file on .NET Framework desktop applications and services.
 * The appSettings.json file in .NET Core projects.
 * Environment variables.
 * A custom configuration file (such as an XML, JSON or simple text file).
 * A database (such as SQL Server, SQLite or a NoSQL database).

### Why the name SystemSettings?
This may change but it seems like a good idea *"for now"*. I wanted a name that communicated in code what the intention of the library was (i.e. it gives you settings for the system you are creating) but could not be easily confused with existing configuration objects and methods to interact with them (e.g. ConfigurationManager, IConfiguration). This library will use those objects to get settings but the host application will only need to know about SystemSettings.

### Initial thoughts on technical design *(heavily subject to change)*
This will be a .NET Standard 2.0 library which is intended to be used in both .NET Framework and .NET Core projects. *It may be possible to target a lower version of .NET Standard but most of the recent configuration changes are in 2.0. Will need to figure it out when I get into the details.*

Create configuration sources as *plug-in* modules: A module just for getting environment variables or a module for web.configs.

Create validation rules as independent modules: A validation rule should not care about the source of the setting or the host application consuming the setting. It just validates a value or set of values according to a rule.

Multiple sources for a configuration setting and fallbacks: A configuration setting could be expected in one source but fallback to another source. For example: *I expect a database connection string to be in an environment variable I can use a connection string in a configuration file instead* **OR** *If I have a connection string in an environment variable and a configuration file, I choose the configuration file connection string to take precedence according to a rule*

It should be able to return all configuration errors in a single object: If there are five errors in different system settings, I should receive a response object with a list of all five errors *(i.e. I don't want to fail on the first error and force a user to correct an error to "discover" the next problem)*.

### Advanced feature *(probably reserved for version 2)*
Do we need to implement a lazy loading mechanism? For example, if settings are in the database, and you don't need the settings right now, you could grab them on the first instance you need them.

How do we refresh settings? Some advantages of existing configuration objects built into .NET are that they can intelligently refresh settings at runtime if they see a change. We don't want to lose this functionality if possible. Can we easily hook into these in SystemSettings? *Can you consume events to capture changes in configuration sources?*
