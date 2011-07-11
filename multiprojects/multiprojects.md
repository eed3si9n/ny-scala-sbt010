!SLIDE
# multi projects
- better than 0.7's
- see wiki's [full configuration](https://github.com/harrah/xsbt/wiki/Full-Configuration)

!SLIDE
## full configuration

    import sbt._

    object FooBuild extends Build {
      lazy val root = Project("root",
        file(".")) aggregate(cli, scalaxbPlugin)
      lazy val cli = Project("scalaxb", file("cli")) 
      lazy val scalaxbPlugin = Project("sbt-scalaxb", 
        file("sbt-scalaxb")) dependsOn(cli)
  
      // common settings
      override lazy val settings = super.settings ++ Seq(
        ...
      )
    }

!SLIDE
## subproject

    lazy val cli = Project("scalaxb", file("cli")) 

- creates scalaxb project under `cli` dir
- details can go into cli/build.sbt

!SLIDE
## 3 ways to reference
1. Project object

        lazy val cli = Project("scalaxb", file("cli")) 

2. RootProject at `uri` or `file`

        RootProject(file("/home/user/a-project"))

3. ProjectRef within `uri` or `file`

        ProjectRef(
          uri("git://github.com/dragos/dupcheck.git"),
          "project-id")

!SLIDE
## 3 ways to relate
1. execution dependency<br>- task on `root` also runs on `cli`
2. classpath dependency<br>- use classpath from other projs
3. configuration dependency<br>- when a key is not found<br>it looks up another proj

!SLIDE
## execution dependency

    lazy val root = Project("root",
      file(".")) aggregate(cli, scalaxbPlugin)

running `compile` now compiles both `cli` and `scalaxbPlugin`

!SLIDE
## classpath dependency

    lazy val scalaxbPlugin = Project("sbt-scalaxb", 
      file("sbt-scalaxb")) dependsOn(cli)

use code without `publish-local`

!SLIDE
## configuration dependency

    lazy val root: Project = Project("root",
      file(".")) aggregate(ui)   
    lazy val ui = Project("ui",
      file("ui"), delegates = root :: Nil)  

if a key is not found in ui/build.sbt it checks build.sbt.<br>
- [broken in sbt 0.10][1]

  [1]: https://groups.google.com/group/simple-build-tool/browse_thread/thread/1f5fbf8b90027e73

!SLIDE
## configuration dependency
workaround is to put common settings in build.scala.

    override lazy val settings = super.settings ++ Seq(
      version := "0.6.2-SNAPSHOT",
      organization := "org.scalaxb",
      scalaVersion := "2.9.0-1",
      ...
    )

sbt 0.10.1 will be out soon with the fix
