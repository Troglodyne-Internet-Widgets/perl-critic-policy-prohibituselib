# CLAUDE.md

This file is the procedure for work in this repository.

The repository holds one Perl::Critic policy, `ProhibitUseLib`.  It reports a
`use lib` line that spells out a path.  The code is in
`lib/Perl/Critic/Policy/ProhibitUseLib.pm`.  The tests are in `t/main.t`.

## Which skills, and when

The hooks of the perl-slop plugin hold you to the procedure.  They refuse an
edit to Perl until `perl-slop:reading-perl` is loaded.  They refuse a commit of
Perl until `perl-slop:data-perl`, `perl-slop:testing-perl` and
`perl-slop:reviewing-perl` are loaded.  A refusal names the skills that are
missing.

Before you answer a question about the code, load `perl-slop:reading-perl`.
The hooks cannot see that case.

## Read the code before you change it

A line that looks pointless is usually the scar of an earlier failure.  The
reason is in the commit, not in the file.  Read the history before the first
edit.

## Finishing a changeset

Apply the skills in this order: data-perl, testing-perl, then reviewing-perl
against the whole diff.  Then run `podchecker` on each changed file.

The pre-commit hook tidies the Perl that you staged.  The hub installs the
hooks.  Do not use `--no-verify`.

To run the tests, use:

    prove -l t/main.t

## When something is slow

Use `perl-slop:profiling-perl`.  Measure before you conclude anything, and
measure again after a change.

## Commits and pull requests

Make a branch.  Do not commit to `master`.  A commit message says what was
wrong and why the change fixes it.  Write it in prose and in the imperative
mood.  After you make sure that something works, say what you ran and what it
printed.  Do not put names of hosts, accounts or customers in a commit message
or a pull request.  `perl-slop:information-security` has the full rule.
