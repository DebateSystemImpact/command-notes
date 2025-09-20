# Style guide

This page lists specific formatting instructions for `tldr` pages.

## Contents

1. [General layout](#general-layout)
2. [Pages](#pages)
3. [General writing](#general-writing)
4. [Heading](#heading)
5. [Example descriptions](#example-descriptions)
6. [Example commands](#example-commands)
7. [Language and translation rules](#language-and-translation-rules)

## General layout

The basic format of each page should match the following template and have at most 8 command examples:

```md
# command name

> Short, snappy command description.
> Preferably one line; two are acceptable if necessary.
> More information: <https://example.com/command_name/help/page>.

- Code description:

`command_name options`

- Code description:

`command_name options`

...
```

Example:

```md
# krita

> A sketching and painting program designed for digital artists.
> See also: `gimp`.
> More information: <https://docs.krita.org/en/reference_manual/linux_command_line.html>.

- Start Krita:

`krita`

- Open specific files:

`krita {{path/to/image1 path/to/image2 ...}}`

- Start without a splash screen:

`krita --nosplash`

- Start with a specific workspace:

`krita --workspace {{Animation}}`

- Start in fullscreen mode:

`krita --fullscreen`
```

> [!NOTE]\
> The page's filename and title must match the command name exactly. The page title can be present in any case, whereas the page's Markdown filenames must be lowercase.

There is a linter that enforces the format above.
It is run automatically on every pull request,
but you may install it to test your contributions locally before submitting them:

```sh
npm install --global tldr-lint
tldr-lint path/to/tldr_page.md
```

For other ways to use `tldr-lint`, such as linting an entire directory, check out the
[`tldr page on tldr-lint`](https://github.com/tldr-pages/tldr/blob/main/pages/common/tldr-lint.md). Alternatively, you can also use its alias `tldrl`.

Depending on your client, you may be able to preview a page locally using the `--render` flag:

```sh
tldr --render path/to/tldr_page.md
```

### PowerShell-Specific Rules

When documenting PowerShell commands, please take note of the following naming conventions.

- The name of the file name must be written in lowercase, such as `invoke-webrequest.md` instead of `Invoke-WebRequest.md`.
- The page title/heading must be written as-is (matching the spelling intended by Microsoft or the PowerShell module author), such as `Invoke-WebRequest` instead of `invoke-webrequest`.
- The command name and options in the examples should also be written as-is, such as `Command-Name {{input}} -CommandParameter {{value}}` instead of `command-name {{input}} -commandparameter {{value}}`.

Due to [various compatibility differences](https://learn.microsoft.com/powershell/scripting/whats-new/differences-from-windows-powershell) and removed Windows-specific commands in PowerShell 6.x, ensure that
the command works on between **PowerShell 5.1** (aka. the "Legacy Windows PowerShell" as installed in Windows 10
and 11), and the **latest version of the Cross-Platform PowerShell** (formerly known as PowerShell Core).

Thus, if the command or its options are unavailable or contain different behaviors between each version, please kindly note them in the descriptions. For example:

```md
# Clear-RecycleBin

> Clear items from the Recycle Bin.
> Note: This command can only be used through PowerShell versions 5.1 and below, or 7.1 and above.
> More information: <https://learn.microsoft.com/powershell/module/microsoft.powershell.management/clear-recyclebin>.
```

## Pages

### Platform differences

If you are afraid the commands may differ between platforms or operating systems (e.g. Windows vs macOS),
most [tldr pages clients](https://github.com/tldr-pages/tldr/wiki/Clients) will choose the most suitable version of the command to be displayed to the end user.

In this case, the information of the Windows version of `cd` (stored in `pages/windows/cd.md`) will be displayed by default to Windows users, and a generic/common version (stored in `pages/common/cd.md`)
will be displayed for Linux, macOS, and other platform users.

Try to match the page filename with the invoked command. Do not use the project name if possible. The goal is to be as transparent as possible to the user when they are curious of a command.

### Aliases

If a command can be called with alternative names (like `vim` can be called by `vi`), alias pages can be created to point the user to the original command name.

```md
# command_name

> This command is an alias of `original-command-name`.

- View documentation for the original command:

`tldr original_command_name`
```

Example:

```md
# vi

> This command is an alias of `vim`.

- View documentation for the original command:

`tldr vim`

```

- Pre-translated alias page templates can be found [here](https://github.com/tldr-pages/tldr/blob/main/contributing-guides/translation-templates/alias-pages.md).

#### PowerShell-Specific Aliases

Some PowerShell commands may introduce aliases which fall into one of these three categories:

1. **Replaces an existing Windows Command Prompt (`cmd`) command**, such as `cd` aliasing to `Set-Location` with different command options. In this case, add the following alias note into the second line of the original
Command Prompt command's tldr description, for example:

```md
# cd

> Display the current working directory or move to a different directory.
> In PowerShell, this command is an alias of `Set-Location`. This documentation is based on the Command Prompt (`cmd`) version of `cd`.
> More information: <https://learn.microsoft.com/windows-server/administration/windows-commands/cd>.

- View documentation of the equivalent PowerShell command:

`tldr set-location`
```

> [!NOTE]\
> The "View documentation of the equivalent PowerShell command" example is optional and must be excluded if the page already has the maximum number (8) of examples.

2. **Provides a new alias but only executable in PowerShell**, such as `ni` for `New-Item`. In this case, use the [standard alias template](https://github.com/tldr-pages/tldr/blob/main/contributing-guides/translation-templates/alias-pages.md),
but add the word "In Powershell," (or equivalent) to indicate that the command is exclusive to PowerShell. For example,

```md
# ni

> In PowerShell, this command is an alias of `New-Item`.
> More information: <https://learn.microsoft.com/powershell/module/microsoft.powershell.management/new-item>.

- View documentation for the original command:

`tldr new-item`
```

**3. Provides a new alias that conflicts with other programs**, most notoriously the inclusion of `curl` and `wget` as aliases of `Invoke-WebRequest` (with a non-compatible set of command options).
Note that PowerShell system aliases that fall into this category are commonly exclusive to Windows.

In this case, provide a note and method to determine whether the command currently refers to a PowerShell command (by alias) or others. For example,

```md
# curl

> In PowerShell, this command may be an alias of `Invoke-WebRequest` when the original `curl` program (<https://curl.se>) is not properly installed.
> More information: <https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest>.

- Check whether `curl` is properly installed by printing its version number. If this command evaluates into an error, PowerShell may have substituted this command with `Invoke-WebRequest`:

`curl --version`

- View documentation for the original `curl` command:

`tldr curl -p common`

- View documentation for PowerShell's `Invoke-WebRequest` command:

`tldr invoke-webrequest`
```

### Disambiguations

If there is a name collision between page names on the same platform, disambiguation pages can be used to direct users to different pages. Name the colliding pages with a dot and an appropriate suffix. Numbers starting from 1 can be used if no other suffix is appropriate. If the colliding page is an acronym, direct the user to a page with the name expanded.

In the following case `just.md` is the filename of the disambiguation page while `just.1.md` and `just.js.md` refer to the actual pages:

```md
# just

> `just` can refer to multiple commands with the same name.

- View documentation for the command runner:

`tldr just.1`

- View documentation for the V8 JavaScript runtime:

`tldr just.js`
```

### Grouping commands

Sometimes commands are meant to be used in combination with other commands. In these cases it makes sense to move them on the same page.

For example `adb disconnect` has a single way using it, but `adb` is expansive enough that it doesn't fit in the main page. Normally, `adb disconnect` is used in combination with `adb pair` and `adb connect`, thus it makes sense to group these together into a single page. For example:

```md
# adb disconnect

> This command has been moved to `adb connect`.

- View documentation for `adb disconnect`:

`tldr adb connect`
```

## General writing

### Emphasis

Do not use *italics*, **boldface** or any other text styling on the pages. These are reserved for client emphasis of placeholders.

### Imperative Mood

- **All descriptions must be phrased in the imperative mood.**
- This also applies to all translations by default unless otherwise specified in the language-specific section below.

When writing descriptions for command examples, **check for any grammatical errors**. `Go to the specified directory` is preferred instead of:

- `Going to the specified directory` (should not be in present participle form)
- `This command will go to the specified directory` (it is clear that this example works for *this* comment)
- `Let's go to the specified directory!`
- `Directory change` (use the active form instead of passive, if possible)

For instance, instead of `Listing all files:`, use the following:

```md
- List all files:

 `ls`
```

### Serial Comma

- When declaring a list of 3 or more items,
use a [serial comma](https://en.wikipedia.org/wiki/Serial_comma),
also known as the Oxford comma,
since omitting it can create ambiguity.

> Delete the Git branches, tags and remotes.

The example above does not use a serial comma, so this could mean one of two things:

- Delete the Git branches named `tags` and `remotes`.
- Delete all of the following: Git branches, Git tags, and Git remotes.

This can be resolved by inserting a comma before the "and" or "or" in the final element in the list.

> Delete the Git branches, tags, and remotes.

> [!NOTE]\
> Brand and project names can be capitalized in the description whenever applicable (e.g. use `A tool for interacting with a Git repository.` instead of ``A tool for interacting with a `git` repository.``).

### Special cases

If a command performs irreversible changes to a file system or devices,
  write every example in a way that cannot be copy pasted thoughtlessly.
  For example, instead of `ddrescue --force --no-scrape /dev/sda /dev/sdb`
  write `ddrescue --force --no-scrape {{/dev/sdX}} {{/dev/sdY}}`
  and use the `{{/dev/sdXY}}` placeholder for *block devices* instead of `/dev/sda1`.

In general, placeholders should make it as intuitive as possible
to figure out how to use the command and fill it in with values.

Acronym expansions (i.e. protocols, tools, etc) must not be translated unless there is a recognized native equivalent for them.

Technical wording on description lines should use the `backtick` syntax.
Use backticks on the following:

- Paths, e.g. `package.json`, `/etc/package.json`.
- Extensions, e.g. `.dll`.
- Commands, e.g. `ls`.
- Standard streams: `stdout`, `stdin`, `stderr`. **Do not** use the full names (e.g. standard output).
- Compression algorithms, e.g. `zip`, `7z`, `xz`.

When describing keycaps or a keyboard shortcut for a utility, use the same [keypress syntax](#keypress-syntax) as in example commands. Make sure to enclose it in backticks so that it is not invisible in markdown renderers (i.e. ``Print the last lines of a given file and keep reading it until `<Ctrl c>`:``).

If a program requires root privileges to run and doesn't provide its own prompt for a password, prepend the command with `sudo` (e.g. `sudo apt update`).

### Standardized Terms

Some terms are used repeatedly throughout pages, and as such, should be standardized. These include:

| Term | Standard | Explanation |
|---|---|---|
| Regular expression | `` `regex` `` | `regex` defines a match pattern given a string of characters (https://en.wikipedia.org/wiki/Regular_expression). |

## Heading

### Program description

- Avoid using the page title in the description (e.g. use `A sketching and painting program designed for digital artists` instead of `Krita is a sketching and painting program designed for digital artists`)
- If the program name differs from its executable name, it can be named at the start of the heading (e.g. `rg` and Ripgrep).
- Avoid mentioning that the program is used on the command-line (e.g. use `Ripgrep, a recursive line-oriented search tool` instead of `Ripgrep, a recursive line-oriented CLI search tool`).

For example, when writing documentation for `cd`, a tool to check out and work on a specific directory in the Terminal or Command Prompt, **do not** write a lengthy description such as:

```md
> `cd` is a system tool, available in Windows, macOS, and Linux, to check out a specific directory to get things done in the Command Prompt, Terminal, and PowerShell.
```

It should instead be simplified to make it easier for everyone to read:

```md
> Change the current working directory.
```

### More information links

- On the `More information` link line, provide a direct link to documentation that instructs on how to use the command. We prefer linking to the author's provided documentation but when not available or very lacking in information, use <https://manned.org> as the default fallback for all platforms
(except `osx` and BSD platforms other than FreeBSD).
If there is no documentation page to be found, you can link to the author's website or a third party tutorial.
- Keep the more information link short. Cut out redundant text if possible. For example use https://manned.org/partclone instead of https://manned.org/man/partclone.8 unless there are two different manpages for a command across distributions/platforms i.e. `command.1` and `command.8`.

- For `osx`: Apple distributes the built-in man pages [in Xcode](https://developer.apple.com/documentation/os/reading_unix_manual_pages).
For commands documented there, we recommend using <https://keith.github.io/xcode-man-pages/>, an HTML export of all Apple's man pages bundled with Xcode.

> [!IMPORTANT]
> All links must be enclosed inside angular brackets (`<` and `>`).

- It is suggested to use a more information link with English content in both translations and English pages. That's because the links can eventually change, but the translations are often out of sync with the English pages.

#### Versioned links

When a utility or distribution has versioned links for the packages, link to the most recent version of documentation (i.e. `latest`) or none if the website automatically redirects to the latest version of the documentation.

For example, use:

- <https://manpages.debian.org/latest/apt/apt.8.html> instead of <https://manpages.debian.org/bookworm/apt/apt.8.en.html>.
- <https://docs.aws.amazon.com/cdk/latest/guide/cli.html> instead of <https://docs.aws.amazon.com/cdk/v2/guide/cli.html>.

#### Microsoft Learn links

When linking pages to the Microsoft Learn links, remove the locale from the address as the website will automatically redirect to the reader's preferred locale setting.
For example, Use <https://learn.microsoft.com/windows-server/administration/windows-commands/cd> instead of
<https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/cd>.

Additionally, if the link is related to PowerShell command documentation, remove the **documentation version indicator** (in which the version of PowerShell/module that the documentation is derived from), aka.
the part of the address that starts with `?view=`.

- Use <https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/select-string> instead of <https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7.4>.
- Use <https://learn.microsoft.com/powershell/module/powershellget/install-module> instead of <https://learn.microsoft.com/en-us/powershell/module/powershellget/install-module?view=powershellget-1.x>.

### See also section

- To reference a related command or subcommand, use:

```md
> See also: `command`.
```

- To reference related commands or subcommands, use:

```md
> See also: `command1`, `command2`, `command3`.
```

- Optionally, you can add a short description beside the referenced pages:

```md
> See also: `date` for Unix information, `uname` for system information and `umount` for unmounting partitions.
```

## Example descriptions

### Short option mnemonics

Short option mnemonics are optional hints that can be added to help users understand the meaning of these short options. The assigned mnemonics should match with the ones in the command's official documentation (e.g. from `man` or `Get-Help`). For example:

```md
- [d]isplay the ins[t]allation [i]D for the current device. Useful for offline license activation:

`slmgr.vbs /dti`

- Display the current license's e[xp]i[r]ation date and time:

`slmgr.vbs /xpr`
```

Note that, in the first example, the `[d]`, `[t]`, and `[i]` characters are enclosed with square brackets to indicate that the `/dti` option of the command is a combination of "display", "installation", and "ID", respectively.
Group consecutive mnemonic characters under the same square brackets, for example: `e[xp]i[r]ation` instead of `e[x][p]i[r]ation`.

**Mnemonic characters must be written in a case-sensitive manner**, even when it is placed as the first character of the sentence (i.e. use `[d]isplay` instead of `[D]isplay`).
This is to avoid conflicts with GNU-style command options which may interpret uppercase options differently than the lowercase ones, such as `-v` for displaying the command's `[v]ersion` number and `-V` to run the command in `[V]erbose` mode.

Option mnemonics may also be used in translations as long as the highlighted word contains similar meanings to the language (commonly English) which the command is written for.
For example, `[d]ownload` in English may be translated into `[d]escargar` in Spanish, `[i]nstall` in English may be translated to `[i]nstallieren` in German, and `[a]pp` in English may be translated into `[a]plikasi` in Indonesian and Malay.

- Optionally, mnemonics and their enclosed terms can be separated with brackets from the rest of the description (i.e. `([a]ll)`) in translations and specific pages to provide additional context or mention a word not present in the description.

> [!NOTE]\
> In cases where the character isn't present in the translated word, you can highlight the option next to the equivalent word or you can add the English work beside the translation inside a bracket.
> For example, `E[x]tract` in English may be translated into `ekstrak [x]` or `ekstrak (E[x]tract)` in Indonesian.

## Example commands

### Argument order

Try to keep the following order:

- Program name
- Input redirection from a file
- All subcommands
- Options/Flags
- Positional arguments/Packages/Data/...
- Output redirection to a file

For example: `systemctl < input_file.txt status --user pipewire > output_file.txt`

This is only a suggestion and should be disregarded when program functionality or readability dictates otherwise.

If the command does multiple things, try to keep the chronological order in which things happen.

### Option syntax

- For user-friendliness, prefer **GNU-style long options** (like `--help` rather than `-h`). Make sure that the options are cross-platform compatible (intended to work the same across multiple platforms) for pages in the `common` directory.
- For letting the client decide whether to show long or short options in commands, use an option placeholder i.e. `{{[-o|--output]}}`.
- If a command only supports short options or the short option greatly differs from the long option, attempt to document what the letter is short for with a [mnemonic](#short-option-mnemonics).
- Prefer grouping flag options together when the program supports it (i.e. `{{[-it|--interactive --tty]}}` instead of `{{[-i|--interactive]}} {{[-t|--tty]}}`).
- Prefer not grouping options that take in arguments (i.e. `{{[-it|--interactive --tty]}} {{[-w|--workdir]}} {{path/to/directory}}` instead of `{{[-itw|--interactive --tty --workdir]}} {{path/to/directory}}`)
- Prefer using a space instead of the equals sign (`=`) to separate options from their arguments (i.e. use `--opt arg` instead of `--opt=arg`), unless the program does not support it.
- Likewise prefer separating shortform options from their arguments with a space (i.e. use `-o arg` instead of `-oarg`), unless the program does not support it.
- If a command only supports `-oarg` and `--opt=arg` the option placeholder should be written like this `{{[-o|--opt=]}}arg`. Keep in mind how the command would look if a client were to display only short or only long options.

### Placeholder syntax

User-provided values should use the `{{placeholder}}` syntax
in order to allow `tldr` clients to highlight them.

> [!TIP]
> It is suggested to enclose placeholders accepting strings as input within quotes. i.e. Use `"{{placeholder}}"` instead of `{{"placeholder"}}`.

Keep the following guidelines in mind when choosing placeholders:

#### Naming

- Use short but descriptive placeholders,
  such as `{{path/to/source_file}}` or `{{path/to/wallet.txt}}`.
- Use [`snake_case`](https://wikipedia.org/wiki/snake_case) for multi-word placeholders.

#### Paths

- Use `{{filename}}` when just the file name is expected.
- For any reference to paths of files or directories,
  use the format `{{path/to/placeholder}}`,
  except when the location is implicit.
- When the path cannot be relative
  and has to start at the root of the filesystem,
  prefix it with a slash outside the placeholder,
  such as `get /{{path/to/remote_file}}`.
- In case of a possible reference both to a file or a directory,
  use `{{path/to/file_or_directory}}`.

> [!NOTE]\
> If the command is specific to Windows, use backslashes (`\`) instead, such as `{{path\to\file_or_directory}}`. Drive letters such as `C:` are optional unless the command input requires an absolute path
> or specific drive letter range, such as `cd /d {{C}}:{{path\to\directory}}`.

#### Extensions

- If a particular extension is expected for the file, append it.
  For example, `unrar x {{path/to/compressed.rar}}`.
- In case a generic extension is needed, use `{{.ext}}`, but **only** if an extension is required.
  For instance, in `find.md`'s example "Find files by extension" (`find {{path/to/root}} -name '{{*.ext}}'`)
  using `{{*.ext}}` explains the command without being unnecessarily specific;
  while in `wc -l {{path/to/file}}` using `{{path/to/file}}` (without extension) is sufficient.

#### Grouping placeholders

- If a command can optionally take 1 or more arguments of the same kind, use an ellipsis: `{{placeholder1 placeholder2 ...}}`.
  For instance, if multiple paths are expected, use `{{path/to/directory1 path/to/directory2 ...}}`.
- If only one of the multiple options is possible, write it as: `{{placeholder1|placeholder2|placeholder3}}`. If there are more than 3 possible values, you can use `|...` after the last item.
- Use two dots to mark a range of possible values, for example `{{1..5}}` or `{{a..z}}`.

#### Optional placeholders

When documenting optional placeholders like paths or file extensions, it is suggested to specify them in the page or example descriptions instead of the placeholder itself. For example:

- Use `{{path/to/source.ext}}` instead of `{{path/to/source.tar[.gz|.bz2|.xz]}}`.

> [!IMPORTANT]
> Do not put placeholders inside placeholders.

### Keypress syntax

To mark keypresses for TUI or GUI programs, use angle brackets `<` and `>`.

- Single character example: `<a>`.
- Special keys are to be written with [`PascalCase`](https://www.theserverside.com/definition/Pascal-case): `<Ctrl>`, `<Super>`, `<Alt>`, `<Shift>`, `<Cmd>`, `<Option>`, `<Windows>`, `<Enter>`, `<Home>`, `<Space>`, `<Esc>`, `<ArrowUp>`, `<ArrowLeft>`, `<ArrowKeys>`, `<PageUp>`, `<F5>`, `<F12>`, `<LeftClick>`, `<MiddleClick>`, ...
- Special keys can be translated if they have culturally relevant translations.
- When a program takes in uppercase character literals mark them as `<A>` instead of marking it with shift. Otherwise always mark characters in lowercase.
- Mark simultaneous keypresses inside the same angle brackets separated by a single space:  `<Ctrl c>`, `<Alt F4>`, `<Ctrl Shift k>`, `<Super Shift PrtSc>`.
- When writing simultaneous keypresses, keep the following order: `<Ctrl Super Windows Alt AltGr Shift everything_else>`.
- Consecutive keypresses need to be contained in their own angle brackets with no space in between: `<Esc><u>`, `<Ctrl k><Ctrl s>`, `<Enter><~><.>`, `<d><o>`.
- Keys that are typed into a prompt do not need to be marked as keypresses: `<:>help<Enter>`. Note that the context switching keypress is marked in angle brackets despite printing on the prompt.
