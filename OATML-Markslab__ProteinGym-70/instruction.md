# Issue

**Title:** Automate file downloading

## Description

To make setting up `ProteinGym` much simpler for users,  I've added 6 new functions to help with finding, downloading, and importing precomputed files provided by the Marks lab.

Example usage of new functions in `proteingym/utils/download.py`:
```python
import sys
sys.path.append('ProteinGym') # Make functions available

import proteingym.utils.download as pgd # Import functions
pgd.set_cache("my/local/dir/") # Set cache directory
print(pgs.PROTEINGYM_CACHE) # Get cache directory
resources_df = pgd.get_resources_df() # Get resources DataFrame
pg_resources = pgd.download_resources(resources_df.iloc[:2]) # Download resources
pgd.count_resources(pg_resources) # Count resources
```

I've also updated the `scripts/zero_shot_config.sh` file to make it more intuitive to users when trying to map onto precomputed files. Wherever possible, I've tried to match up the default folder names/structure to that in the precomputed zip files.


Finally, bumped to version 1.0.1 in `setup.py`.

Let me know if you need me to make any mods. Thanks!


Addresses #63

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1d603d28ed8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.