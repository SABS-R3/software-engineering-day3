---
title: Discussion
---

Possible answers to first group exercise in Introduction to Development Practices lesson.

## Repository itself
- Repo name is unhelpful rf4
- The README.md mentions this code has been developed since 2010, but the commits indicate it's only just been committed to GitHub
- No CITATION file
- LICENSE file is empty
- Commits with cursory commit messages (e.g. 'Change', 'Not working')
- Code is not in a working state on master branch - developing code should either be on a separate branch, or file releases made avai$
- No tests
- GitHub has highlighted potential security vulnerabilities with the repo's dependencies

## README.md
- No prerequisites
- No instructions for running the code
- No description of the data used by the software that is in the repository
- Incomplete description of the repository data directory
- Links to blog post that isn't there

## What questions do we want to answer with this data?
-  No real content, comment that says it'll be finished later

## Code
- Arbitrary copy of the .py with a _working suffix, no explanation
- Many function docstrings missing (including key functions)
- Inconsistent commenting of logger throughout
- Logger retrieved in each function (and sometimes not used), when it could be done once and referenced later
- Hard coded file path references inline
- print(len(df)) with no context, comment - why isn't it logged?
- Commenting good in places, nonexistent commenting in others
- Many variable names are non-descriptive single letters
- The code doesn't work! (df renamed to df47 in main()
- The main() function is overlong and uncommented
- Commented out function
- Two functions have a number in the function name without explanation
- Too many spaces in add_column5 function inconsistent with coding style
- Some ultra-long code lines that should be properly formatted over multiple lines

{% include links.md %}
