# Robot Framework

Robot Framework is a ``Python-based``, extensible ``keyword-driven`` automation framework for ``acceptance testing, acceptance test driven development (ATDD), behavior driven development (BDD) and robotic process automation (RPA)``. It can be used in distributed, heterogeneous environments, where automation requires using different technologies and interfaces.

### Advantages

* Enables easy-to-use tabular syntax for creating test cases in a uniform way.
* Provides ability to create reusable higher-level keywords from the existing keywords.
* Provides easy-to-read result reports and logs in HTML format.
* Is platform and application independent.
* Provides a simple library API for creating customized test libraries which can be implemented natively with either Python or Java.
* Provides a command line interface and XML based output files for integration into existing build infrastructure (continuous integration systems).
* Provides support for Selenium for web testing, Java GUI testing, running processes, Telnet, SSH, and so on.
* Supports creating data-driven test cases.
* Has built-in support for variables, practical particularly for testing in different environments.
* Provides tagging to categorize and select test cases to be executed.
* Enables easy integration with source control: test suites are just files and directories that can be versioned with the production code.
* Provides test-case and test-suite -level setup and teardown.
* The modular architecture supports creating tests even for applications with several diverse interfaces.


### Installation
We can use pip or build it from a jar file through the `setup.py`

```python
# Install the latest version (does not upgrade)
pip install robotframework

# Upgrade to the latest version
pip install --upgrade robotframework

# Install a specific version
pip install robotframework==4.0.3

# Install separately downloaded package (no network connection needed)
pip install robotframework-4.0.3.tar.gz

# Install latest (possibly unreleased) code directly from GitHub
pip install https://github.com/robotframework/robotframework/archive/master.zip

# Uninstall
pip uninstall robotframework

```
###   Creating test data

#### Syntax

**Settings**	* 1) Importing test libraries, resource files and variable files.
                * 2) Defining metadata for test suites and test cases.

**Variables**	 * Defining variables that can be used elsewhere in the test data.

**Test Cases**	 * Creating test cases from available keywords.

**Tasks**	* Creating tasks using available keywords. Single file can only contain either tests or tasks.

**Keywords**	* Creating user keywords from existing lower-level keywords

**Comments**	Additional comments or data. Ignored by Robot Framework.


Different sections are recognized by their header row. The recommended header format is ***Settings***, but the header is case-insensitive, surrounding spaces are optional, and the number of asterisk characters can vary as long as there is one asterisk in the beginning. In addition to using the plural format, also singular variants like Setting and Test Case are accepted. In other words, also * setting would be recognized as a section header.

**Space separated format***
When Robot Framework parses data, it first splits the data to lines and then lines to tokens such as keywords and arguments. When using the space separated format, the separator between tokens is two or more spaces or alternatively one or more tab characters

```bash
*** Settings ***
Documentation     Example using the space separated format.
Library           OperatingSystem

*** Variables ***
${MESSAGE}        Hello, world!

*** Test Cases ***
My Test
    [Documentation]    Example test.
    Log    ${MESSAGE}
    My Keyword    ${CURDIR}

Another Test
    Should Be Equal    ${MESSAGE}    Hello, world!

*** Keywords ***
My Keyword
    [Arguments]    ${path}
    Directory Should Exist    ${path}
```

**Pipe separated format**
The biggest problem of the space delimited format is that visually separating keywords from arguments can be tricky. This is a problem especially if keywords take a lot of arguments and/or arguments contain spaces. In such cases the pipe delimited variant can work better because it makes the separator more visible.

```bash
| *** Settings ***   |
| Documentation      | Example using the pipe separated format.
| Library            | OperatingSystem

| *** Variables ***  |
| ${MESSAGE}         | Hello, world!

| *** Test Cases *** |                 |               |
| My Test            | [Documentation] | Example test. |
|                    | Log             | ${MESSAGE}    |
|                    | My Keyword      | ${CURDIR}     |
| Another Test       | Should Be Equal | ${MESSAGE}    | Hello, world!

| *** Keywords ***   |                        |         |
| My Keyword         | [Arguments]            | ${path} |
|                    | Directory Should Exist | ${path} |
```

