@title[Cover]

# #! /bin/bash

#### It Does Not Have to Hurt to Write/Learn
<br />
<br />
<span class="byline">stephen.chu</span>


---
@title[Why Bash]

### Why Bash?

- Ubiquitous (Linux/Unix/Windows/Raspberry/Embedded)
- Glue Language
- Not meant to be: OO/Functional/Prototypical
- Just a "scripting language"


---
@title[What This Talk Is Not]

### What This Talk Is Not

+++
@title[Commands/Processes]

ls / grep / printf / awk

+++
@title[File Systems]

xfs / ext3

+++
@title[Network] 

/var/run/postgresql/s.PGSQL.5432
localhost:8080


---
@title[Table of Content]

### What This Talk Is About

+++
@title[Education]

Education/Tips/Tricks

+++
@title[Follow]

Follow conventions/idioms/guidlines

+++
@title[Write Better Scripts]
Don't create 💣


---
#### Agenda

1. File extensions
1. Functions & Libraries (exit codes, return)
1. If-Else Idioms
1. stdin, stdout, stderr and redirections
1. Parameter expansions: ${}, $(), <(), >()
1. Pipelines: |
1. Fail Fast "strict" Mode
1. Debugging
1. Testing
1. Advanced: exec
1. Advanced: login/interactive shell


---
@title[Filename Extensions]

#### Q: Should your Bash scripts/files be named like...

+++
@title[Example]

- `foo.sh`, or
- `foo`?

![Question](images/file-extensions/question.png)


+++
@title[When In Doubt]

#### When In Doubt:

##### Google Shell Scripting Style Guide

<span style="font-size:0.6em">`https://google.github.io/styleguide/shell.xml`</span>


+++
@title[Style Guide Says]

![Google Says](images/file-extensions/google-says.png)


+++
@title[Answer]

#### Answer:

![Answer](images/file-extensions/answer.png)


+++
@title[In Other Words]

```sh
#! /bin/bash

source _cowsay.sh
source _slack.sh
source _ssh.sh

.
.
.
```
@[1,1-3](Use `source` to bring in other functions. Python `import`)


---
@title[Functions]

#### Bash Functions

+++
@title[Example]

```sh
#! /bin/bash
happy_birthday() {
  local who="$1"
  echo "Happy birthday, $who!"
}
```
@[2](You don't need the keyword `function` in front.)
@[3](Inside a function, there are local variables. Outside, none.)
@[3](Function arguments are positional only: `$1`, `$2`, etc.)


+++
@title[Namespace]

#### Better:

```sh
#! /bin/bash
say:happy_birthday() {
  local who="$1"
  echo "Happy birthday, $who!"
}
```
@[2](Function name can contain `:`, `-`, etc.)
@[2](Google Style Guide says use `::`, I prefer single `:`.)


---
@title[Exit Code]

#### Command/Program Exit Codes

Used to indicate success/failure

* 0: Was successful
* non-zero: Was error'ed out

```sh
> ./deploy -e sandbox
  # ... 5 mins later ...
> echo $?
1
```
@[3](Exit codes are for computers; texts are for humans (more on this later).)


---
@title[Return]

#### Exit codes are "return"-ed by a program/function

```sh
#! /bin/bash
happy_birthday() {
  local who="$1"
  echo "Happy birthday, $who!"
}
```

+++
@title[Return-2]

#### But often, you don't have to do it explicitly

```sh
#! /bin/bash
deployment:any-errors() {
  grep --quiet "ERROR"
}

cat /var/log/syslog | deployment:any-errors
```

  EXIT STATUS
     0     One or more lines were selected.
     1     No lines were selected.
     >1    An error occurred.


---
@title[Return-3]

#### You Should Too

```sh
source _slack.sh

if ! slack:send_message "INFO: Deploy succeeded"; then
  echo "ERROR: Slack is down..."
  exit 1
fi
```
@[3](All conditionals work with exit codes.)
@[5](Any non-zero value works, conventionally <= 128.)


---
@title[STD{IN,OUT,ERR}]

#### <span style="font-color:grey">STD</span>IN, <span style="font-color:grey">STD</span>OUT, and <span style="font-color:grey">STD</span>ERR


+++
@title[STDIN and STDOUT]

STDIN and STDOUT: How running programs talk to *each other*.

```sh
cat /var/log/syslog | grep "ERROR"
```
@[1](2 processes: `cat` and `grep` are spun up.)
@[1](LHS stdout is connected to RHS stdin.)
@[1](By default, the `|` does NOT connect stderr between LHS and RHS.)


+++
@title[Broken Pipe]

```sh
```
@[1](RHS 'cya' without telling LHS.)


+++
@title[STDERR]

#### STDERR: How running programs talk to *humans*.

```sh
> ./deploy
INFO: Git checkout source code... DONE
INFO: Build Docker image... DONE
INFO: Run some tests... 
ERROR: You have no tests?!
> echo $?
1
```
@[1]()

---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
---
@title[]
