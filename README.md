# NAME

Perl::Critic::Policy::ProhibitUseLib - Let the script find its own lib dir instead of spelling out a path.

# VERSION

version 1.000

# Perl::Critic::Policy::ProhibitUseLib

`use lib "$FindBin::Bin/../lib"` is a path written out by hand, relative to a
script that may not stay where it is, repeated in every script that needs it:

```perl
use FindBin;
use lib "$FindBin::Bin/../lib";     # every script has this line
use lib "$FindBin::Bin/../../lib";  # ...and this is the one that moved
```

Nothing checks it.  Move a script one directory deeper and it keeps compiling
right up until it loads the wrong copy of a module, or a stale one somebody
left in the parent of the parent.

[FindBin::libs](https://metacpan.org/pod/FindBin%3A%3Alibs) walks up from the script and finds the `lib` directory
itself, which is both shorter and correct after the move:

```perl
use FindBin::libs;
```

## PROHIBITED

```perl
use lib "$FindBin::Bin/../lib";
use lib '/opt/myapp/lib';
use lib::relative '../lib';
```

## ALLOWED

```perl
use FindBin::libs;
use lib::abs '../lib';    # if you have told the policy to allow it
```

`no lib '...'` is left alone: taking a directory back off `@INC` is not the
thing this policy is about.

## CONFIGURATION

- `modules`

    Space separated list of module names to complain about.  Defaults to
    `lib lib::relative`.

    ```
    [ProhibitUseLib]
    modules = lib lib::relative lib::abs
    ```

## CAVEATS

This finds the pragma, not the practice.  A script that pushes onto `@INC`
directly, or that shells out to something with `PERL5LIB` set, is doing the
same thing by other means and goes unnoticed.

## METHODS

### supported\_parameters

`modules`, the list of pragma names to flag.

### default\_severity

SEVERITY\_MEDIUM

### default\_themes

maintenance, portability

### applies\_to

PPI::Statement::Include

### violates

Standard [Perl::Critic::Policy](https://metacpan.org/pod/Perl%3A%3ACritic%3A%3APolicy) interface.  Returns a violation for a `use`
of one of the configured pragmas, and nothing for anything else -- including
`no lib`, which removes a directory rather than adding one.

# BUGS

Please report any bugs or feature requests on the bugtracker website
[https://github.com/teodesian/perl-critic-policy-prohibituselib/issues](https://github.com/teodesian/perl-critic-policy-prohibituselib/issues)

When submitting a bug or request, please include a test-file or a
patch to an existing test-file that illustrates the bug or desired
feature.

# AUTHORS

Current Maintainers:

- George S. Baugh <teodesian@gmail.com>

# COPYRIGHT AND LICENSE

Copyright (c) 2026 Troglodyne LLC

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
