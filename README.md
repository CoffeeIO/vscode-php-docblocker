# PHP DocBlocker - CoffeeIO

This is a forked version of PHP DocBlocker by neilbrayfield(neild3r).

Added features:
- Optional alignment of @param statements
- Optional alignment of @return statement



---



[![Latest Release](https://vsmarketplacebadge.apphb.com/version-short/coffeeio.php-docblocker.svg)](https://marketplace.visualstudio.com/items?itemName=coffeeio.php-docblocker) [![Installs](https://vsmarketplacebadge.apphb.com/installs/coffeeio.php-docblocker.svg)](https://marketplace.visualstudio.com/items?itemName=coffeeio.php-docblocker) [![Rating](https://vsmarketplacebadge.apphb.com/rating-short/coffeeio.php-docblocker.svg)](https://marketplace.visualstudio.com/items?itemName=coffeeio.php-docblocker) [![Build status](https://travis-ci.org/coffeeio/vscode-php-docblocker.svg?branch=master)](https://travis-ci.org/coffeeio/vscode-php-docblocker) [![Coverage status](https://coveralls.io/repos/github/coffeeio/vscode-php-docblocker/badge.svg)](https://coveralls.io/github/coffeeio/vscode-php-docblocker)

Basic PHP DocBlocking extension.

We now have a set of unit tests and some full coverage on the parsing of signatures as well as continuous integration. This should ensure the extension remains stable as development progresses.

## Features

* Completion snippet after `/**` above a class, function, class property
* Continuation of DocBlock when pressing enter when in a DocBlock
* Completion of DocBlock tags such as `@param`, `@return`, `@throws`
* Inferring of param and return types from signatures
* Configuration of template for each type of docblock completion

## Requirements

This extension has no dependencies.

## Extension Settings

This extension contributes the following settings:

* `php-docblocker-coffeeio.gap`: set to `false` to disable the gap between the description and tags
* `php-docblocker-coffeeio.returnGap`: set to `true` to add a gap between the param and return tags
* `php-docblocker-coffeeio.returnVoid`: set to `false` to turn off the automatic void return type when it can't be determined
* `php-docblocker-coffeeio.extra`: an array of extra tags to add to each DocBlock (These can include tabstops and snippet variables)
* `php-docblocker-coffeeio.useShortNames`: Whether we should use short type names. e.g. bool or boolean
* `php-docblocker-coffeeio.qualifyClassNames`: When adding type hints for class names search namespace use statements and qualify the class
* `php-docblocker-coffeeio.alignParams`: set to `true` to align params vertically and add appropriate spaces after param names
* `php-docblocker-coffeeio.alignReturn`: set to `true` to align return vertically with above params statements, this setting requires align params to also be active
* `php-docblocker-coffeeio.author`: An object containing your default author tag settings
* `php-docblocker-coffeeio.functionTemplate`: See below for how to set up docblock templates
* `php-docblocker-coffeeio.propertyTemplate`: See below for how to set up docblock templates
* `php-docblocker-coffeeio.classTemplate`: See below for how to set up docblock templates

### Templating

If you want more control over the order or gap settings on your docblocks or you want different things for properties vs class templates
you can start customising the template configuration objects. These are the config options `functionTemplate`, `propertyTemplate` and
`classTemplate`.

#### Default set up for function

The below is the default set up for a function. The order of the keys represents the output order. There are no specific options in each
config option per key to add additional control.

```json
    {
        "message": {},
        "param": {},
        "return": {},
        "extra": {}
    }
```

#### Supported template keys

| Key             | Aplies to type  | Description                                                                       |
|-----------------|-----------------|-----------------------------------------------------------------------------------|
| message         | All             | Space for entering a description of your block                                    |
| extra           | All             | Adds in your custom tags from the extra config                                    |
| param           | Function        | Function @param items                                                             |
| return          | Function        | Function @return item                                                             |
| var             | Property        | Property @var item                                                                |
| *               | All             | This is for any key that is unmatched you can use the content option to add a tag |

#### Supported template config options

| Option          | Aplies to key(s) | Description                                    |
|-----------------|------------------|------------------------------------------------|
| gapBefore       | All              | Adds a gap before the tag section starts       |
| gapAfter        | All              | Adds a gap after the tag section ends          |
| content         | *                | Adds a gap after the tag section ends          |

#### Configured function template example

In the example below we have added some gap configuration and removed the return tag for our template as well as
changing the default order. This means we'll never have a @return tag and extra comes before the params. It's also
worth pointing out that the gapAfter in the message is the same as setting the gap config option in the main config
to true.

```json
    {
        "message": {
            "gapAfter": true
        },
        "extra": {},
        "param": {
            "gapBefore": true
        },
    }
```

#### Configured function with extra content and placeholders

The example below won't have a return tag and will add in an author tag with correct placeholders depending on
how many options you have. You can put in placeholders by using `###` in place of the tab stop number and it
will be calculated at generation time.

```json
    {
        "message": {
            "gapAfter": true
        },
        "param": {
            "gapBefore": true
        },
        "author": {
            "content": "@author ${###:Neil Brayfield} <${###:neil@test.com}>"
        }
    }
```

## Supported DocBlock tags

Please see below for a list of supported tags and their snippets. These tags are available within a DocBlock
and can be triggered by typing @ then another characted (Provided your vscode settings allow).

| Tag                          | Snippet                                 |
| ---------------------------- | --------------------------------------- |
| @api                         | @api                                    |
| @abstract                    | @abstract                               |
| @after                       | @after                                  |
| @afterClass                  | @afterClass                             |
| @author                      | @author ${1:{{name}}} <${2:{{email}}}>  |
| @backupGlobals               | @backupGlobals ${1:switch}              |
| @backupStaticAttributes      | @backupStaticAttributes ${1:switch}     |
| @before                      | @before                                 |
| @beforeClass                 | @beforeClass                            |
| @category                    | @category ${1:description}              |
| @codeCoverageIgnore          | @codeCoverageIgnore                     |
| @codeCoverageIgnoreEnd       | @codeCoverageIgnoreEnd                  |
| @codeCoverageIgnoreStart     | @codeCoverageIgnoreStart                |
| @copyright                   | @copyright ${1:2018} ${2:Name}          |
| @covers                      | @covers ${1:fqcn}                       |
| @coversDefaultClass          | @coversDefaultClass ${1:fqcn}           |
| @coversNothing               | @coversNothing                          |
| @dataProvider                | @dataProvider ${1:methodName}           |
| @depends                     | @depends ${1:methodName}                |
| @deprecated                  | @deprecated ${1:version}                |
| @doesNotPerformAssertions    | @doesNotPerformAssertions               |
| @example                     | @example ${1:location} ${2:description} |
| @filesource                  | @filesource                             |
| @final                       | @final                                  |
| @group                       | @group ${1:group}                       |
| @global                      | @global                                 |
| @ignore                      | @ignore ${1:description}                |
| @inheritDoc                  | @inheritDoc                             |
| @internal                    | @internal ${1:description}              |
| @large                       | @large                                  |
| @license                     | @license ${1:MIT}                       |
| @link                        | @link ${1:http://url.com}               |
| @medium                      | @medium                                 |
| @method                      | @method ${1:mixed} ${2:methodName()}    |
| @package                     | @package ${1:category}                  |
| @param                       | @param ${1:mixed} $${2:name}            |
| @preserveGlobalState         | @preserveGlobalState                    |
| @property                    | @property ${1:mixed} $${2:name}         |
| @property-read               | @property-read ${1:mixed} $${2:name}    |
| @property-write              | @property-write ${1:mixed} $${2:name}   |
| @requires                    | @requires ${1:mixed}                    |
| @return                      | @return ${1:mixed}                      |
| @runInSeparateProcess        | @runInSeparateProcess                   |
| @runTestsInSeparateProcesses | @runTestsInSeparateProcesses            |
| @see                         | @see ${1:http://url.com}                |
| @since                       | @since ${1:1.0.0}                       |
| @small                       | @small                                  |
| @source                      | @source ${1:location} ${2:description}  |
| @static                      | @static                                 |
| @subpackage                  | @subpackage ${1:category}               |
| @test                        | @test                                   |
| @testdox                     | @testdox ${1:description}               |
| @testWith                    | @testWith ${1:elements}                 |
| @throws                      | @throws ${1:Exception}                  |
| @ticket                      | @ticket ${1:ticket}                     |
| @todo                        | @todo ${1:Something}                    |
| @uses                        | @uses ${1:MyClass::function} ${2:Name}  |
| @var                         | @var ${1:mixed}                         |
| @version                     | @version ${1:1.0.0}                     |

