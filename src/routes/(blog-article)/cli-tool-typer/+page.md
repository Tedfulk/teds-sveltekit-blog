---
slug: cli-tool-typer
title: Writing CLI Tools With Typer
date: 2024-03-14T00:00:00.000Z
excerpt: Discover the key features of Typer for creating command line interfaces in Python.
coverImage: /images/posts/Typer-Key-Features.JPEG
tags:
  - Python
  - CLI tools
  - Typer
---

<script>
  import Callout from "$lib/components/molecules/Callout.svelte";
  import CodeBlock from "$lib/components/molecules/CodeBlock.svelte";
  import Image from "$lib/components/atoms/Image.svelte";
</script>

# Introduction

Typer is an open-source library that allows developers to create command line interfaces (CLI) easily and efficiently. It's built on top of Python's Click, offering more type hinting support and a more user-friendly approach. This post will cover all the key features of Typer that you should know when writing your own CLI tool.

## Key Features

<details>

<summary><b> Type Hinting </b></summary>

Typer supports type annotations for function parameters, making your code easier to read and maintain.

</details>

<details>

<summary><b> Command Auto-Completion </b></summary>

Typer provides command auto-completion out of the box, reducing the time spent typing commands and improving user experience.
</details>

<details>

<summary><b> Validation & Error Handling </b></summary>

With Pydantic integration, Typer allows robust validation and error handling for input data, ensuring your CLI tool functions smoothly.

</details>

<details>

<summary><b> Rich Command Help & Descriptions </b></summary>

Typer offers rich command help documentation that can be easily extended with additional context or examples.

<CodeBlock lang="python">

```python
import typer

app = typer.Typer()

def main(
    hero: str = typer.Argument(..., help="The hero's name to greet"),
    power: str = typer.Option("Super Strength", help="The hero's superpower", show_default=True),
):
    """
    Greet a Marvel hero and mention their superpower.
    """
    print(f"Hello {hero}, renowned for your {power}!")

if __name__ == '__main__':
    app()

```

</CodeBlock>

This example demonstrates how to create a simple CLI tool that greets a Marvel hero and mentions their superpower. By using type annotations and the typer.Argument and typer.Option decorators, the CLI tool automatically provides a rich help message that includes information about each parameter, making it clear and user-friendly.

</details>

<details>

<summary><b> Callbacks & Event Handling </b></summary>

Typer allows you to define callback functions that are executed before or after a command is run, giving you granular control over the command execution process. Here's an example of how to use event handlers in Typer:

<CodeBlock lang="python">

```python
import typer

app = typer.Typer()

def endgame_outcome(ctx: typer.Context, result: Any):
    """
    After calculating the outcomes, display a summary related to Iron Man's sacrifice.
    """
    if result == 1:
        typer.echo("In this future, victory was achieved through Iron Man's sacrifice.")
    else:
        typer.echo("In all other futures, we faced defeat.")

@app.command()
def calculate_futures():
    """
    Simulate Doctor Strange's calculation of the alternate futures.
    """
    # Simulating the scenario where out of 14,000,605 futures, only 1 results in victory.
    total_futures = 14_000_605
    winning_futures = 1
    return winning_futures

app.callback(endgame_outcome)

if __name__ == '__main__':
    app()


```

</CodeBlock>

The moment when Doctor Strange uses the Time Stone to view 14,000,605 possible outcomes of their forthcoming battle, finding that only one of these futures results in victory.

The function `calculate_futures` represents this calculation, returning the number of winning futures (which is 1). The callback function `endgame_outcome` then interprets this result. If the outcome is the single winning future, it acknowledges Iron Man's critical role and his ultimate sacrifice.

</details>

<details>

<summary><b> Optional & Required Arguments </b></summary>

You can declare optional and required arguments for your CLI tool, making it more user-friendly and flexible.

<CodeBlock lang="python">

```python
import typer

app = typer.Typer()

@app.command()
def locate_stones(reality: bool, space: bool = False, time: bool = False):
    """
    Locate Infinity Stones in the Marvel Universe.
    """
    stones = {
        "reality": "Found" if reality else "Missing",
        "space": "Found" if space else "Missing",
        "time": "Found" if time else "Missing",
    }
    for stone, status in stones.items():
        typer.echo(f"{stone.capitalize()} Stone: {status}")

if __name__ == '__main__':
    app()

```

</CodeBlock>

This CLI tool helps users locate Infinity Stones in the Marvel Universe. The reality stone is a required argument, indicating whether it's been found, while the space and time stones are optional.

</details>

<details>

<summary><b> Multiple Subcommands </b></summary>

With Typer, you can create multiple subcommands within a single application, allowing for granular control over command functionality.

<CodeBlock lang="python">


```python
import typer

app = typer.Typer()

ironman_app = typer.Typer()
captainamerica_app = typer.Typer()

@app.callback()
def main():
    """
    Welcome to the Marvel CLI, where you can simulate actions of different Avengers.
    """

@ironman_app.command("snap")
def perform_snap():
    """
    Simulate Iron Man's snap to save the universe.
    """
    print("With a heavy heart, Iron Man snaps his fingers, sacrificing himself to save the universe.")

@captainamerica_app.command("shield")
def throw_shield():
    """
    Simulate Captain America throwing his shield.
    """
    print("Captain America throws his shield, knocking down enemies in a single swoop.")

app.add_typer(ironman_app, name="ironman")
app.add_typer(captainamerica_app, name="captainamerica")

if __name__ == "__main__":
    app()

```

</CodeBlock>

Each Avenger, like Iron Man or Captain America, has their own set of special moves or commands.

With Iron Man, you can choose to "snap" to mimic the famous scene where he saves the universe but sacrifices himself. It's like getting to step into his shoes and make that heroic choice.

For Captain America, you get to "throw his shield," which feels like you're right there in the action, knocking out the bad guys with his iconic shield.

This setup lets us organize our commands in a neat way, making it super easy to add more heroes and actions later on. It's like having a box of action figures, and each one can do something cool; our program just helps you pick which one to play with and what move you want to see.

</details>

<details>

<summary><b> Environment Variables </b></summary>

Typer allows you to use environment variables, you can access them using the `os.environ` dictionary provided by Python's standard library:

<CodeBlock lang="python">

```python
import os
import typer

app = typer.Typer()

@app.command()
def activate_shield(level: int):
    """
    Activate Captain America's shield to a specific power level using an environment variable.
    """
    os.environ["SHIELD_LEVEL"] = str(level)
    typer.echo(f"Shield activated to level {level}.")

if __name__ == '__main__':
    app()

```

</CodeBlock>

In this example the CLI tool activates Captain America's shield to a specified power level, demonstrating the use of environment variables. By storing the shield level in an environment variable, the tool illustrates how to use os.environ for managing application state outside the Python process. This can be especially useful for passing information between different parts of an application or to other programs.

</details>

## Conclusion

Typer for Python CLI development is a game-changer, offering superhero-like capabilities effortlessly. With Typer, tasks from adding type annotations to managing environment variables become enjoyable. It's like upgrading your toolset, turning each function into a precision tool. Typer's accessibility welcomes beginners and experts alike, echoing the Avengers' unity in the face of challenges. It ensures success in CLI development, empowering developers to leave their mark on the software world. To checkout more and build your own CLI tool you can click here for more detail [Typer](https://typer.tiangolo.com/).