Possible pipes surrounded by spaces in the actual test data must be escaped with a backslash, though:

```bash
| *** Test Cases *** |                 |                 |                      |
| Escaping Pipe      | ${file count} = | Execute Command | ls -1 *.txt \| wc -l |
|                    | Should Be Equal | ${file count}   | 42                   |

```

**reStructuredText format**
[reStructuredText (reST)](https://en.wikipedia.org/wiki/ReStructuredText) is an easy-to-read plain text markup syntax that is commonly used for documentation of Python projects, including Python itself as well as this User Guide. reST documents are most often compiled to HTML, but also other output formats are supported. Using reST with Robot Framework allows you to mix richly formatted documents and test data in a concise text format that is easy to work with using simple text editors, diff tools, and source control systems.

```bash
reStructuredText example
------------------------

This text is outside code blocks and thus ignored.

.. code:: robotframework

   *** Settings ***
   Documentation    Example using the reStructuredText format.
   Library          OperatingSystem

   *** Variables ***
   ${MESSAGE}       Hello, world!

   *** Test Cases ***
   My Test
       [Documentation]    Example test.
       Log    ${MESSAGE}
       My Keyword    ${CURDIR}

   Another Test
       Should Be Equal    ${MESSAGE}    Hello, world!

Also this text is outside code blocks and ignored. Code blocks not
containing Robot Framework data are ignored as well.

.. code:: robotframework

   # Both space and pipe separated formats are supported.

   | *** Keyword ***  |                        |         |
   | My Keyword       | [Arguments]            | ${path} |
   |                  | Directory Should Exist | ${path} |

.. code:: python

   # This code block is ignored.
   def example():
       print('Hello, world!')
```
Robot Framework supports reStructuredText files using both .rst and .rest extension. When executing a directory containing reStucturedText files, the --extension option must be used to explicitly tell that these files should be parsed.

### Creating test cases


#### 1. Test Case Syntax

The first column in the test case section contains test case names. A test case starts from the row with something in this column and continues to the next test case name or to the end of the section.

The second column normally has keyword names.

Columns after the keyword name contain possible arguments to the specified keyword.

```bash
*** Test Cases ***
Valid Login
    Open Login Page
    Input Username    demo
    Input Password    mode
    Submit Credentials
    Welcome Page Should Be Open

Setting Variables
    Do Something    first argument    second argument
    ${value} =    Get Some Value
    Should Be Equal    ${value}    Expected value
```

**Settings in the Test Case section**
Test cases can also have their own settings. Setting names are always in the second column, where keywords normally are, and their values are in the subsequent columns. Setting names have square brackets around them to distinguish them from keywords. The available settings are listed below and explained later in this section.

```
[Documentation]
	Used for specifying a test case documentation.
[Tags]
	Used for tagging test cases.
[Setup], [Teardown]
	Specify test setup and teardown.
[Template]
	Specifies the template keyword to use. The test itself will contain only data to use as arguments to that keyword.
[Timeout]
	Used for setting a test case timeout. Timeouts are discussed in their own section.
```

An Example

```bash
*** Test Cases ***
Test With Settings
    [Documentation]    Another dummy test
    [Tags]    dummy    owner-johndoe
    Log    Hello, world!

```
**Using Arguments**
Keywords can accept zero or more arguments, and some arguments may have default values. What arguments a keyword accepts depends on its implementation, and typically the best place to search this information is keyword's documentation

**Positional arguments**
Most keywords have a certain number of arguments that must always be given. In the keyword documentation this is denoted by specifying the argument names separated with a comma like *first, second, third*. The argument names actually do not matter in this case, except that they should explain what the argument does, but it is important to have exactly the same number of arguments as specified in the documentation. Using too few or too many arguments will result in an error.

The test below uses keywords Create Directory and Copy File from the OperatingSystem library. Their arguments are specified as *path and source, destination*, which means that they take one and two arguments, respectively. The last keyword, No Operation from BuiltIn, takes no arguments.

```bash
*** Test Cases ***
Example
    Create Directory    ${TEMPDIR}/stuff
    Copy File    ${CURDIR}/file.txt    ${TEMPDIR}/stuff
    No Operation
```

**Default values**
Arguments often have default values which can either be given or not.

Using default values is illustrated by the example below that uses Create File keyword which has arguments *path, content=, encoding=UTF-8*. Trying to use it without any arguments or more than three arguments would not work.

```bash
*** Test Cases ***
Example
    Create File    ${TEMPDIR}/empty.txt
    Create File    ${TEMPDIR}/utf-8.txt         Hyvä esimerkki
    Create File    ${TEMPDIR}/iso-8859-1.txt    Hyvä esimerkki    ISO-8859-1
```

**Variable number of arguments**
It is also possible that a keyword accepts any number of arguments. These so called varargs can be combined with mandatory arguments and arguments with default values, but they are always given after them. In the documentation they have an asterisk before the argument name like *(asteriks) varargs*.


For example, Remove Files and Join Paths keywords from the OperatingSystem library have arguments *(ateriks)paths and base, (asteriks)parts*, respectively. The former can be used with any number of arguments, but the latter requires at least one argument


```bash
*** Test Cases ***
Example
    Remove Files    ${TEMPDIR}/f1.txt    ${TEMPDIR}/f2.txt    ${TEMPDIR}/f3.txt
    @{paths} =    Join Paths    ${TEMPDIR}    f1.txt    f2.txt    f3.txt    f4.txt

```

**Named arguments**
The named argument syntax makes using arguments with default values more flexible, and allows explicitly labeling what a certain argument value means. Technically named arguments work exactly like keyword arguments in Python.


**basic syntax** : When the named argument syntax is used with **user keywords**, the argument names must be given without the ${} decoration. For example, user keyword with arguments ${arg1}=first, ${arg2}=second must be used like arg2=override.

Using normal positional arguments after named arguments like, for example, | Keyword | arg=value | positional |, does not work. The relative order of the named arguments does not matter.

**Named arguments with variables**
It is possible to use **variables** in both named argument names and values. If the value is a single **scalar variable**, it is passed to the keyword as-is. This allows using any objects, not only strings, as values also when using the named argument syntax. For example, calling a keyword like *arg=${object}* will pass the variable ${object} to the keyword without converting it to a string.

The named argument syntax requires the equal sign to be written literally in the keyword call. This means that variable alone can never trigger the named argument syntax, not even if it has a value like *foo=bar*. This is important to remember especially when wrapping keywords into other keywords. If, for example, a keyword takes a variable number of arguments like *@{args}* and passes all of them to another keyword using the same *@{args}* syntax, possible *named=arg* syntax used in the calling side is not recognized. This is illustrated by the example below.

```bash
*** Test Cases ***
Example
    Run Program    shell=True    # This will not come as a named argument to Run Process

*** Keywords ***
Run Program
    [Arguments]    @{args}
    Run Process    program.py    @{args}    # Named arguments are not recognized from inside @{args}

```

As already explained, the named argument syntax works with keywords. In addition to that, it also works when importing libraries.

The following example demonstrates using the named arguments syntax with *library keywords*, user keywords, and when importing the Telnet test library.

```bash
*** Settings ***
Library    Telnet    prompt=$    default_log_level=DEBUG

*** Test Cases ***
Example
    Open connection    10.0.0.42    port=${PORT}    alias=example
    List files    options=-lh
    List files    path=/tmp    options=-l

*** Keywords ***
List files
    [Arguments]    ${path}=.    ${options}=
    Execute command    ls ${options} ${path}
```

**Free Named Arguments**
Free named arguments support variables similarly as named arguments. In practice that means that variables can be used both in names and values, but the escape sign must always be visible literally. For example, both foo=${bar} and ${foo}=${bar} are valid, as long as the variables that are used exist. An extra limitation is that free argument names must always be strings.

As the first example of using free named arguments, let's take a look at Run Process keyword in the **Process** library. It has a signature command, (asteriks)arguments, (double asteriks)configuration, which means that it takes the command to execute (command), its arguments as variable number of arguments ((asteriks)arguments) and finally optional configuration parameters as free named arguments (configuration). The example below also shows that variables work with free keyword arguments exactly like when using the named argument syntax.

```bash
*** Test Cases ***
Free Named Arguments
    Run Process    program.py    arg1    arg2    cwd=/home/user
    Run Process    program.py    argument    shell=True    env=${ENVIRON}
```


As the second example, let's create a wrapper user keyword for running the program.py in the above example. The wrapper keyword Run Program accepts all positional and named arguments and passes them forward to Run Process along with the name of the command to execute.

```bash
*** Test Cases ***
Free Named Arguments
    Run Program    arg1    arg2    cwd=/home/user
    Run Program    argument    shell=True    env=${ENVIRON}

*** Keywords ***
Run Program
    [Arguments]    @{args}    &{config}
    Run Process    program.py    @{args}    &{config}
```


As an example of using the named-only arguments with user keywords, here is a variation of the Run Program in the above free named argument examples that only supports configuring shell:

```bash
*** Test Cases ***
Named-only Arguments
    Run Program    arg1    arg2              # 'shell' is False (default)
    Run Program    argument    shell=True    # 'shell' is True

*** Keywords ***
Run Program
    [Arguments]    @{args}    ${shell}=False
    Run Process    program.py    @{args}    shell=${shell}
```

### Failures

**When test case fails**
A test case fails if any of the keyword it uses fails. Normally this means that execution of that test case is stopped, possible test **teardown** is executed, and then execution continues from the next test case. It is also possible to use special continuable failures if stopping test execution is not desired.

**Error messages**
The error message assigned to a failed test case is got directly from the failed keyword. Often the error message is created by the keyword itself, but some keywords allow configuring them.

By default error messages are normal text, but they can contain HTML formatting. This is enabled by starting the error message with marker string *HTML*. This marker will be removed from the final error message shown in reports and logs. Using HTML in a custom message is shown in the second example below.

```bash
*** Test Cases ***
Normal Error
    Fail    This is a rather boring example...

HTML Error
    ${number} =    Get Number
    Should Be Equal    ${number}    42    *HTML* Number is not my <b>MAGIC</b> number.
```

**Test Caxe Names and Docummentation**
The test case name comes directly from the Test Case section: it is exactly what is entered into the test case column. Test cases in one test suite should have unique names. Pertaining to this, you can also use the automatic variable ${TEST_NAME} within the test itself to refer to the test name

```bash
*** Variables ***
${MAX AMOUNT}      ${5000000}

*** Test Cases ***
Amount cannot be larger than ${MAX AMOUNT}
    # ...
```

The [Documentation] setting allows you to set a free documentation for a test case. That text is shown in the command line output, as well as the resulting test logs and test reports. It is possible to use simple HTML formatting in documentation and variables can be used to make the documentation dynamic. Possible non-existing variables are left unchanged.


```bash
*** Test Cases ***
Simple
    [Documentation]    Simple documentation
    No Operation

Formatting
    [Documentation]    *This is bold*, _this is italic_  and here is a link: http://robotframework.org
    No Operation

Variables
    [Documentation]    Executed at ${HOST} by ${USER}
    No Operation

Splitting
    [Documentation]    This documentation    is split    into multiple columns
    No Operation

Many lines
    [Documentation]    Here we have
    ...                an automatic newline
    No Operation
```
**Tagging test cases**
Using tags in Robot Framework is a simple, yet powerful mechanism for classifying test cases. Tags are free text and they can be used at least for the following purposes:


* Tags are shown in test reports, logs and, of course, in the test data, so they provide metadata to test cases.
* Statistics about test cases (total, passed, failed are automatically collected based on tags).
* With tags, you can include or exclude test cases to be executed.
* With tags, you can specify which test cases should be skipped.

Tags are free text, but they are normalized so that they are converted to lowercase and all spaces are removed. If a test case gets the same tag several times, other occurrences than the first one are removed. Tags can be created using variables, assuming that those variables exist


```bash
*** Settings ***
Force Tags      req-42
Default Tags    owner-john    smoke

*** Variables ***
${HOST}         10.0.1.42

*** Test Cases ***
No own tags
    [Documentation]    This test has tags owner-john, smoke and req-42.
    No Operation

With own tags
    [Documentation]    This test has tags not_ready, owner-mrx and req-42.
    [Tags]    owner-mrx    not_ready
    No Operation

Own tags with variables
    [Documentation]    This test has tags host-10.0.1.42 and req-42.
    [Tags]    host-${HOST}
    No Operation

Empty own tags
    [Documentation]    This test has only tag req-42.
    [Tags]
    No Operation

Set Tags and Remove Tags Keywords
    [Documentation]    This test has tags mytag and owner-john.
    Set Tags    mytag
    Remove Tags    smoke    req-*
```

**Reserved tags**
Users are generally free to use whatever tags that work in their context. There are, however, certain tags that have a predefined meaning for Robot Framework itself, and using them for other purposes can have unexpected results. All special tags Robot Framework has and will have in the future have the robot: prefix. To avoid problems, users should thus not use any tag with this prefixes unless actually activating the special functionality.


###  Test setup and teardown

Robot Framework has similar test setup and teardown functionality as many other test automation frameworks. In short, a test setup is something that is executed before a test case, and a test teardown is executed after a test case. In Robot Framework setups and teardowns are just normal keywords with possible arguments


The test teardown is special in two ways. First of all, it is executed also when a test case fails, so it can be used for clean-up activities that must be done regardless of the test case status. In addition, all the keywords in the teardown are also executed even if one of them fails. This continue on failure functionality can be used also with normal keywords, but inside teardowns it is on by default.

The easiest way to specify a setup or a teardown for test cases in a test case file is using the Test Setup and Test Teardown settings in the Setting section. Individual test cases can also have their own setup or teardown. They are defined with the [Setup] or [Teardown] settings in the test case section and they override possible Test Setup and Test Teardown settings. Having no keyword after a [Setup] or [Teardown] setting means having no setup or teardown. It is also possible to use value NONE to indicate that a test has no setup/teardown.

```bash

*** Settings ***
Test Setup       Open Application    App A
Test Teardown    Close Application

*** Test Cases ***
Default values
    [Documentation]    Setup and teardown from setting section
    Do Something

Overridden setup
    [Documentation]    Own setup, teardown from setting section
    [Setup]    Open Application    App B
    Do Something

No teardown
    [Documentation]    Default setup, no teardown at all
    Do Something
    [Teardown]

No teardown 2
    [Documentation]    Setup and teardown can be disabled also with special value NONE
    Do Something
    [Teardown]    NONE

Using variables
    [Documentation]    Setup and teardown specified using variables
    [Setup]    ${SETUP}
    Do Something
    [Teardown]    ${TEARDOWN}
```
The name of the keyword to be executed as a setup or a teardown can be a variable. This facilitates having different setups or teardowns in different environments by giving the keyword name as a variable from the command line.


###  Test templates
Test templates convert normal **keyword-driven** test cases into **data-driven** tests. Whereas the body of a keyword-driven test case is constructed from keywords and their possible arguments, test cases with template contain only the arguments for the template keyword. Instead of repeating the same keyword multiple times per test and/or with all tests in a file, it is possible to use it only per test or just once per file.

Template keywords can accept both normal positional and named arguments, as well as arguments embedded to the keyword name. Unlike with other settings, it is not possible to define a template using a variable.

**Basic usage**
How a keyword accepting normal positional arguments can be used as a template is illustrated by the following example test cases. These two tests are functionally fully identical.

```bash
*** Test Cases **
Normal test case
    Example keyword    first argument    second argument

Templated test case
    [Template]    Example keyword
    first argument    second argument

```

As the example illustrates, it is possible to specify the template for an individual test case using the [Template] setting. An alternative approach is using the Test Template setting in the Setting section, in which case the template is applied for all test cases in that test case file.


```bash
*** Settings ***
Test Template    Example keyword

*** Test Cases ***
Templated test case
    first round 1     first round 2
    second round 1    second round 2
    third round 1     third round 2

```

Using keywords with default values or accepting variable number of arguments, as well as using named arguments and free named arguments, work with templates exactly like they work otherwise. Using variables in arguments is also supported normally.


**Templates with embedded arguments**
Templates support a variation of the embedded argument syntax. With templates this syntax works so that if the template keyword has variables in its name, they are considered placeholders for arguments and replaced with the actual arguments used with the template. The resulting keyword is then used without positional arguments. This is best illustrated with an example:

```bash
*** Test Cases ***
Normal test case with embedded arguments
    The result of 1 + 1 should be 2
    The result of 1 + 2 should be 3

Template with embedded arguments
    [Template]    The result of ${calculation} should be ${expected}
    1 + 1    2
    1 + 2    3

*** Keywords ***
The result of ${calculation} should be ${expected}
    ${result} =    Calculate    ${calculation}
    Should Be Equal    ${result}     ${expected}
```

When embedded arguments are used with templates, the number of arguments in the template keyword name must match the number of arguments it is used with. The argument names do not need to match the arguments of the original keyword, though, and it is also possible to use different arguments altogether:

```bash
*** Test Cases ***
Different argument names
    [Template]    The result of ${foo} should be ${bar}
    1 + 1    2
    1 + 2    3

Only some arguments
    [Template]    The result of ${calculation} should be 3
    1 + 2
    4 - 1

New arguments
    [Template]    The ${meaning} of ${life} should be 42
    result    21 * 2
```


The main benefit of using embedded arguments with templates is that argument names are specified explicitly.
When using normal arguments, the same effect can be achieved by naming the columns that contain arguments. This is illustrated by the data-driven style example in the next section.

**Templates with for loops**
If templates are used with for loops, the template is applied for all the steps inside the loop. The continue on failure mode is in use also in this case, which means that all the steps are executed with all the looped elements even if there are failures.

```bash
*** Test Cases ***
Template with for loop
    [Template]    Example keyword
    FOR    ${item}    IN    @{ITEMS}
        ${item}    2nd arg
    END
    FOR    ${index}    IN RANGE    42
        1st arg    ${index}
    END
```
**Templates with if expression**
If expression can be also used together with templates. This can be useful, for example, when used together with for loops to filter executed arguments.

```bash
*** Test Cases ***
Template with for and if
    [Template]    Example keyword
    FOR    ${item}    IN    @{ITEMS}
        IF  ${item} < 5
            ${item}    2nd arg
        END
    END
```


### Different test case styles
There are several different ways in which test cases may be written. Test cases that describe some kind of workflow may be written either in keyword-driven or behavior-driven style. Data-driven style can be used to test the same workflow with varying input data.

**Keyword-driven style**
Workflow tests, such as the Valid Login test described earlier, are constructed from several keywords and their possible arguments. Their normal structure is that first the system is taken into the initial state (Open Login Page in the Valid Login example), then something is done to the system (Input Name, Input Password, Submit Credentials), and finally it is verified that the system behaved as expected (Welcome Page Should Be Open).


**Data-driven style**
Another style to write test cases is the data-driven approach where test cases use only one higher-level keyword, often created as a user keyword, that hides the actual test workflow. These tests are very useful when there is a need to test the same scenario with different input and/or output data. It would be possible to repeat the same keyword with every test, but the test template functionality allows specifying the keyword to use only once.

```bash
*** Settings ***
Test Template    Login with invalid credentials should fail

*** Test Cases ***                USERNAME         PASSWORD
Invalid User Name                 invalid          ${VALID PASSWORD}
Invalid Password                  ${VALID USER}    invalid
Invalid User Name and Password    invalid          invalid
Empty User Name                   ${EMPTY}         ${VALID PASSWORD}
Empty Password                    ${VALID USER}    ${EMPTY}
Empty User Name and Password      ${EMPTY}         ${EMPTY}
```

**Behavior-driven style**
It is also possible to write test cases as requirements that also non-technical project stakeholders must understand. These executable requirements are a corner stone of a process commonly called Acceptance Test Driven Development (ATDD) or Specification by Example.

One way to write these requirements/tests is Given-When-Then style popularized by Behavior Driven Development (BDD). When writing test cases in this style, the initial state is usually expressed with a keyword starting with word Given, the actions are described with keyword starting with When and the expectations with a keyword starting with Then. Keyword starting with And or But may be used if a step has more than one action.


```bash
*** Test Cases ***
Valid Login
    Given login page is open
    When valid username and password are inserted
    and credentials are submitted
    Then welcome page should be open
```















