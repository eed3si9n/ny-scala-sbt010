!SLIDE
# sbt 0.10
maps and a sea of keys and value bindings

<br>
[@eed3si9n](https://twitter.com/#!/eed3si9n)
[@rktoomey](https://twitter.com/#!/rktoomey)
[@softprops](https://twitter.com/#!/softprops)<br>
[meetu.ps/2sKqN](http://meetu.ps/2sKqN)

!SLIDE
## Part The First

### a bit of sbt _theory_

!SLIDE

## Build Expressionism<sup>&trade;</sup>

defining a build in terms of [expressions](https://github.com/harrah/xsbt/wiki/Settings)

not *assignments*


!SLIDE
## Goal: a unified design

    key in scope bindingFn value

!SLIDE
## Result:

- Almost everything is a Setting[T]

- Builds are collections of Settings

- Settings can (`<<=`) other settings result types

- More on this later &hellip;

!SLIDE
## Build it _your_ way
1. (small) shell
2. (medium) [Quick](https://github.com/harrah/xsbt/wiki/Basic-Configuration) Configuration
3. (large) [Full](https://github.com/harrah/xsbt/wiki/Full-Configuration) Configuration

!SLIDE
## In other words
1. `> eval ... # toe dipping`
2. `build.sbt # single project`
3. `build.scala # family of projects`

!SLIDE
## Ghost in the > shell

or &ldquo;manning sbt from your terminal&rdquo;

!SLIDE

## eval
## inspect
## show
## last

!SLIDE

plus familiar friends

## ~
## +
## ++

!SLIDE

## self documenting

`h` is your friend

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

!SLIDE
## key dependencies
![deps](theory/sbt0.10k-v2.png)

!SLIDE


