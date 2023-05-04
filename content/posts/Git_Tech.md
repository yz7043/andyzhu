---
title: "Git_Tech"
date: 2023-05-04T00:33:24-04:00
draft: false
---

# Introduction

Here is a memo of some git techniques that can speed up work efficiency.

# Content

## Duplicate a git repo with all its branch

You can find it in [Duplicating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository)

```bash
git clone --bare https://github.com/EXAMPLE-USER/OLD-REPOSITORY.git
cd OLD-REPOSITORY.git
git push --mirror https://github.com/EXAMPLE-USER/NEW-REPOSITORY.git
cd ..
rm -rf OLD-REPOSITORY.git
```

