# rust-repository-template

This is a template repository for Rust workspace.

## GitHub Actions Permissions Setup

To enable GitHub Actions to run properly in your repository, you need to adjust the default permissions granted to the `GITHUB_TOKEN`. This is especially important for **private repositories**.

Follow these steps to configure the permissions:

1. Go to the **Settings** tab of your repository.
2. On the left-hand menu, select **Actions/General**.
3. Under the **Workflow permissions** section, ensure the following options are selected:
   - **`Read and write permissions`**: This grants read and write access to the repository for all scopes.
   - **`Allow GitHub Actions to create and approve pull requests`**: This allows GitHub Actions to create pull requests.
4. Save the changes.

![GitHub Actions permissions setup screenshot](https://github.com/user-attachments/assets/da55e896-e087-486e-aadc-7fc1283dc652)

These settings are **necessary only for private repositories**. For public repositories, this configuration is not required.

## Pre-commit Hooks Setup

To enable pre-commit hooks in your repository, you need to install `pre-commit` by running the following command:

```bash
uvx pre-commit install
```

## Workspace Setup

Update your workspace metadata in `Cargo.toml`:

```toml
[workspace.package]
version = "0.1.0"
edition = "2024"
readme = "README.md"
license = "MIT OR Apache-2.0"  # Replace with your license
repository = "https://github.com/conjikidow/repo"  # Replace with your repository URL
```

Add packages to the workspace by:

```bash
cargo new --vcs none crates/<package-name>
```

By default this template is binary-first; add `--lib` if you need a library crate.

## Release Management Setup

To set up release management using `release-plz` and `dist`, follow these steps:

- Common setup
  1. Set `release = true` in the `[workspace]` section of `.release-plz.toml` to enable releases.
  2. Use a GitHub App to mint the token for `release-plz`, and store the App ID and private key as repository secrets named `APP_ID` and `APP_PRIVATE_KEY` (see [release-plz docs](https://release-plz.dev/docs/github/token#use-a-github-app)).

- Binary publishing (default; uses `dist`)
  1. `dist` is already configured (`dist-workspace.toml`).
  2. If you need to change the settings, install `dist` and run `dist init` and follow the prompts.

     ```bash
     cargo binstall cargo-dist
     dist init
     ```

- Library-only publishing
  1. Remove `dist-workspace.toml` and `.github/workflows/release.yml`, and delete the `[profile.dist]` section from `Cargo.toml`.
  2. Let `release-plz` create the GitHub Release by setting `git_release_enable = true`.

For more details, see [release-plz](https://release-plz.ieni.dev/docs) and [dist](https://axodotdev.github.io/cargo-dist/book).

### Trusted publishing (crates.io OIDC)

Enable trusted publishing on crates.io.
New crates cannot be published via OIDC the first time—run the first `cargo publish` manually.
