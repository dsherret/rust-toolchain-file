# Install Rust Toolchain via rust-toolchain.toml

GitHub Action to install Rust's toolchain via the
[`rust-toolchain.toml`](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file)
file in your repository.

Fork of https://github.com/dtolnay/rust-toolchain that supports and makes it
mandatory to use a rust-toolchain.toml file. Unfortunately `rust-toolchain` did
not want to support installing from a rust-toolchain.toml file, so this project
exists. If you do not want to use a rust-toolchain.toml file, then please use
that project as this action does not support other inputs.

## Example workflow

1. Create a
   [`rust-toolchain.toml`](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file)
   file in the root directory of your repository:

   ```toml
   [toolchain]
   channel = "1.68"
   components = [ "rustfmt", "clippy" ]
   ```

2. Add an entry to this action in your GitHub Actions workflow file (ensure the
   repo is checked out first):

   ```yaml
   name: test suite
   on: [push, pull_request]

   jobs:
     test:
       name: cargo test
       runs-on: ubuntu-latest
       steps:
         - uses: actions/checkout@v3
         - uses: dsherret/rust-toolchain-file@v1
         - run: cargo test --all-features
   ```

The selection of Rust toolchain on the CI will then be made based on the
rust-toolchain.toml file enabling you to easily keep it in sync with your local
development versions and have a single source of truth for what version to use.

## Inputs

You must define everything in the rust-toolchain.toml file.

## Outputs

<table>
<tr>
  <th>Name</th>
  <th>Description</th>
</tr>
<tr>
  <td><code>cachekey</code></td>
  <td>A short hash of the installed rustc version, appropriate for use as a cache key. <code>"20220627a831"</code></td>
</tr>
<tr>
  <td><code>name</code></td>
  <td>Rustup's name for the selected version of the toolchain, like <code>"1.62.0"</code>. Suitable for use with <code>cargo +${{steps.toolchain.outputs.name}}</code>.</td>
</tr>
</table>

## License

The scripts and documentation in this project are released under the
[MIT License].

[MIT License]: LICENSE
