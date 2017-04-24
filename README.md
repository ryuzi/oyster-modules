# Oyster Modules

[![Build Status](https://travis-ci.org/ryuzi/oyster-modules.svg?branch=master)](https://travis-ci.org/ryuzi/oyster-modules)

> modules for oyster (https://github.com/ryuzi/oyster)

## Getting Started

### Prerequisites

* [Oyster](https://github.com/ryuzi/oyster) should be installed


### Basic Installation

Oyster Module is installed by running one of the following commands in your terminal. You can install this via the command-line with either `curl` or `wget`.

#### via curl

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ryuzi/oyster-modules/master/install)"
```

#### via wget

```shell
sh -c "$(wget https://raw.githubusercontent.com/ryuzi/oyster-modules/master/install -O -)"
```

Then add the following settings to your `$HOME/.oysterc` file:
```
export OYST_MODULE_PATH=$HOME/.oyster-modules
```

And reloading file using command:
```shell
$ source $HOME/.oysterc
```


## Using Oyster Modules

You can import in your script by using command:

```
import <module-name>
```

### Oyster Module Reference

* testing - Unit Test module
    * `assert $LINENO <expression>`
* template - Template module
    * `template.loader <filename>`
    * `template.render <context> <key-value [key-value...]>`

## Unit Test

For example, create `mytest` with these contents:

```bash
#!/usr/bin/env oyster
import testing
import template

setup() {

    filename=$(mktemp "filename.XXXXX")
    echo "Your message: {{ message }}" > "${filename}"
    answer="Your message: fantastic!"

}

teardown() {

    rm "${filename}"

}

tests.template.render() {

    local context params result

    context=$(template.loader "${filename}")
    params=(
        message="sucktastic"
    )

    result=$(template.render "${context}")
    assert $LINENO "$?" = 1
    assert $LINENO "${result}" = ""

    result=$(template.render "${context}" "${params[@]}")
    assert $LINENO "$?" = 0
    assert $LINENO "${result}" = "${answer}"

    params=(
        message="fantastic!"
    )
    result=$(template.render "${context}" "${params[@]}")
    assert $LINENO "$?" = 0
    assert $LINENO  "${result}" = "${answer}"

}

tests.calc() {
    assert $LINENO $((1 + 5)) = 6
    assert $LINENO $((24 % 9)) = 4
}
```

You can run by using command:

```shell
$ chmod u+x mytest
$ ./mytest run

tests.template.render: ...F..
tests.calc: .F

-----
Fail:
tests.template.render(mytest at line 34): test expression [ Your message: sucktastic = Your message: fantastic! ]
tests.calc(mytest at line 47): test expression [ 6 = 4 ]

-----
Ran 2 tests.

[mytest@DD/MM/YYYY:hh:mm:ss] errors = 2                                                              [ FAILURE  ]
```


## Template

For example, create `myhello` with these contents:

```bash
#!/usr/bin/env oyster
import template

file="$(mktemp)"
echo "{{ place }} is {{ weather }} today." > "${file}"

# echo message
how_weather() {
    local context params result

    context=$(template.loader "${file}")
    params=(
        place="Tokyo"
        weather="sunny"
    )

    result=$(template.render "${context}" "${params[@]}")
    echo "${result}"
}
```

You can run by using command:

```shell
$ ./myhello how_weather
Tokyo is sunny today.
```

## LICENSE

Oyster is released under an MIT-style license; see `LICENSE` for details.
