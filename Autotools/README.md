### GNU Autotools: Developer Order of Operations (Bootstrapping)

The tools must be run in a specific sequence, because each step generates files required by the next.

The autotools process is functional in nature. 
Every tool has a single responsibility, task, or purpose...
So, in order to build the program of a source repository - you'll have to run them in order.

**autoscan**

    -   **Action:** Scans source code and includes to identify which libraries and headers  your code uses.
    -   **Output:** Creates `configure.scan`  a "draft" version of `configure.ac`.
 
Rename to `configure.ac`, then manually fix errors.

**aclocal**

    -   **Input:** `configure.ac`
    -   **Output:** `aclocal.m4`
    -   **Action:** Scans `configure.ac` for third-party macros  (Automake or Libtool) gathers - definitions into `aclocal.m4`

 so `autoconf` can process them.

**autoheader**

    -   **Input:** `configure.ac`
    -   **Output:** `config.h.in`
    -   **Action:** Creates a template file for C preprocessor macros. When the user runs `./configure`, this template is transformed into a real `config.h`.

 **autoconf**
 
    -   **Input:** `configure.ac`, `aclocal.m4`
    -   **Output:** `configure` (the shell script)
    -   **Action:** Converts the M4 macros in your configuration file into the final, portable `configure` shell script that the end-user executes.

**automake**

    -   **Input:** `Makefile.am`, `configure.ac`
    -   **Output:** `Makefile.in`
    -   **Action:** Translates your high-level `Makefile.am` into a `Makefile.in` template. The user's `./configure` script will later turn this into the final `Makefile`.

---


**autoreconf -i** → Runs the standard loop (`aclocal`, `autoconf`, `autoheader`, `automake`).

####  Misc/Supplementary 
These tools are typically used outside the standard repetitive build loop to help with project creation or major updates.

1.  **autopoint** → (Optional) Add translation support.
2.  **libtoolize** → (Optional) Add shared library support.


3.  **autoupdate** (The Version Upgrader)
    -   **When to use:** When you upgrade your system's Autotools version and want to modernize an old project.
    -   **Action:** Reads your existing `configure.ac` and replaces obsolete M4 macros with their modern equivalents. 
    -   **Benefit:** Keeps your configuration files readable and compatible with the latest GNU standards without manual searching.

4.  **autopoint** (The Translation Helper)
    -   **When to use:** When your project uses `gettext` for internationalization (translations).
    -   **Action:** Copies the necessary `gettext` infrastructure files into your project directory. 
    -   **Placement:** It is usually run before `autoreconf` or as part of the `autoreconf --install` process if `AM_GNU_GETTEXT` is detected in your configuration.

5.  **libtoolize** (Optional)
    -   **Action:** If your project builds libraries, this adds the necessary infrastructure to handle cross-platform shared library creation.

AC_ (Autoconf): Core macros for system checks, compiler detection, and basic shell script generation (e.g., AC_INIT, AC_CHECK_HEADERS).

AM_ (Automake): Macros specifically for projects using automake to generate Makefiles (e.g., AM_INIT_AUTOMAKE, AM_PROG_AS).

m4_ (M4sugar/M4): Low-level internal M4 macros that handle string manipulation, loops, and logic inside the processing phase.

LT_ (Libtool): Macros used for managing shared libraries (e.g., LT_INIT).

PKG_ (Pkg-config): Macros used to find libraries using the pkg-config tool (e.g., PKG_CHECK_MODULES).
