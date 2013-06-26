# Grails Glassfish plugin
A plugin for Grails, that makes Glassfish the default app server during development. 

It may be useful, when you have Glassfish application server in your production environment, and want to test your software during development in production-like environment.

## Installation

Just now, there are several ways of getting plugin installed. First, the default for any common Grails plugin, is the most simple way. But if for some reason, it's not your variant, you can chose any other way.

### Default Grails-way
Like usual Grails plugin, Glassfish plugin is available from Grails Central Plugin Repository. And you can install it simply by adding a dependency to BuildConfig.groovy:

    plugins {
       build ':glassfish:0.1.2'
    }

### Get ready-to-use plugin
You can get packaged and ready-to-use plugin. First, download it:

[latest version of plugin](https://dl.dropbox.com/u/8513842/grails-glassfish-0.1.2.zip)

And then install it:

`grails install-plugin /path/where/your/donwload/plugin`

### Build from source and install
Finally, you can manually build plugin from source on your local machine, and then install it. You need checkout last version of sources of plugin:

`git clone https://github.com/kiRach/grails-glassfish.git`

Then you need to package plugin:

`grails package-plugin`

And finally, you need to install packaged plugin:

`grails install-plugin /path/to/packaged/plugin`

## Using
First of all, you need to remove other app server plugin, if you have it already installed(cuz it can cause confusion). For example, if you have Tomcat app server plugin installed(default behavior for any newly created Grails application), you need remove it deleting its dependency in BuildConfig.groovy.

The same is for any other app servers(jetty, etc.)

Then, you can just run

`grails run-app`

and now you have working Grails application under Glassfish app server.

## Additional config params
You can specify some config params, if you need it. Each config param should be specified in `BuildConfig.groovy` file, and should have this semantic:

```
grails.glassfish.${PARAM_NAME} = ${PARAM_VALUE}
```

For example, if you want to enable websocket support, you need to specify in `BuildConfig.groovy` file:

```
grails.glassfish.websocket = true
```

For now, available params are:
+ ***websocket***: `true` or `false`
Let you enable websocket support.

+ ***comet***: `true` or `false`
Let you enable comet support.

P.S.: if you need some additional params, you can email me on kirach@kirach.net or leave your request in [issues](https://github.com/kiRach/grails-glassfish/issues).

## Notes

+ For now, plugin supports only using `run-app` command. It doesn't support `run-war` command or using https. If one of this cases is your and you need it to be supported by plugin, email me on kirach@kirach.net or leave your request in [issues](https://github.com/kiRach/grails-glassfish/issues).

+ When you will run your application, using this plugin, you can see in console some errors, reporting by slf4j. Like this ones:
```
| Error SLF4J: Class path contains multiple SLF4J bindings.
| Error SLF4J: Found binding in [jar:file:/home/kirach/.grails/ivy-cache/org.slf4j/slf4j-log4j12/jars/slf4j-log4j12-1.6.6.jar!/org/slf4j/impl/StaticLoggerBinder.class]
| Error SLF4J: Found binding in [jar:file:/home/kirach/.grails/ivy-cache/org.glassfish.main.extras/glassfish-embedded-all/jars/glassfish-embedded-all-3.1.2.jar!/org/slf4j/impl/StaticLoggerBinder.class]
| Error SLF4J: Found binding in [jar:file:/home/kirach/.grails/ivy-cache/org.grails/grails-plugin-log4j/jars/grails-plugin-log4j-2.0.3.jar!/org/slf4j/impl/StaticLoggerBinder.class]
| Error SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
| Error SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
```
It's normal behavior and you can ignore this messages. Glassfish embedded server(this is used by plugin) is distributed with "built-in" slf4j dependencies, and Grails uses same dependencies too(but other versions). That's why this messages appears(signaling about multiples slf4j bindings found). But as a result, slf4j chooses first founded binding and uses it. Plugin is constructed with respect of this behavior.