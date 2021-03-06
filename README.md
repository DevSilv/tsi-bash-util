# tsi-bash-util (TreeStructInfo Bash Utility)

tsi-bash-util is a utility that allows the user to programmatically use the [TreeStructInfo file format](https://tsinfo.4programmers.net/pl/format/2.0.htm).

**Read before use:** This application is intended to be used **only** according to the purpose described above. You may use it **only** when you know what the code will really do in your environment. For details, see the section "[Disclaimers](#disclaimers)" below.

## Table of contents

1. [Copyright note](#Copyright-note)
2. [Disclaimers](#Disclaimers)
3. [Usage](#Usage)
4. [Exit statuses](#exceptions-thrown-by-the-utility)
5. [Environments, tools and technologies used](#environments-tools-and-technologies-used)
6. [Credits](#Credits)

## Copyright note

This project has currently **no license**.

For details what does "no license" mean, see [this GitHub guide on "no license"](https://choosealicense.com/no-permission/). A quote from that site:

> When you make a creative work (which includes code), the work is under exclusive copyright by default. Unless you include a license that specifies otherwise, nobody else can copy, distribute, or modify your work without being at risk of take-downs, shake-downs, or litigation. Once the work has other contributors (each a copyright holder), “nobody” starts including you.

## Disclaimers

While probably nothing dangerous would happen, you run code/scripts within this project only **under your own responsibility**.

A general advice: if you do not understand exactly what impact has **every character** in a piece of code, think twice before running this piece of code. Even the presence or not of particular quotes (e.g., `"text"` vs `'text'` vs `text`) may drastically change the result of running the code. And it is easy to mistake double quotes for single quotes.

The code is to be considered correct only according to the **tests** that the project contains. The code may contain bugs that are not detected by the tests.

## Usage

[TODO]

### Environment requirements

The environment of one has to fulfil at least the following requirements so that one be able to run the utility:

1. The shell is [Bash](https://www.gnu.org/software/bash/), version >= 5.0, and the path to its executable is `/bin/bash`.
2. There is available the `/dev/null` device file.

### Examples

[TODO]

## Exit statuses

- `0` — No error exit status
- `1` — General error exit status

## Environments, tools and technologies used

[TODO]

## Credits

- furious programming ([GitHub profile](https://github.com/furious-programming)), the author of the [TreeStructInfo file format](https://tsinfo.4programmers.net/pl/format/2.0.htm)