---
layout: post
title: "git bisect run automates bug hunting"
tags: [git, debugging]
---

I always used `git bisect` manually — marking commits as `good` or `bad`
one at a time. Tedious. Turns out you can fully automate it:

```bash
git bisect start HEAD v1.0
git bisect run ./test.sh
```

Git does a binary search through your commit history, running the script at
each step. Exit code `0` means good, non-zero means bad. It finds the
first bad commit on its own.

Any executable works as the test. A test suite, a `grep`, a one-liner:

```bash
git bisect run sh -c "make && ./run_tests | grep PASS"
```

For a repo with 1000 commits, that's ~10 test runs instead of 1000.
Binary search wins again.
