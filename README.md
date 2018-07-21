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
