# fm

`fm` is a small, general-purpose file manager built using GTK and [Relm4].

![Screenshot](https://user-images.githubusercontent.com/1372438/164090003-20bca431-e2ef-475a-86d4-df64d10e1989.png)

The directories are visualized using [Miller columns], which enables quick
navigation throughout the hierarchy.

`fm` is in the early stages of development. Manipulating important files with it
risks data loss.

## Platform support

Development is currently focused on Linux, but bug reports for other platforms
are welcome.

The application is known to run successfully on MacOS. Other platforms are
untested, but if you can get the system dependencies to build, `fm` should work.

# cicd_playground

## Setting up GitHub Actions for your cross-platform Rust application to build and test the application on multiple operating systems.

1. Create a new directory in your repository:
   In the root of your repository, create a new directory structure: `.github/workflows/`

2. Create a new YAML file:
   In the `.github/workflows/` directory, create a new file named `ci.yml`. This file will contain your workflow configuration.

3. Configure your workflow:
   Here's a basic GitHub Actions workflow for a Rust project:

```yaml
name: Rust CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

env:
  CARGO_TERM_COLOR: always

jobs:
  build:
    name: Build and test
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macOS-latest]
        rust: [stable]

    steps:
    - uses: actions/checkout@v2
    - name: Install Rust
      uses: actions-rs/toolchain@v1
      with:
        profile: minimal
        toolchain: ${{ matrix.rust }}
        override: true
        components: rustfmt, clippy

    - name: Build
      run: cargo build --verbose

    - name: Run tests
      run: cargo test --verbose

    - name: Check formatting
      run: cargo fmt -- --check

    - name: Run clippy
      run: cargo clippy -- -D warnings
```

### Now, let's break down this configuration:

The workflow is triggered on pushes and pull requests to the main branch.
It uses a matrix strategy to run the job on Ubuntu, Windows, and macOS.
The job performs these steps:

- Checks out your code
- Installs Rust
- Builds your project
- Runs tests
- Checks code formatting
- Runs the Clippy linter

Commit and push:
Add this file to your repository, commit, and push to GitHub.

View the results:

1. Go to your GitHub repository
2. Click on the "Actions" tab
3. You should see your workflow running (or completed if it's fast)

Customize as needed:
You might want to add additional steps such as:

- Creating release artifacts
- Deploying to staging/production
- Running additional tests or checks

Some additional tips:

- read: https://blog.urth.org/2023/03/05/cross-compiling-rust-projects-in-github-actions/
- read: https://dzfrias.dev/blog/deploy-rust-cross-platform-github-actions/
- read: https://jondot.medium.com/building-rust-on-multiple-platforms-using-github-6f3e6f8b8458
- read: https://reemus.dev/tldr/rust-cross-compilation-github-actions
- You can use conditional steps if you need different behavior on different platforms.
- For faster builds, consider caching dependencies. GitHub Actions provides a cache action for this purpose.
- If you have longer-running tests, you might want to separate them into a different job or workflow.
