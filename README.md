DevSkim Plugin for Sublime Text
===============================

The plugin implements a security linter within the Sublime Text editor, leveraging the rules from the [DevSkim-Rules](https://github.com/Microsoft/DevSkim-Rules) repo. It helps software engineers to write secure code by flagging potentially dangerous calls, and gives in-context advice for remediation.

Requirements
--------------

The plugin requires Sublime Text 3 (build >= 3114), and will function on Windows, Linux, and MacOS.

Installation
------------
If using [Package Control](https://packagecontrol.io/) for Sublime Text, simply install the `DevSkim` package.

Alternatively, you can clone the plugin and rules repos directly into your Sublime plugin folder.  For example, for Sublime Text 3 on a Mac this would look something like:

```
cd ~/"Library/Application Support/Sublime Text 3/Packages"
git clone --depth 1 https://github.com/Microsoft/DevSkim-Sublime-Plugin.git DevSkim
cd DevSkim
git clone --depth 1 https://github.com/Microsoft/DevSkim-Rules.git rules
```
And on Windows:
```
cd "%APPDATA%\Sublime Text 3\Packages"
git clone --depth 1 https://github.com/Microsoft/DevSkim-Sublime-Plugin.git DevSkim
cd DevSkim
git clone --depth 1 https://github.com/Microsoft/DevSkim-Rules.git rules
```

(`--depth 1` downloads only the current version to reduce the clone size.)

Note if you are using the portable version of Sublime Text, the location will be different.  (See http://docs.sublimetext.info/en/latest/basic_concepts.html#the-data-directory for more info).

**IMPORTANT** If you already have a package called `DevSkim` installed, either remove this first, or clone this repo to a different folder, else module name resolution can break the plugin.

Platform support
----------------
#### Operating System:

The plugin has identical behavior across Windows, Mac, and Linux.

#### Sublime Text Version:

The plugin requires [Sublime Text 3](http://www.sublimetext.com/3) build >= 3114.

Features
--------
The below features are available via the keyboard shortcuts shown, or via the Command Palette (^ means the `ctrl` or `cmd` keys):

| Feature               | Shortcut        |
|-----------------------|-----------------|
| Run DevSkim           | `^D`            |

The

The "format on key" feature is on by default, which formats the current line after typing `;`, `}` or `enter`.
To disable it, go to `Preferences` -> `Package Settings` -> `DevSkim` -> `Plugin Settings - User`, and add
`"typescript_auto_format": false` to the json file.

Rules System
------------

The plugin supports both built-in and custom rules:

#### Built-In Rules

Built-in rules come from the [DevSkim-Rules](https://github.com/Microsoft/DevSkim-Rules.git) repo, and should be stored
in the `rules` directory within the DevSkim directory.

Rules are organized by subdirectory and file, but are flattened internally when loaded.

Each rule contains a set of patterns (strings and regular expressions) to match, a list of file types to
apply the rule to, and, optionally, a list of possible code fixes. An example rule is shown below:

```
[
    {
        "id": "DS126858",
        "name": "Weak/Broken Hash Algorithm",
        "active": true,
        "tags": [
            "Cryptography.BannedHashAlgorithm"
        ],
        "severity": "critical",
        "description": "A weak or broken hash algorithm was detected.",
        "replacement": "Consider switching to use SHA-256 or SHA-512 instead.",
        "rule_info": "https://github.com/microsoft/devskim/guidance/DS126858.md",
        "applies_to": [
            "$PHP"
        ],
        "patterns": [
            {
                "pattern": "md5(",
                "type": "string"
            },
        ],
        "fix_it": [
            {
                "type": "regex_substitute",
                "name": "Change to SHA-256",
                "search": "\\bmd5\\(([^\\)]+\\)",
                "replace": "hash('sha256', \\1)"
            }
        ]
    }
]
```

Screenshots
------

TODO

Reporting Issues
-------
Please see [CONTRIBUTING](https://github.com/Microsoft/DevSkim-Sublime-Plugin/blob/master/CONTRIBUTING.md) for information on reporting issues and contributing code.

Tips and Known Issues
----
See tips and known issues in the [wiki page](https://github.com/Microsoft/DevSkim-Sublime-Plugin/wiki/Tips-and-Known-Issues).
