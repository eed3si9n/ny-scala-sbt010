!SLIDE
# port a plugin
- a good way to learn sbt 0.10
- helps migration

!SLIDE
# plugins
> a way to use external code in a build definition

!SLIDE
# plugins
- custom tasks
- custom commands[*][1]
- custom settings

  [1]: http://groups.google.com/group/simple-build-tool/browse_frm/thread/7344d891f0a941a5/edfae75e124d255e
  
!SLIDE

- sbt-idea
- xsbt-web-plugin
- sbt-appengine
- sbt-assembly
- posterous-sbt
- coffeescripted-sbt
- sbt-sh 

!SLIDE
## using 0.7 plugins

    import sbt._
    
    class Project(info: ProjectInfo) 
        extends DefaultProject(info) 
        with assembly.AssemblyBuilder {
          
      //  inherits `lazy val assembly` 
    }

!SLIDE
## using 0.7 plugins
![uml](plugins/sbt0.7pluginsUML.png)

!SLIDE
## using 0.10 plugins

in project/plugins/built.sbt

    libraryDependencies += "com.eed3si9n" %%
      "sbt-assembly" % "0.2"

!SLIDE
## using 0.10 plugins
![plugins k-v](plugins/sbt0.10plugins.png)

!SLIDE
## using 0.10 plugins
![plugins k-v](plugins/sbt0.10plugins2.png)

!SLIDE
## 0.10 plugins internal
![plugins k-v](plugins/sbt0.10plugins3.png)

!SLIDE
## step 1: build.sbt

    sbtPlugin := true

    name := "sbt-assembly"

    organization := "com.eed3si9n"

    version := "0.3"

!SLIDE
## step 2: extend [Plugin][1]
AssemblyPlugin.scala

    package assembly
    
    import sbt._
    import Keys._

    object AssemblyPlugin extends Plugin { 
    }

  [1]: http://harrah.github.com/xsbt/latest/api/sbt/Plugin.html

!SLIDE
## step 3: define [TaskKey][1]

    object AssemblyPlugin extends Plugin { 
      val assembly = TaskKey[File]("assembly", "Builds a single-file deployable jar.")
    }

  [1]: http://harrah.github.com/xsbt/latest/api/sbt/TaskKey.html

!SLIDE
## step 4: hook up the key

    object AssemblyPlugin extends Plugin { 
      val assembly = TaskKey[File]("assembly", "Builds a single-file deployable jar.")
      
      override lazy val settings = Seq(
        assembly <<= (X, Y, Z) map { (x, y, z) =>
          // expression that returns File }
      )
    }

!SLIDE
## you are here
![plugins k-v](plugins/sbt0.10plugins2.png)

!SLIDE
## step 5: define a scope

    object AssemblyPlugin extends Plugin { 
      val Assembly = config("assembly") extend(Runtime)
      val assembly = TaskKey[File]("assembly", "Builds a single-file deployable jar.")
      ...

!SLIDE
## step 6: define [SettingKey][1]s

    object AssemblyPlugin extends Plugin { 
      val Assembly = config("assembly") extend(Runtime)
      val assembly = TaskKey[File]("assembly", "Builds a single-file deployable jar.")
      
      val jarName    = SettingKey[String]("jar-name")
      val outputPath = SettingKey[File]("output-path")
      ...

  [1]: http://harrah.github.com/xsbt/latest/api/sbt/SettingKey.html

!SLIDE
## step 6: define [SettingKey][1]s

      ...
      val jarName    = SettingKey[String]("jar-name")
      val outputPath = SettingKey[File]("output-path")
      ...

- no `assembly` prefix here
- reuse predefined [Keys][2] (`name`, `test`, ...)

  [1]: http://harrah.github.com/xsbt/latest/api/sbt/SettingKey.html
  [2]: http://harrah.github.com/xsbt/latest/api/sbt/Keys.html
  


