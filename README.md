# Devoxx introduction to Elixir

## What is Elixir?

- Based on the Erlang VM & OTP

  - Benefits from Erlang's maturity & interoperability
    - Created in 1981
    - v1.0 in 1989 
    - Open sourced in 1994
    - 50% of telecommunications in Europe
    
  - High scalability: 2M concurrent connexions on WhatsApp, 5M on Discord, 2M with Phoenix (main Elixir web framework)
  - Concurrency: light processes, cost almost no memory, like Go & Kotlin's routines
  - Fault tolerance: "Let it crash" & OTP Supervisor
  
- Created in 2011 by José Valim
  - Core contributor on Ruby on rails & Ruby (e.g. multithreading)
  - Created Elixir because he grew annoyed of Ruby's shortcomings
  
 - Functional programming

  - Immutability
  - Pure functions & limited side effects
  - Pattern matching
  - No heavy static typing (e.g. Haskell)
  - Learning curve smoother than other FP languages

## Overview

## Demo

### Tooling

- Package manager of the Erlang world: Hex

- ELixir's own Mix command

- REPL: iex

  - h for help

```
  iex> h
```
  - Can execute code interactively
  
```
  iex> 1 + 1
  2
```

  - Can load projects and use their module

```
  # In your regular terminal, in an Elixir project's folder
  iex -S mix
```

```
  # In your iex console
  iex> HelloWorld.hello
  ** (UndefinedFunctionError) function HelloWorld.hello/0 is undefined (module HelloWorld is not available)
      HelloWorld.hello()
  iex> c "hello_world.ex"
  [HelloWorld]
  iex> HelloWorld.hello
  "world"
```

  - Can also load modules remotely
    - In one terminal, enter:
    
    ```
      iex --sname server
    ```
    
    - In the newly opened iex console:
    
    ```
      iex(server@maxime-laptop)> c "hello_world.ex"
    ```
    
    - In another terminal:
    
    ```
      iex --sname client --remsh "server@maxime-laptop"
    ```
    
    ```
      # You can use the module you've loaded in the other window
      iex(server@maxime-laptop)> HelloWorld.hello
      "world"
    ```
