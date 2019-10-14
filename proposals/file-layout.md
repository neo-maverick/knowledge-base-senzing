# Senzing file layout

## Types of Senzing files

Senzing has directories of files that fit into the following categories:

1. Immutable files:
    1. `g2` directory.
    1. `data` directory.
1. Configuration files:
    1. `etc` directory.
1. Mutable files:
    1. `var`directory.

The expectation is that `g2` and `data` directory contents are not modified by the user nor by processes.
If there are files that need to be customized or modified,
they are copied into either the `etc` directory or `var` directory and then modified.

## Identifying the locations

Given the 4 separate directories, processes look for configuration in the
following priority order:

1. Command-line parameters
1. Configuration file
1. Environment variables
1. Defaults

### Command-line parameters

The highest configuration priority is given to a command line parameter / option.
It over-rides a configuration file value, Environment variables, and defaults.

1.  Example:

    ```console
    senzing-program \
      --data-dir /path/to/data \
      --g2-dir /path/to/g2 \
      --etc-dir /path/to/etc \
      --var-dir /path/to/var
    ```

### Configuration file

A configuration file can specify one or more configurations.

1. Examples:

    ```console
    senzing-program \
      --config-file /path/to/config-file.toml
    ```

    ```console
    senzing-program \
      --config-file /path/to/config-file.json
    ```

    `/path/to/config-file.toml` contents.
    Example:

    ```console
    [senzing]
    dataDir = /path/to/data
    etcDir  = /path/to/etc
    g2Dir   = /path/to/g2
    varDir  = /path/to/var
    ```

    `/path/to/config-file.json` contents.
    Example:

    ```json
    {
      "senzing": {
        "dataDir": "/path/to/data",
        "etcDir": "/path/to/etc",
        "g2Dir": "/path/to/g2",
        "varDir": "/path/to/var"
      }
    }
    ```

### Environment variables

1. Example:

    ```console
    export SENZING_DATA_DIR=/path/to/data
    export SENZING_ETC_DIR=/path/to/etc
    export SENZING_G2_DIR=/path/to/g2
    export SENZING_VAR_DIR=/path/to/var
    ```

### Default locations

1. Example:

    1. "data" directory
        1. `${CURRENT_WORKING_DIRECTORY}/data`
        1. `/opt/senzing/data`
    1. "etc" directory
        1. `${CURRENT_WORKING_DIRECTORY}/etc`
        1. `/etc/opt/senzing`
    1. "g2" directory
        1. `${CURRENT_WORKING_DIRECTORY}/g2`
        1. `/opt/senzing/g2`
    1. "var" directory
        1. `${CURRENT_WORKING_DIRECTORY}/var`
        1. `/var/opt/senzing`

1. **Notes**
    1. The "data" directory refers to the directory containing actual data (e.g. `cnv.ibm`, `gnv.ibm`).
       It does not refer to the the directory that holds version subdirectories.
    1. To disambiguate "floating" paths from versioned paths, the use of the `/latest` directory as a soft link to the version of Senzing software might be used as the system default.

## Projects

Given that a Project is a specific set of (g2, data, etc, var),
the "Default locations" could be augmented to:

   Example:

1. "data" directory
    1. `${SENZING_PROJECT_DIR}/data`
    1. `${CURRENT_WORKING_DIRECTORY}/data`
    1. `/opt/senzing/data`
1. "etc" directory
    1. `${SENZING_PROJECT_DIR}/etc`
    1. `${CURRENT_WORKING_DIRECTORY}/etc`
    1. `/etc/opt/senzing`
1. "g2" directory
    1. `${SENZING_PROJECT_DIR}/g2`
    1. `${CURRENT_WORKING_DIRECTORY}/g2`
    1. `/opt/senzing/g2`
1. "var" directory
    1. `${SENZING_PROJECT_DIR}/var`
    1. `${CURRENT_WORKING_DIRECTORY}/var`
    1. `/var/opt/senzing`

Then, a Senzing project directory could be specified as a configuration parameter.

1. Command-line parameters.
   Example:

    ```console
    senzing-program \
      --project-dir /path/to/project
    ```

1. Configuration file.
   Examples:

    ```console
    senzing-program \
      --config-file /path/to/config-file.toml


    `/path/to/config-file.toml` contents.
    Example:

    ```console
    [senzing]
    projectDir = /path/to/project
    ```

1. Environment variables.
   Example:

    ```console
    export SENZING_PROJECT_DIR=/path/to/project
    ```

### Special cases

1. A command like:

    ```console
    senzing-program \
      --project-dir /path/to/project \
      --etc-dir /path/to/etc
    ```

Means use the project directory for (g2, data, var) directories and `/path/to/etc` for the "etc" directory.
This allows for flexible testing against multiple configurations.

## References

1. Linux Filesystem Hierarchy Standard
    1. [tldp.org](http://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/)
    1. [Wikipedia](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)
1. Layered configuration
    1. [Python LayeredConfig](https://layeredconfig.readthedocs.io/en/latest/usage.html#precedence)
1. TOML
    1. [Wikipedia](https://en.wikipedia.org/wiki/TOML)
1. Configuration packages
    1. [Viper](https://github.com/spf13/viper)
1. Cascading Configuration pattern
    1. [Cascading Configuration Design Pattern](https://fredtrotter.com/2017/12/05/cascading-configuration-design-pattern/)
    1. [Cascading Configuration Pattern](http://www.octodecillion.com/cascadeconfigpattern/)