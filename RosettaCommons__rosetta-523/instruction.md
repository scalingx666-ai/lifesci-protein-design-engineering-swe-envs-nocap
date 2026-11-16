# Issue

**Title:** Supporting secure unpickling in PyRosetta

## Description

Currently, `PackedPose` objects are serialized/deserialized using the `pickle` module (introduced in ~2019), and the `Pose.cache` dictionary (introduced in #430) supports caching arbitrary datatypes in the `Pose` object using the `pickle` module. Additionally, #462 enables saving compressed `PackedPose` objects to disk (i.e., as `*.b64_pose` and `*.pkl_pose` files) for sharing PyRosetta `Pose` objects with the scientific community. However, use of the `pickle` module is not secure (see warning [here](https://docs.python.org/3/library/pickle.html) as outlined in #519).

Herein this PR, a secure `pickle.loads` method is developed and slotted into the `PackedPose` and `Pose.cache` infrastructure to permanently disallow certain risky packages, modules, and namespaces from being unpickled/loaded (e.g., `exec`, `eval`, `os.system`, `subprocess.run`, etc., and will be updated over time as needed), thus significantly improving the security of handling `PackedPose` and `Pose` objects in memory if received from a second party (i.e., over a socket, queue, interprocess communication, etc.) or when reading a file received from a second party (i.e., using `pyrosetta.distributed.io.pose_from_file` with a `*.b64_pose` and `*.pkl_pose` file). By default, only `pyrosetta` and `numpy` packages, and certain `builtins` modules (like `dict`, `complex`, `tuple`, etc.), are considered secure and permitted to be unpickled/loaded. Other packages that the user may want to serialize/deserialize may be assigned as secure per-process by the user in-code (see methods below). It is worth noting that PyTorch developers have implemented a similar strategy with the [torch.serialization.add_safe_globals()](https://docs.pytorch.org/docs/stable/notes/serialization.html#torch.serialization.add_safe_globals) method.

Another aim of this PR is to implement an optional Hash-based Message Authentication Code (HMAC) key in the `Pose.cache` dictionary for data integrity verification. While not a security feature, this new API allows the user to set a HMAC key to be prepended to every score value in the `Pose.cache` dictionary that effectively says "this was saved by PyRosetta", so  that it intentionally raises an error when the HMAC key is missing or differs upon retrieval, indicating that the data appears to have been tampered with or modified. By default, the HMAC key is disabled (being set to `None`) in order to reduce memory overhead of the `Pose.cache` dictionary; e.g., if 32 bytes are prepended to each score value, with 1,000 score values that's 32,000 bytes or 32 KB of overhead, and with a million score values that's 32 MB of overhead.

The following are newly added functions:
- `pyrosetta.secure_unpickle.add_secure_package`: Add a package to the unpickle allowed list
- `pyrosetta.secure_unpickle.remove_secure_package`: Remove a package from the unpickle allowed list
- `pyrosetta.secure_unpickle.clear_secure_packages`: Remove all packages from the unpickle allowed list
- `pyrosetta.secure_unpickle.get_disallowed_packages`: Return all permanently disallowed packages/modules/prefixes
- `pyrosetta.secure_unpickle.get_secure_packages`: Return all packages in the unpickle allowed list
- `pyrosetta.secure_unpickle.set_secure_packages`: Set all packages in the unpickle allowed list
- `pyrosetta.secure_unpickle.set_unpickle_hmac_key`: Set the HMAC key for the `Pose.cache` dictionary
- `pyrosetta.secure_unpickle.get_unpickle_hmac_key`: Return the HMAC key for the `Pose.cache` dictionary

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd237bff7744`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.