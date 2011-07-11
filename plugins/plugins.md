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

- [mpeltonen/sbt-idea](https://github.com/mpeltonen/sbt-idea/tree/sbt-0.10)
- [siasia/xsbt-web-plugin](https://github.com/siasia/xsbt-web-plugin)
- [eed3si9n/sbt-appengine](https://github.com/eed3si9n/sbt-appengine)
- [eed3si9n/sbt-assembly](https://github.com/eed3si9n/sbt-assembly)
- [softprops/coffeescripted-sbt](https://github.com/softprops/coffeescripted-sbt)
- [n8han/posterous-sbt](https://github.com/n8han/posterous-sbt/tree/sbt0.10)
- [steppenwells/sbt-sh](https://github.com/steppenwells/sbt-sh)

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
- no more Path. use [sbt.File][3] (pimps to `RichFile`)

  [1]: http://harrah.github.com/xsbt/latest/api/sbt/SettingKey.html
  [2]: http://harrah.github.com/xsbt/latest/api/sbt/Keys$.html
  [3]: http://harrah.github.com/xsbt/latest/api/sbt/RichFile.html

!SLIDE
## step 7: scope the keys

    override lazy val settings = inConfig(Assembly)(Seq(
      assembly <<= packageBin.identity,
      packageBin <<= assemblyTask,
      jarName <<= (name, version) { (name, version) => 
        name + "-assembly-" + version + ".jar" },
      outputPath <<= (target, jarName) { (t, s) => t / s },
      test <<= (test in Test).identity,
    )) ++
    Seq(
      assembly <<= (assembly in Assembly).identity
    )

!SLIDE
## you are here
![plugins k-v](plugins/sbt0.10plugins3.png)

!SLIDE
## why scope?!

!SLIDE
## why scope?!
1. no `assembly-` prefixing the keys
2. reuse existing keys (`name`, `test`, ...)
3. override keys without affecting other tasks

    <pre>> set test in Assembly := {}</pre>

!SLIDE
## why scope?!
1. no `assembly-` prefixing the keys
2. reuse existing keys (`name`, `test`, ...)
3. override keys without affecting other tasks

    <pre>> set test in Assembly := {}</pre>

## => better modularity

!SLIDE
also, what was up with 

      packageBin <<= assemblyTask,

??

!SLIDE
## step 8: the task (style 1)
declare deps inside `Seq()`:

    private assemblyTask(d1: X1, d2: X2, d3: X3,
        d4: X4, d5: X5, d6: X6): File = {
      // returns File
    }
    
    override lazy val settings = inConfig(Assembly)(Seq(
      assembly <<= packageBin.identity,
      packageBin <<= (D1, D2, D3, D4, D5, D6) map {
        (d1, d2, d3, d4, d5, d6) =>
          assemblyTask(d1, d2, d3, d4, d5, d6)
      } 
    )) ++

!SLIDE
## step 8: the task (style 2)
declare deps outside `Seq()`:

    private assemblyTask: Initialize[Task[File]] =
      (D1, D2, D3, D4, D5, D6) map {
        (d1, d2, d3, d4, d5, d6) =>
        // returns File
      }      
    
    override lazy val settings = inConfig(Assembly)(Seq(
      assembly <<= packageBin.identity,
      packageBin <<= assemblyTask 
    )) ++

!SLIDE
## step 9: publish-local
plugins are library loaded into the build.

