# SGPT

SGPT is a powerful command-line interface (CLI) tool designed for seamless interaction with OpenAI models directly from
your terminal. Effortlessly run queries, generate shell commands or code, create images from text, and more, using
simple commands. Streamline your workflow and enhance productivity with this powerful and user-friendly CLI tool.

Developed with the help of [sgpt](https://github.com/tbckr/sgpt).

## Installation Methods

There are two primary ways to install SGPT:

1. Install using Go:

With Go installed on your system, run the following command:

```shell
go install github.com/tbckr/sgpt/cmd/sgpt@v1.6.1
```

2. Download the latest release:

Visit the GitHub [release page](https://github.com/tbckr/sgpt/releases) and download the latest release for your
platform.

## Build from Source Code

### Build with Go

To build SGPT from the source code using Go, follow these steps:

1. Ensure you have [Go](https://go.dev/dl/) installed on your system.
2. Clone the SGPT repository:

```shell
git clone https://github.com/tbckr/sgpt.git
```

3. Navigate to the cloned repository:

```shell
cd sgpt
```

4. Build the SGPT executable using Go:

```shell
go build -o sgpt cmd/sgpt/main.go
```

This will create an executable named sgpt in the current directory.

### Build a Docker Image

To build a Docker image for SGPT, use the `docker-build.sh` script located in the `bin` folder:

1. Make sure you are in the root of the cloned SGPT repository.
2. Make the docker-build.sh script executable:

```shell
chmod +x bin/docker-build.sh
```

3. Run the `docker-build.sh` script to build the Docker image:

```shell
bin/docker-build.sh
```

The script will build the Docker image for the linux/amd64 platform with the tag `sgpt:latest`. Change the platform,
build args or tag according to your needs.

## Usage Guide

### Obtaining an OpenAI API Key

To use the OpenAI API, you must first obtain an API key.

1. Visit [https://platform.openai.com/overview](https://platform.openai.com/overview) and sign up for an account.
2. Navigate to [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys) and generate
   a new API key.
3. Update your `.bashrc` or `.zshrc` file to include the following export statement:

```shell
export OPENAI_API_KEY="sk-..."
```

After completing these steps, you'll have an OpenAI API key that can be used to interact with the OpenAI models through
the SGPT tool.

### Querying OpenAI Models

SGPT allows you to ask simple questions and receive informative answers. For example:

```shell
$ sgpt txt "mass of sun"
The mass of the sun is approximately 1.989 x 10^30 kilograms.
```

You can also pass prompts to SGPT using pipes:

```shell
$ echo -n "mass of sun" | sgpt txt
The mass of the sun is approximately 1.989 x 10^30 kilograms.
```

### Chat Capabilities

SGPT provides chat functionality that enables interactive conversations with OpenAI models. You can use the `--chat`
flag with the `txt`, `sh`, and `code` subcommands to initiate and reference chat sessions.

The chat capabilities allow you to interact with OpenAI models in a more dynamic and engaging way, making it
easier to obtain relevant responses, code, or shell commands through continuous conversations.

The example below demonstrates how to fine-tune the model's responses for more targeted outcomes.

1. The first command initiates a chat session named `ls-files` and asks the model to "list all files directory":

```shell
$ sgpt sh --chat ls-files "list all files directory"
ls
```

The model responds with the shell command ls, which is used to list all files in a directory.

2. The second command continues the conversation within the `ls-files` chat session and requests to "sort by name":

```shell
$ sgpt sh --chat ls-files "sort by name"
ls | sort
```

The model provides the appropriate shell command `ls | sort`, which lists all files in a directory and sorts them by
name.

To manage active chat sessions, use the `sgpt chat` command. Here are the available options for chat session management:

- `sgpt chat ls`: List all active chat sessions.
- `sgpt chat show <chat session>`: Display the content of a specific chat session.
- `sgpt chat rm <chat session>`: Remove a chat session.
- `sgpt chat rm --all`: Delete all chat sessions.

### Running Queries with Docker

For users who prefer to use Docker, SGPT provides a Docker image:

1. Pull the latest Docker image:

```shell
docker pull ghcr.io/tbckr/sgpt:latest
```

2. Run queries using the Docker image:

```shell
$ docker run --rm -e OPENAI_API_KEY=${OPENAI_API_KEY} ghcr.io/tbckr/sgpt:latest txt "mass of sun"
The mass of the sun is approximately 1.989 x 10^30 kilograms.
```

### Generating and Executing Shell Commands

SGPT can generate shell commands based on your input:

```shell
$ sgpt sh "make all files in current directory read only"
chmod -R 444 *
```

You can also generate a shell command and execute it directly:

```shell
$ sgpt sh --execute "make all files in current directory read only"
chmod -R 444 *
Do you want to execute this command? (Y/n) y
```

### Enhancing Your Workflow with Bash Aliases and Functions

SGPT can be further integrated into your workflow by creating bash aliases and functions. This enables you to automate
common tasks and improve efficiency when working with OpenAI models and shell commands.

Indeed, you can configure SGPT to generate your git commit message using the following bash function:

```shell
gsum() {
  commit_message="$(sgpt txt "Generate git commit message, my changes: $(git diff)")"
  printf "%s\n" "$commit_message"
  read -rp "Do you want to commit your changes with this commit message? [y/N] " response
  if [[ $response =~ ^[Yy]$ ]]; then
    git add . && git commit -m "$commit_message"
  else
    echo "Commit cancelled."
  fi
}
```

For instance, the commit message for this description and bash function would appear as follows:

```shell
$ gsum
feat: Add bash function to generate git commit messages

Added `gsum()` function to `.bash_aliases` that generates a commit message using sgpt to summarize git changes.
The user is prompted to confirm the commit message before executing `git add . && git commit -m "<commit_message>"`.
This function is meant to automate the commit process and increase productivity in daily work.

Additionally, updated the README.md file to include information about the new bash function and added a section to
showcase useful bash aliases and functions found in `.bash_aliases`.
Do you want to commit your changes with this commit message? [y/N] y
[main d6db80a] feat: Add bash function to generate git commit messages
 2 files changed, 48 insertions(+)
 create mode 100644 .bash_aliases
```

A compilation of beneficial bash aliases and functions is available in [.bash_aliases](.bash_aliases).

### Code Generation Capabilities

SGPT can efficiently generate code based on given instructions. For instance, to solve the classic FizzBuzz problem
using Python, simply provide the prompt as follows:

```shell
$ sgpt code "Solve classic fizz buzz problem using Python"
for i in range(1, 101):
    if i % 3 == 0 and i % 5 == 0:
        print("FizzBuzz")
    elif i % 3 == 0:
        print("Fizz")
    elif i % 5 == 0:
        print("Buzz")
    else:
        print(i)
```

SGPT will return the appropriate Python code to address the FizzBuzz problem

### Transforming Text to Images

SGPT can also generate images based on textual descriptions using the DALLE API. To create an image from text, input
your desired description as shown below:

```shell
$ sgpt image "v for vendetta"
<image url>
```

SGPT will return an image URL representing the "V for Vendetta" prompt.

If you prefer to download the generated image directly to your current working directory, use the `--download` flag:

```shell
$ sgpt image --download "v for vendetta"
1c561592-6d93-438f-9bee-d96c898a31a8.png
```

The image will be downloaded with a unique file name, making it easily accessible within your working directory.

## Acknowledgements

Inspired by [shell-gpt](https://github.com/TheR1D/shell_gpt).
