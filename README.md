# Oyster Modules

[![Build Status](https://travis-ci.org/ryuzi/oyster-modules.svg?branch=master)](https://travis-ci.org/ryuzi/oyster-modules)

> modules for oyster (https://github.com/ryuzi/oyster)

## Getting Started

### Prerequisites

* [Oyster](https://github.com/ryuzi/oyster) should be installed


### Basic Installation

Testing Module is installed by running one of the following commands in your terminal. You can install this via the command-line with either `curl` or `wget`.
Then reloading .oystrc file.

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ryuzi/oyster-modules/master/install)"
```

#### via wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/ryuzi/oyster-modules/master/install -O -)"
```

## Using Oyster Modules

```
import <module name>
```

### Writing a shell script

For example, create `/path/to/myproject/myscript` with these contents:

```bash
#!/usr/bin/env oyster
import template

how_weather() {
    local context params result

    context="{{ place }} is {{ weather }} today."
    params=(
        place="Tokyo"
        weather="sunny"
    )

    result=$(template.render "${context}" "${params[@]}")
    echo "${result}"
}
```

You can run by using command:

```
$ ./myscript how_weather
Tokyo is sunny today.
```

### Module List

* template - template module
    * template.loader
    * template.render


## LICENSE

Oyster is released under an MIT-style license; see `LICENSE` for details.
