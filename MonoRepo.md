#monorepo #software #git #repository #bazel
# What is <font color="#de7802">MonoRepo</font>
A monorepo, short for "<font color="#de7802">mono repository</font>," is a **software development strategy** where the <font color="#de7802">code for multiple projects is stored in a single version-controlled repository</font>.

Monorepos can encourage <u>rapid development workflows and improve cross-team collaboration</u> by providing a<u> consistent and unified codebase</u>.

**When you make a change,** you <u>do not rebuild or retest every project in the monorepo</u>. Instead, you <u>only rebuild and retest the projects that can be affected by your change</u>.

## <font color="#de7802">MonoRepo</font> vs <font color="#ffc000">Monolithic</font>
It is important to distinguish between a monorepo and a monolithic application.

- A monorepo is a <u>single repository containing multiple independent projects which has connection between them</u>
- a monolithic application is a <u>single-tiered application designed as a single service</u>

### <font color="#92cddc">polyrepo</font>
The alternative to a monorepo is a multi-repo (also known as a polyrepo), where <u>each project is stored in a separate version-controlled repository</u>.
Multi-repos are the more common approach and offer team <u>autonomy</u>.
![[Pasted image 20230504213320.png]]
## <font color="#c3d69b">Benfits</font> 
Pros:
-   **Improved cross-team collaboration and code sharing**.
-   Consistent and unified codebase.
-   **Easier large-scale refactoring**.
-   Simplified dependency management.
Cons:
-   Scalability <u>challenges with build tools and version control systems</u>.
-   <u>Increased complexity in managing the repository</u>.
-   Potential for <u>slower build and test times</u> due to the larger codebase.

---
# <font color="#00b050">Bazel</font>
Bazel is a <font color="#00b050">build tool that is used to build and test software projects</font>.It supports a wide range of programming languages.

It uses a build language called <font color="#92d050">Starlark</font>, <font color="#92d050">which is a subset of Python</font>, <u>to define build rules</u>.
These<font color="#00b050"> rules describe how to build and test your software project,</font> and Bazel uses them to <u>generate a build graph</u> that represents the dependencies between your code and the libraries it uses.

### <font color="#92cddc">incremental builds</font>
when you make a change to your code, Bazel <font color="#92cddc">only rebuilds the parts of your project that are affected by that change</font>.

### <font color="#548dd4">Testing</font>
It supports a <font color="#548dd4">wide range of testing frameworks</font>, including JUnit, Pytest, and Karma, and can run tests in parallel to speed up the testing process.

### <font color="#d99694">Quick summery </font>
### <font color="#ffff00">workspace</font>
a workspace is a <font color="#ffff00">directory on your computer that contains all the files and dependencies necessary to build and run your project</font>. When you create a new workspace, Bazel creates a set of configuration files that define <font color="#ffff00">how your project should be built and what external dependencies it requires</font>.

allows you to easily manage dependencies between different parts of your project.

### <font color="#ffc000">BUILD</font>
a BUILD file <font color="#ffc000">is a configuration file that describes how to build one or more targets in your project</font>. A <u>target is typically a binary executable, a library, or a test suite</u>.

The BUILD file <font color="#ffc000">contains a set of rules that define how to build each target</font>, including what source files to compile, what libraries to link against, what compiler flags to use, and more.

allows you to express the dependencies between different parts of your project in a declarative way.
```Starlark
# my_library/BUILD

load("@rules_rust//rust:rust.bzl", "rust_library")

rust_library(
    name = "my_library",
    srcs = ["my_lib.rs"],
    visibility = ["//visibility:public"],
)
```

### <font color="#00b0f0">BUILD.bazel</font>
a BUILD.bazel file<u> is a special type of BUILD file that is used to define <font color="#00b0f0">reusable</font> rules and macros that can be used across multiple BUILD files in your project</u>. This allows you to factor out common build logic into shared modules that can be used by multiple targets.

BUILD.bazel file is <u>typically located at the root of your project</u>, and it can contain any number of rules and macros.
```Starlark
# BUILD.bazel

def_python_library(
    name = "my_library",
    srcs = glob(["**/*.py"]),
    deps = [
        "@python//stdlib",
        "//my_package:my_dependency",
    ],
)
```

```Starlark
# my_package/BUILD

load("//:BUILD.bazel", "def_python_library")

def_python_library(
    name = "my_library",
    srcs = ["my_module.py"],
    deps = [
        "@python//stdlib",
        "//my_package:my_dependency",
    ],
)
```

### <font color="#76923c">load</font>
`load` is a function that is used to <font color="#76923c">load external rules and macros </font>into your BUILD files.
The `load` function takes two arguments: <font color="#76923c">the path to the BUILD</font> file containing the rules and <font color="#76923c">macros you want to load,</font>.
```Starlark
load("//my_library:rules.bzl", "my_rule")
```

### <font color="#953734">Macro</font>
a macro is a function that <font color="#953734">generates one or more build rules or other macros</font>.
They can take parameters and generate different output depending on the inputs.
```Starlark
# my_library/macros.bzl

def my_macro(name, srcs):
    native.cc_library(
        name = name,
        srcs = srcs,
        deps = [
            "@my_library//:my_dependency",
        ],
    )
```