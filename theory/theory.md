!SLIDE
# theory of sbt 0.10

!SLIDE
## 3 representations
1. shell
2. Quick Configuration DSL
3. Scala code

!SLIDE
## 3 representations
1. shell
2. Quick (aka Light) Configuration DSL
3. Scala code (aka Full Configuration)

!SLIDE
- shell is for running the build

!SLIDE
- shell is for running the build
- Quick Configuration DSL <br>is for defining projects

!SLIDE
- shell is for running the build
- Quick Configuration DSL <br>is for defining projects
- Scala code <br>is for advanced stuff

!SLIDE
## key-value

type `name` in the shell

    > name
    [info] helloworld

!SLIDE
## key-value

type `name` in the shell

    > name
    [info] helloworld

- this is looking up a value from `settings`<br>using the key `name`

!SLIDE
## key-value

type `publish-local` in the shell

    > publish-local
    [info] Packaging /Users/eed3si9n/work/helloworld/target/scala-2.8.1.final/helloworld_2.8.1-0.1-sources.jar ...
    ...

!SLIDE
## key-value

type `publish-local` in the shell

    > publish-local
    [info] Packaging /Users/eed3si9n/work/helloworld/target/scala-2.8.1.final/helloworld_2.8.1-0.1-sources.jar ...
    ...

- this is looking up a value from `settings`<br>using the key `publish-local`

!SLIDE
## key-value
`settings` Seq()
![k-v](theory/sbt0.10k-v.png)
