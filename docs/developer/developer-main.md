# Developer documentation

## Automated testing

### Environment requirements

The environment of one has to fulfil at least the following requirements so that one be able to run automated tests:

1. The shell is [Bash](https://www.gnu.org/software/bash/), version >= 5.0, and the path to its executable is `/bin/bash`.
2. There is available the `/dev/null` device file.
3. The following Linux shell utilities are available through the `PATH` environment variable:
    - find (GNU findutils), version >= `4.6.0`

## Workflow

1. Write some code.
2. Write unit tests for this code.
3. Run unit tests for the whole application.
4. If all unit tests pass, write integration tests for this code.
5. Run integration tests for the whole application.
6. If all integration tests pass, commit.

## Code conventions

### Comments

- There is to be no comments in the source code.

### Quoting

- Numbers **are** to be placed within double quotation marks except where it is not possible.
- Paths are **not** to be placed within any quotation marks except where required.

### Exiting and returning

- An exit code is to mean either an error or no error.

### Type system

- For a function, returning an empty string is to mean returning "false", and returning the string `yes` is to mean returning "true".

### Naming

- Names of functions need not start with a verb.
- Names of functions are to be as short as possible.
- Names of functions are to be prefixed with `tsi_`.

## Best practices used

### Error handling

- Throw early, catch late. (Throw at the point that the error firstly occurs, catch at the point that the error may be appropriately handled, as I understood it.)