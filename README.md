![Elixir](http://elixir.gbaptista.com/assets/icons/elixir-24-a7a7a95fc248f0c3c00f770363e07d0e6c3166f499a0beb06c21a85779eaddcb.png) ![Love](http://elixir.gbaptista.com/assets/icons/heart-24-32727c879e61bc07a0ed0ae911c9dcd6363b27c7705f7ede2f3557c1c3f41c77.png) Elixir Online Coding
=================
[http://elixir.gbaptista.com](http://elixir.gbaptista.com)

This repository is currently used for [issue tracking](https://github.com/gbaptista/elixir.gbaptista.com/issues) and you can also contribute to the official demos:

## Elixir Demos

#### Default snippets:

- Playground: [What time is it?](#what-time-is-it)
- Test Suite: [ExUnit](#exunit) - [Unit tests: Functions](#unit-tests-functions)

#### All demos:

* [Playground](#playground)
  - [What time is it?](#what-time-is-it)
  - [Hello World](#hello-world)
  - [Current Elixir infos](#current-elixir-infos)

* [ExUnit](#exunit)
  - [Behaviour Description](#behaviour-description)
  - [Unit tests: Empty template](#unit-tests-empty-template)
  - [Unit tests: Doctests](#unit-tests-doctests)
  - [Unit tests: Functions](#unit-tests-functions)

* [ExSpec](#exspec)
  - [Spec tests: Empty template](#spec-tests-empty-template)
  - [Spec tests: Functions](#spec-tests-functions)

* [ESpec](#espec)
  - [Spec tests: Empty template](#spec-tests-empty-template-1)
  - [Spec tests: Functions](#spec-tests-functions-1)

### Playground

_Elixir Version: **1.3.2**_

#### What time is it?

Playground Code:
```elixir
IO.puts "What time is it?" <> "\n"

IO.puts DateTime.utc_now()

IO.puts "\nTime zone: '#{DateTime.utc_now.time_zone}'"
```
Snippet: [ktEkLQ](http://elixir.gbaptista.com/playground/ktEkLQ?demo=true)

#### Hello World

Playground Code:
```elixir
IO.puts "Hello world from Elixir"
```
Snippet: [orwlAw](http://elixir.gbaptista.com/playground/orwlAw?demo=true)

#### Current Elixir infos

Playground Code:
```elixir
IO.puts "Current Elixir infos:\n"

IO.puts "Elixir Version: #{System.version}\n"

IO.puts "Build info:\n\n#{inspect(System.build_info)}\n"

IO.puts "Erlang/OTP Version: #{System.otp_release}"
```
Snippet: [z0E9Tw](http://elixir.gbaptista.com/playground/z0E9Tw?demo=true)

### ExUnit
_Elixir Version: **1.3.2**_ | _WhiteBread Version: **2.7.0**_

#### Behaviour Description
Plain Text Behaviour Description:
```gherkin
Feature: Serve coffee
  Coffee should not be served until paid for
  Coffee should not be served until the button has been pressed
  If there is no coffee left then money should be refunded

  Scenario: Buy last coffee
    Given there are 1 coffees left in the machine
    And I have deposited 1 pound
    When I press the coffee button
    Then I should be served a coffee
```

Test Code:
```elixir
defmodule SunDoe.CoffeeShopContext do
  use WhiteBread.Context

  given_ "there are 1 coffees left in the machine", fn state ->
    {:ok, state |> CoffeeStore.put_coffees(1)}
  end

  given_ ~r/^I have deposited (?<pounds>[0-9]+) pound$/,
  fn state, %{pounds: pounds} ->
    {:ok, state |> CoffeeStore.put_coffees(pounds)}
  end

  when_ "I press the coffee button", fn state ->
    # Domain logic to serve coffees would happen
    # here. Then update the state with the result
    {:ok, state |> CoffeeStore.serve_coffee(1)}
  end

  then_ "I should be served a coffee", fn state ->
    served_coffees = state |> CoffeeStore.coffee_served

    # The context automatically imports ExUnit.Assertions
    # so any usual assertions can be made
    assert served_coffees == 1

    {:ok, :whatever}
  end
end
```

Source Code:
```elixir
defmodule CoffeeStore do
  def put_coffees(state, n), do: Map.put(state, :coffees, n)
  def put_pounds(state, n),  do: Map.put(state, :pounds, n)

  def serve_coffee(state, n) do
    Map.put(state, :coffees_served, n)
  end

  def coffee_served(state) do
    Map.get(state, :coffees_served)
  end
end
```

Playground Code:
```elixir
IO.inspect CoffeeStore.coffee_served %{coffees_served: 3}
```

Snippet: [LdapqQ](http://elixir.gbaptista.com/test/LdapqQ?demo=true)

#### Unit tests: Empty template

Test Code:
```elixir
defmodule ExampleTest do
  use ExUnit.Case

  test "the truth" do
    assert true == true
  end
end
```

Snippet: [QezOsg](http://elixir.gbaptista.com/test/QezOsg?demo=true)

#### Unit tests: Doctests

Test Code:
```elixir
defmodule KVServer.CommandTest do
  use ExUnit.Case, async: true
  doctest KVServer.Command
end
```

Source Code:
```elixir
defmodule KVServer.Command do
  @doc ~S"""
  Parses the given `line` into a command.

  ## Examples

      iex> KVServer.Command.parse "CREATE shopping\r\n"
      {:ok, {:create, "shopping"}}

  """
  def parse(line) do
    case String.split(line) do
      ["CREATE", bucket] -> {:ok, {:create, bucket}}
    end
  end
end
```

Playground Code:
```elixir
IO.puts inspect KVServer.Command.parse "CREATE shopping\r\n"
```

Snippet: [D6oVig](http://elixir.gbaptista.com/test/D6oVig?demo=true)

#### Unit tests: Functions
Test Code:
```elixir
defmodule TestMyFunctions do
  use ExUnit.Case

  test "add" do
    assert MyFunctions.sum(2, 2) == 4
    assert MyFunctions.sum(5, 3) == 8
  end

  test "multiply" do
    assert MyFunctions.multiply(3, 2) == 6
    assert MyFunctions.multiply(4, 8) == 32
  end
end
```

Source Code:
```elixir
defmodule MyFunctions do
  def sum(x, y) do
    x + y
  end

  def multiply(x, y) do
    x * y
  end
end
```

Playground Code:
```elixir
IO.puts "2 + 2 = #{MyFunctions.sum(2, 2)}"
IO.puts "3 x 2 = #{MyFunctions.multiply(3, 2)}"
```

Snippet: [zLlwwA](http://elixir.gbaptista.com/test/zLlwwA?demo=true)

### ExSpec

_Elixir Version: **1.3.2**_ | _ExSpec Version: **2.0.0**_

#### Spec tests: Empty template

Test Code:
```elixir
defmodule MyTest do
  use ExSpec, async: true

  context "test something" do
    it "assert true", do: assert true == true
  end
end
```

Snippet: [MhS5Ew](http://elixir.gbaptista.com/test/MhS5Ew?demo=true)

#### Spec tests: Functions

Test Code:
```elixir
defmodule MyFunctionsTest do
  use ExSpec, async: true

  context "sum" do
    it "sum x + y" do
      assert MyFunctions.sum(5, 3) == 8
    end
  end

  context "multiply" do
    it "multiply x * y" do
      assert MyFunctions.multiply(4, 8) == 32
    end
  end
end
```

Source Code:
```elixir
defmodule MyFunctions do
  def sum(x, y) do
    x + y
  end

  def multiply(x, y) do
    x * y
  end
end
```

Playground Code:

```elixir
IO.puts "2 + 2 = #{MyFunctions.sum(2, 2)}"
IO.puts "3 x 2 = #{MyFunctions.multiply(3, 2)}"
```

Snippet: [4aJUyw](http://elixir.gbaptista.com/test/4aJUyw?demo=true)

### ESpec

_Elixir Version: **1.3.2**_ | _ESpec Version: **0.8.26**_

#### Spec tests: Empty template

Test Code:
```elixir
defmodule SomeSpec do
  use ESpec

  context "test something" do
    it do: expect true |> to(be_true)
  end
end
```

Snippet: [vIsWCg](http://elixir.gbaptista.com/test/vIsWCg?demo=true)

#### Spec tests: Functions

Test Code:
```elixir
defmodule MyFunctionsSpec do
  use ESpec

  context "sum" do
    it "sum x + y" do
      expect(MyFunctions.sum(5, 3)).to eq(8)
    end
  end

  context "multiply" do
    it "multiply x * y" do
      expect(MyFunctions.multiply(4, 8)).to eq(32)
    end
  end
end
```

Source Code:
```elixir
defmodule MyFunctions do
  def sum(x, y) do
    x + y
  end

  def multiply(x, y) do
    x * y
  end
end
```

Playground Code:
```elixir
IO.puts "2 + 2 = #{MyFunctions.sum(2, 2)}"
IO.puts "3 x 2 = #{MyFunctions.multiply(3, 2)}"
```

Snippet: [TK22cQ](http://elixir.gbaptista.com/test/TK22cQ?demo=true)
