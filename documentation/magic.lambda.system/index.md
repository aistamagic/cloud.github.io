
# System related slots for Hyperlambda

This project contains _"system slots"_ to be able to invoke system commands. More specifically the project
contains the following slots.

* __[system.terminal.create]__ - Creates a new terminal process on the server
* __[system.terminal.write-line]__ - Writes a line/command to a previously created terminal process on the server
* __[system.terminal.destroy]__ - Destroys/kills a previously created terminal process on the server
* __[system.execute]__ - Execute the specified command returning the result to caller

By combining the above slots with for instance the magic.lambda.sockets project, you can spawn off terminal/bash
processes on your server, creating _"virtual web based terminal sessions"_ on your server. To create a new
terminal process, use something such as the following.

```
system.terminal.create:my-terminal
   folder:/

   /*
    * STDOUT callback, invoked when something is channeled over STDOUT.
    */
   .stdOut

      /*
       * Do standard out stuff here, with the incoming [.arguments]/[cmd] command.
       */
      log.info:x:@.arguments/*/cmd


   /*
    * STDERROR callback, invoked when something is channeled over STDOUT.
    */
   .stdErr

      /*
       * Do standard out stuff here, with the incoming [.arguments]/[cmd] command.
       */
      log.info:x:@.arguments/*/cmd
```

The above **[.stdOut]** and **[.stdErr]** lambda objects are invoked when output data or error data is
received from the process, allowing you to handle it any ways you see fit. The **[folder]** argument
is the default/initial folder to spawn of the process in. All of these arguments are optional.

The name or the value of the **[system.terminal.create]** invocation however is important. This becomes
a unique reference for you, which you can later use to de-reference the instance, while for instance
feeding the terminal lines of input, using for instance the **[system.terminal.write-line]** slot.
To write a command line to an existing terminal window, such as the one created above, you can use
something such as the following.

```
system.terminal.write-line:my-terminal
   cmd:ls -l
```

The above will execute the `ls -l` command in your previously create _"my-terminal"_ instance, and
invoke your **[.stdOut]** callback once for each line of output the command results in. To destroy
the above created terminal, you can use something such as the following.

```
system.terminal.destroy:my-terminal
```

All terminal slots _requires a name_ to be able to uniquely identify _which_ instance you wish to create,
write to, or destroy. This allows you to create as many terminals as you wish on your server, only restricted
by memory on your system, and/or your operating system of choice.
The terminal slots works transparently for both Windows, Linux and Mac OS X, except of course the commands
you pass into them will differ depending upon your operating system.

**Notice** - If you don't reference a terminal session for more than 30 minutes, the process will be
automatically killed and disposed, and any future attempts to reference it, will resolve in an error.
This is to avoid having hanging processes on the server, in case a terminal process is started, and
then something happens, which disconnects the client, resulting in _"hanging sessions"_.

## How to use [system.execute]

If you only want to execute a specific program in your system you can use **[system.execute]**, and pass in
the name of the command as a value, and any arguments as children, optionally applying a **[structured]** argument
to signifiy if you want each line of input to be returned as a single node or not. Below is an example.

```
system.execute:ls
   structured:true
   .:-l
```

The above will result in something such as follows.

```
system.execute
   .:total 64
   .:"-rw-r--r--  1 thomashansen  staff   495  9 Nov 10:37 Dockerfile"
   .:"-rw-r--r--  1 thomashansen  staff  1084 29 Oct 14:51 LICENSE"
   .:"-rw-r--r--  1 thomashansen  staff   604 29 Oct 14:51 Program.cs"
   .:"drwxr-xr-x  3 thomashansen  staff    96 29 Oct 16:53 Properties"
   .:"-rw-r--r--  1 thomashansen  staff  3154 29 Oct 14:51 Startup.cs"
   .:"-rw-r--r--  1 thomashansen  staff  1458 11 Nov 10:57 appsettings.json"
   .:"-rw-r--r--  1 thomashansen  staff   650  9 Nov 08:26 backend.csproj"
   .:"drwxr-xr-x  3 thomashansen  staff    96  9 Nov 10:23 bin"
   .:"-rw-r--r--  1 thomashansen  staff   700  9 Nov 07:29 dev_backend.csproj"
   .:"drwxr-xr-x  9 thomashansen  staff   288 12 Nov 10:31 files"
   .:"drwxr-xr-x  9 thomashansen  staff   288  9 Nov 19:05 obj"
   .:"drwxr-xr-x  6 thomashansen  staff   192 29 Oct 14:51 slots"
   .:"-rw-r--r--  1 thomashansen  staff  1905 29 Oct 14:51 web.config"
```

**Notice** - If you ommit the **[structured]** argument, or set its value to _"false"_, the result of the
above invocation will return a single string.

## Querying operating system version

In addition to the above slots this project also contains the following slots.

* __[system.os]__ - Returns description of your operating system
* __[system.is-os]__ - Returns true if your underlaying operating system is of the specified type

The last slot above takes an argument such as _"Windows"_, _"OSX_, _"Linux"_, etc, and will return true
of the operating system you are currently running on belongs to the specified family of operating systems.
Below is example usage of both.

```
system.is-os:OSX
system.os
```

## Project website for magic.lambda.system

The source code for this repository can be found at [github.com/polterguy/magic.lambda.system](https://github.com/polterguy/magic.lambda.system), and you can provide feedback, provide bug reports, etc at the same place.

- ![Build status](https://github.com/polterguy/magic.lambda.system/actions/workflows/build.yaml/badge.svg)
- [![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=alert_status)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Bugs](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=bugs)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Code Smells](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=code_smells)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Coverage](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=coverage)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Duplicated Lines (%)](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=duplicated_lines_density)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Lines of Code](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=ncloc)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Maintainability Rating](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=sqale_rating)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Reliability Rating](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=reliability_rating)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Security Rating](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=security_rating)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Technical Debt](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=sqale_index)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)
- [![Vulnerabilities](https://sonarcloud.io/api/project_badges/measure?project=polterguy_magic.lambda.system&metric=vulnerabilities)](https://sonarcloud.io/dashboard?id=polterguy_magic.lambda.system)

## Copyright and maintenance

The projects is copyright of Aista, Ltd 2021 - 2023, and professionally maintained by [AINIRO your friendly ChatGPT website chatbot vendor](https://ainiro.io).
