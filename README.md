Shovel
======

A [scriptcs](https://github.com/scriptcs/scriptcs) script pack enabling the creation of scriptcs (.csx) build scripts.

What is Shovel?
---------------
Shovel provides a task-centric environment that can be used to create build scripts with the full power of .Net.

How does it work?
-----------------
You define a task by specifying the task name, any tasks it may be dependent on, and lastly the action(s) that should be performed by the task.

Example:
--------
The following script defines a task named "DeployNewVersion" which is dependent on a task named "Build". The "Build" task invokes MSBuild to build a C# project. The "DeployNewVersion" task then copies the built application to a target location.

```C#
Require<Shovel>();

"Build"
  .MsBuild(msb =>
  {
    msb.Project = @"c:\my-project\my-project.csproj";
    msb.Targets("Clean", "Compile");
  });

"DeployNewVersion"
  .DependsOn("Build")
  .Do(() =>
  {
    Console.WriteLine("Deploying new version...");
    File.Copy(@"c:\my-project\bin\debug\my-project.exe", @"c:\deployment\my-project.exe");
  });
```

When executing the script, the task(s) to run may be specified as follows:

    scriptcs mybuildscript.csx -- -tasks:DeployNewVersion

A task may declare multiple dependencies eg:

```C#
"DeployNewVersion"
  .DependsOn("Build", "Test")
```

Multiple tasks to run may be specified on the command line eg:

    scriptcs build.csx -- -tasks:Clean,Build,Deploy

Installation
------------
First [install scriptcs](https://github.com/scriptcs/scriptcs#getting-scriptcs). Then change to the directory where you want to create your script (or have an existing one), and use scriptcs to install Shovel to the packages directory:

    scriptcs -install scriptcs.shovel

Now all you have to do is import Shovel in your script using the `Require` statement:

```C#
Require<Shovel>();
```

That's it - go build something!