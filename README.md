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
  
  - Pure functions & limited side effects, easy to test
  
  - Pattern matching
  
  - No heavy static typing (e.g. Haskell)
  
  - Learning curve smoother than other FP languages

## Overview

## Demo

### Some tips & conventions

- File extensions: .ex (compiled) and .exs (interpreted)

- Comments with #

- Print variables with IO.puts or IO.inspect (inspect also unfolds complex datatypes, out of the box)

- Snake case + variable declaration & initialization:
  ```
    iex> hello_world = "hello"
    iex> hello_world
    "Hello"
  ```

- Prefix unused variables with \_ to indicate they are not used (e.g. in function signature, called arity in Elixir)

- Parentheses are optional if there is no ambiguity (e.g. nested function calls)
  ```
    iex> IO.puts("Hello")
    "Hello"
    
    iex> IO.puts "Hello"
    "Hello"
  ```

- The last line in a function is the returned value

- Function piping
  ```
    iex> IO.puts "Hello"
    "Hello"
  
    iex> "Hello" |> IO.puts
    iex> "Hello"
  ```
  
- Unlike Erlang, can reuse variable names, but they will use a new memory address

### Tooling

- Package manager of the Erlang world: Hex

- Elixir's own Mix command
  ```
  bash> mix
  ```

- REPL: iex

  - h for help
    ```
      iex> h
      
      iex> h(File)
    ```

  - Can execute code interactively
    ```
      iex> 1 + 1
      2
    ```

  - Can load projects and use their module
    ```
      # In an Elixir project's folder
      bash> iex -S mix

      iex> HelloWorld.hello
      ** (UndefinedFunctionError) function HelloWorld.hello/0 is undefined (module HelloWorld is not available)
          HelloWorld.hello()
      
      iex> c "hello_world.ex"
      [HelloWorld]
      iex> HelloWorld.hello
      "world"
    ```

  - Can connect remotely
    - In one terminal:
      ```
        bash> iex --sname server

        iex(server@maxime-laptop)> c "hello_world.ex"
      ```
    
    - In another terminal: 
      ```
        bash> iex --sname client --remsh "server@maxime-laptop"
      
        # You can use the module you've loaded in the other window!
        iex(server@maxime-laptop)> HelloWorld.hello
        "world"
      ```

### Types

- Tip: You can use "i" to inspect a value:
  ```
    iex> i 1
    Term
      1
    Data type
      Integer
    Reference modules
      Integer
    Implemented protocols
      IEx.Info, Inspect, List.Chars, String.Chars
  ```

- Integer: 
  ```
    iex> 1
  ```
  
- Float: 
  ```
    iex> 1.0
  ```
  
- Char: 
  ```
    iex> 'A'
  ```
  
- String: 
  ```
    iex> "Hello"
  ```
  
- Boolean: 
  ```
    iex> true
    iex> false
  ```
  
- Atom, a constant whose only value is its own name:
  ```
    iex> :atom
  ```

- List: 
  ```
    iex> [1, 2, 3, 'A', "A"]
  ```
  
- Map: 
  ```
    iex> %{key: "value"}
    iex> %{"key" => "value"}
  ```

- Tuple: 
  ```
    iex> {1}
    iex> {1, 2}
    iex> {1, 'A', 3, "Hello"}
  ```

- Struct, akin to Objects, more on that later with Nicolas:
  ```
    iex> %User{name: "John", age: 30}
  ```

- Anonymous functions: 
  ```
    iex> hello_func = fn -> "Hello" end
    iex> hello_func.()
    "Hello"
    
    iex> # Can also use them as parameters
    iex> hello_world_func = fn func -> "#{func.()} world" end`
    iex> hello_world_func.(hello_func)
    "Hello world"

### Operators

```
  iex> 1 + 1
  2
  
  iex> 10 / 3
  3.333333333
  
  iex> div 10, 3
  3
  
  iex> "Hello" <> " world"
  "Hello world"
  
  iex> value = "world"
  iex> "Hello #{value}"
  "Hello world"
  
  iex> [1, 2] ++ [3]
  [1, 2, 3]
```
  
### Pattern matching

- Example with a function that returns Tuples:
  ```
    iex> File.read("filename_that_does_not_exist")
    {:error, :enoent}

    iex> File.read("filename_that_exists")
    {:ok, "Hello world"}
  ```
  
- Now with pattern matching:
  ```
    iex> {:ok, message} = File.read("filename_that_exists")
    {:ok, "Hello world"}
    
    iex> {:error, error_type} = File.read("filename_that_does_not_exist")
    {:error, :enoent}
    
    iex> {:ok, message} = File.read("filename_that_does_not_exist")
    ** (MatchError) no match of right hand side value: {:error, :enoent}
    (stdlib) erl_eval.erl:450: :erl_eval.expr/5
    (iex) lib/iex/evaluator.ex:257: IEx.Evaluator.handle_eval/5
    (iex) lib/iex/evaluator.ex:237: IEx.Evaluator.do_eval/3
    (iex) lib/iex/evaluator.ex:215: IEx.Evaluator.eval/3
    (iex) lib/iex/evaluator.ex:103: IEx.Evaluator.loop/1
    (iex) lib/iex/evaluator.ex:27: IEx.Evaluator.init/4

    iex> {:error, error_type} = File.read("filename_that_exists")
    ** (MatchError) no match of right hand side value: {:ok, "Hello world"}
    (stdlib) erl_eval.erl:450: :erl_eval.expr/5
    (iex) lib/iex/evaluator.ex:257: IEx.Evaluator.handle_eval/5
    (iex) lib/iex/evaluator.ex:237: IEx.Evaluator.do_eval/3
    (iex) lib/iex/evaluator.ex:215: IEx.Evaluator.eval/3
    (iex) lib/iex/evaluator.ex:103: IEx.Evaluator.loop/1
    (iex) lib/iex/evaluator.ex:27: IEx.Evaluator.init/4
    
  ```
