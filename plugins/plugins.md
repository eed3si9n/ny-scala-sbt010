!SLIDE
# port a plugin
- good way to learn sbt 0.10
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
## 0.7 plugins

    import sbt._
    
    class Project(info: ProjectInfo) 
        extends DefaultProject(info) 
        with assembly.AssemblyBuilder {
          
      //  inherits `lazy val assembly` 
    }

!SLIDE
## 0.10 plugins

in project/plugins/built.sbt

    libraryDependencies += "com.eed3si9n" %%
      "sbt-assembly" % "0.2"




    

