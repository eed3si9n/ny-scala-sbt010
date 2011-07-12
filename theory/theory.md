!SLIDE
## Part The First

### a bit of sbt _theory_

!SLIDE

## Build Expressionism<sup>&trade;</sup>

defining a build in terms of [expressions](https://github.com/harrah/xsbt/wiki/Settings)

not *assignments*

!SLIDE
## Goal: a unified design

    key in scope bind value

!SLIDE
## Strategy:

# Settings!

!SLIDE
## From a bird's eye
- Setting[T] produce typed values (simple values, tasks)

- Builds are collections of Settings

- Settings can (`<<=`) other settings' result types

- Think unix pipes but more flexible and for setting definitions

- More on Settings later &hellip;

!SLIDE
## Build it _your_ way

on build sizes and flavors

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

# s/actions/tasks/g

!SLIDE

# new friends

## eval (uate an expression)
## inspect (task doc and deps)
## show (run task and show output)
## last (say again?)

!SLIDE

# familiar friends

## ~ (watch and repeat)
## + (against cross version)
## ++ (against all cross versions)
## (compile, clean, ...)

!SLIDE
## documentation as a feature

!SLIDE
## self documenting

`h` is your friend

    > h h
    Alias of 'help'

`alias` is also your friend

    > alias q = exit
    > alias t = test
    > alias \^@^/ = t
    > \^@^/
    > [info] No tests to run.
    > q

!SLIDE
## self aware

`session` is your passenger

    > h session
    > Manipulates session settings ...
    > session list
    > session save-all

!SLIDE
## Learn by <strike>doing</strike> playing

!SLIDE
## Start thinking in terms of key-value
(insert some web-scale pun)

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
    [info] Packaging .../helloworld_2.8.1-0.1-sources.jar ...
    ...

!SLIDE
## key-value

type `publish-local` in the shell

    > publish-local
    [info] Packaging .../helloworld_2.8.1-0.1-sources.jar ...
    ...

- this is looking up a value from `settings`<br>using the key `publish-local`
- values can executable functions (tasks)

!SLIDE
## key-value
`settings` Seq()
![k-v](theory/sbt0.10k-v.png)

!SLIDE
## key dependencies
![deps](theory/sbt0.10k-v2.png)

!SLIDE
## That's it for the basics
