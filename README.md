# tinyLL -- a shared library for creating and applying the Landlock ruleset

**tinyLL** is a library for GNU/Linux systems that provides functions for creating and applying Landlock ruleset. [Landlock] is a kernel security feature first introduced in 5.13 kernel that allows an unprivileged process to restrict its access to the file system.

## Building

The library can be built with GCC using the following command:

``gcc libtinyll.c -c -fpic -o libtinyll.a``

This file can be used for static linking. For dynamic linking, a shared object has to be created:

``gcc libtinyll.a -shared -o libtinyll.so``

## Functions

The library provides the following functions:

* **landlock_create_ruleset** -- a wrapper for the "landlock_create_ruleset" system call that can be used for creating a Landlock ruleset or for determining the version of Landlock ABI available. Returns the ruleset FD or the version of ABI. More information can be found in the [Landlock manpages](https://github.com/landlock-lsm/man-pages/blob/landlock-v4/man2/landlock_create_ruleset.2)
* **landlock_add_rule** -- a wrapper for the "landlock_add_rule" that can be used to add a rule to the Landlock ruleset. More information can be found in the [Landlock documentation](https://github.com/landlock-lsm/man-pages/blob/landlock-v4/man2/landlock_add_rule.2)
* **landlock_restrict_self** -- a "smart" wrapper for the "landlock_restrict_self" system call that tries to apply the Landlock ruleset using that system call and in case of success, closes the ruleset file descriptor. An application can apply the ruleset only if it has either CAP_SYS_ADMIN in ints effective capability set, or the "No New Privileges" restriction enabled. More information can be found in the [Landlock documentation](https://github.com/landlock-lsm/man-pages/blob/landlock-v4/man2/landlock_restrict_self.2)
* **create_full_ruleset** -- creates a Landlock ruleset that manages all the permissions that can currently be managed by Landlock, and returns its file descriptor. Takes no arguments.
* **add_read_access_rule** -- takes a ruleset file descriptor and a target path file descriptor and adds a rule to the ruleset that allows read access to the target path and all paths beneath it.
* **add_read_access_rule_by_path** -- takes a ruleset file descriptor and a target path and adds a rule to the ruleset that allows read access to the target path and all paths beneath it.
* **add_write_access_rule** -- takes a ruleset file descriptor, a target path file descriptor and a "restricted" bit value (either 0 or 1) and adds a rule to the ruleset that allows write access to the target path and all paths beneath it. If a "restricted" bit is set to 1, the rule won't allow creating FIFO, Unix-domain sockets and block devices under that paths. If a "restricted" argument is set to a value other than 0 or 1, this function won't succeed.
* **add_write_access_rule_by_path** -- takes a ruleset file descriptor, a target path and a "restricted" bit value (either 0 or 1) and adds a rule to the ruleset that allows write access to the target path and all paths beneath it. If a "restricted" bit is set to 1, the rule won't allow creating FIFO, Unix-domain sockets and block devices under that paths. If a "restricted" argument is set to a value other than 0 or 1, this function won't succeed.
* **add_execute_rule** -- takes a ruleset file descriptor and a target path file descriptor and adds a rule to the ruleset that allows execution of all binaries beneath the target path.
* **add_execute_by_path** -- takes a ruleset file descriptor and a target path and adds a rule to the ruleset that allows execution of all binaries beneath the target path.
* **check_nnp** -- returns 0 if the "No New Privileges" is disabled, 1 if it's enabled and -1 if the "prctl" syscall didn't succeed. Takes no arguments.
* **enable_nnp** -- enables the "No New Privileges" restriction for the process. Takes no arguments.
