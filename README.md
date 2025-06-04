# Github CI Test

A coworker recently asserted to me that Github Actions runs its PR checks on a post-merged copy of the code in the PR, not the pre-merged copy. This seemed super wrong to me, so I tested it.

## Setup

This is a super-simple CI setup wherein the github action config just runs every shell script in `/tests`. Each test just prints out that it ran.

## The test

1. Create a main branch and add the base test (test_1.sh)
2. Create a branch off of main (`outdated_branch`) and add test_2.sh
3. Switch back to main and add test_3.sh

```mermaid
graph TD;
    A[main: Add test_1.sh] --> C[outdated_branch: Add test_2.sh]
    A --> D[main: Add test_3.sh]

    style A fill:#e1f5fe
    style C fill:#fff3e0
    style D fill:#e1f5fe
```

Then I opened a PR from 'outdated_branch' to 'main'. I expected to only see tests 1 and 2 run, since those are the only tests on `outdated_branch`, but I actually saw that all three tests ran. This implies that my coworker was right: github PR actions run on the post-merge copy of your code, not the pre-merge copy!
