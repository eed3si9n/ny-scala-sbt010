!SLIDE

# quick config dsl
<br>
<br>
sbt 0.10 offers a simple way to describe project settings by using a list
of expressions.
<br>
<br>

to use this light-weight syntax, <br>
create a file called `build.sbt` in the root of your project

!SLIDE

## settings and tasks
<br>
<br>
- [Settings](https://github.com/harrah/xsbt/wiki/Settings) describe a configuration 
which is evaluated at project load time
  - once the project is loaded, settings and their dependencies are fixed
- [Tasks](https://github.com/harrah/xsbt/wiki/Tasks) are executed on demand
  - tasks can be added, changed or removed at execution
  - use `map` or `flatMap` to pass values from one task to another
<br>
<br>

!SLIDE 

## configuration

- [key](http://harrah.github.com/xsbt/latest/sxr/Keys.scala.html)s describe available settings
- configuration methods initialize and assign 

!SLIDE

## field guide to configuration methods
<br>
[harrah/xsbt/wiki/Index](https://github.com/harrah/xsbt/wiki/Index)
<br>
<br>
- methods ending in `=` intialize
- methods beginning with `<` depend on other values
- no semi-colons at the end of lines
- parentheses are not required except when using a function with `~=`

!SLIDE

## bind values
<br>

`:=` initializes a setting that is

- a constant value
- does not depend on any other settings
- will silently overwrite a previously defined value with the same key
<br>
<br>

<pre>
    name := "My Project"
    version := "0.1"
    organization := "myproject"
    scalaVersion := "2.9.0-1"
    publishTo := Some("name" at "url")
<pre/>

!SLIDE

## adding dependencies
<br>
`+=` and `++=` append to a sequence
<br>
<br>      
    libraryDependencies += "org.xyz" %% "xyz" % "2.1"

    libraryDependencies ++= Seq(
	    "net.databinder" %% "dispatch-meetup" % "0.7.8",
	    "net.databinder" %% "dispatch-twitter" % "0.7.8"
    )
    
!SLIDE

## settings that use other settings
<br>      
    libraryDependencies += "org.xyz" %% "xyz" % "2.1"
<br>
is the same as using `<<=` to call apply to update the previous value of 
`libraryDependencies` with another `ModuleID`  
<br>

    libraryDependencies <<= libraryDependencies { deps =>
      deps :+ "org.xyz" %% "xyz" % "2.1"
    }


!SLIDE

## dependencies that use scala version
<br>
`scalaVersion` is itself a Setting, so you need to combine an append operator with
the `<<=` operator:
- `<+=` combines `+=` and `<<=` 
- `<++=` combines `++=` and `<<=`

<br>
<br>
    libraryDependencies <+= scalaVersion( "org.scala-lang" % "scala-compiler" % _ )
    
    libraryDependencies <++= scalaVersion { sv =>
      ("org.scala-lang" % "scala-compiler" % sv) ::
      ("org.scala-lang" % "scalap" % sv) ::
      Nil
    }    

!SLIDE

## filtering
<br>
The `~=` operator applies a function `F => F` to a value.

- the parentheses are required
<br>
<br>
<pre>
defaultExcludes ~= (filter => filter || "*~")
</pre>

!SLIDE

## add maven repos
<br>

    resolvers += "name" at "url"
  
    resolvers ++= Seq("name2" at "url2", 
      "name3" at "url3")


!SLIDE

## define credentials
<br>
<br>
using a file

    credentials += Credentials(Path.userHome / ".ivy2" / 
      ".credentials")
<br>
or inline 

    credentials += Credentials("Some Repo", "myproject.org", 
      "admin", "admin123")

!SLIDE

## initial commands in console
<br>
Use `initialCommands` to
  - limit scope with `in console` or `in console-quick` 
<br>
<br>

    initialCommands := "import myproject._"
    
!SLIDE

## TODO: section on tasks

- tasks without inputs
- tasks

!SLIDE

## TODO: focus on specific tasks

- how to create a target to run your project code
  - configure logging for tasks
- crossbuild for 2.8.1 and 2.9.0-1 (? not exactly sure where this goes)
- publish jar, source and javadoc
- how to use test runners: specs2, scalatest
- other common tasks

!SLIDE

## TODO: talk about scoping?

not sure if Doug will address this in the theory section or it might seem more
natural in Eugene's multiproject section


