# Francis

Simple boilerplate killer using Plug and Bandit inspired by [Sinatra](https://sinatrarb.com) for Ruby

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed
by adding `francis` to your list of dependencies in `mix.exs`:

```elixir

def deps do
  [
    {:francis, "~> 0.1.0"}
  ]
end
```
## Usage

To start the server up you can run `mix francis.server` or if you need a iex console you can run with `iex -S mix francis.server`

## Example of a router

```elixir
defmodule Example do
  use Francis

  get("/", fn _ -> "<html>world</html>" end)
  get("/:name", fn %{params: %{"name" => name}} -> "hello #{name}" end)

  ws("ws", fn "ping" -> "pong" end)

  unmatched(fn _ -> "not found" end)
end
```
## Example of a router with Plugs

With the `plugs` option you are able to apply a list of plugs that happen between before dispatching the request.


In the following example we're adding the `Plug.BasicAuth` plug to setup basic authentication on all routes

```elixir
defmodule Example do
  import Plug.BasicAuth

  use Francis, plugs: [{:basic_auth, username: "test", password: "test"}]

  get("/", fn _ -> "<html>world</html>" end)
  get("/:name", fn %{params: %{"name" => name}} -> "hello #{name}" end)

  ws("ws", fn "ping" -> "pong" end)

  unmatched(fn _ -> "not found" end)
end
```

## Example using it with Mix.install

In your `iex` instance run:
```elixir
Mix.install([{:francis, "~> 0.1"}])

defmodule Example do
  use Francis

  get("/", fn _ -> "<html>world</html>" end)
  get("/:name", fn %{params: %{"name" => name}} -> "hello #{name}" end)

  ws("ws", fn "ping" -> "pong" end)

  unmatched(fn _ -> "not found" end)

  def start(_, _) do
    children = [{Bandit, [plug: __MODULE__]}]
    Supervisor.start_link(children, strategy: :one_for_one)
  end
end

Example.start()
```

Check the folder [example](https://github.com/filipecabaco/francis/tree/main/example) to check the code.
