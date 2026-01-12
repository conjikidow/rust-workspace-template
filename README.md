# rust-workspace-template

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

## Workspace Setup

Update your workspace metadata in `Cargo.toml`:

```toml
[workspace.package]
version = "0.1.0"
edition = "2024"
readme = "README.md"
license = "Apache-2.0 OR MIT"  # Replace with your license
repository = "https://github.com/conjikidow/repo"  # Replace with your repository URL
```

This template repository includes [`LICENSE-APACHE`](LICENSE-APACHE) and [`LICENSE-MIT`](LICENSE-MIT) by default.
They are provided as a template and may be modified, replaced, or removed as needed.

Add packages to the workspace by:

```bash
cargo new --vcs none crates/<package-name>
```

By default this template is binary-first; add `--lib` if you need a library crate.

## Release Management Setup

### Overview

This template uses [release-plz](https://release-plz.dev) to automate releases based on Conventional Commits.
It opens a release PR and, once merged, publishes the crate and (optionally) creates a GitHub Release.

Rust workspaces can be either:

- **Binary workspace** (default here): includes at least one binary crate. Publishing binaries is done with [dist](https://axodotdev.github.io/cargo-dist/book), which builds per-platform binaries and uploads them to GitHub Releases.
- **Library-only workspace**: no binaries. In this case, remove the `dist` configuration and let `release-plz` handle releases only.

### GitHub App Setup (recommended)

Release automation workflows in this template need a GitHub token.
You can use a Personal Access Token, but a GitHub App is the recommended option.
The App becomes the author of automation PRs (e.g. `automation-bot[bot]`).

Create a minimal GitHub App:

1. Create the App:
   - Personal account: <https://github.com/settings/apps/new>
   - Organization: `https://github.com/organizations/<org>/settings/apps/new`
2. **GitHub App name**: any name you like (e.g. `automation-bot`).
3. **Homepage URL**: any URL (your GitHub profile is fine).
4. **Webhook**: disable it (no URL required).
5. **Repository permissions**:
   - `Contents`: Read & write
   - `Pull requests`: Read & write
   - If you use protected tags: `Administration`: Read & write
   - If you publish binaries with `dist`: `Workflows`: Read & write
6. (Optional) Limit installation scope to your account only.
7. Create a **private key** from the App settings page and store it securely.

Install the App on the repository where you will run releases.

Store the credentials as GitHub Actions secrets:

- `APP_ID`: the GitHub App ID
- `APP_PRIVATE_KEY`: the private key content

You can find the secrets page at:
`https://github.com/<owner>/<repo>/settings/secrets/actions`

### Setup for Binary Workspace (uses dist)

1. Ensure `.release-plz.toml` has `release = true` under `[workspace]` (this template sets it to `false` by default).
2. `dist` is already configured via `dist-workspace.toml`.
   If you need to reconfigure, install `dist` and run `dist init`:

   ```bash
   cargo binstall cargo-dist
   dist init
   ```

### Setup for Library-only Workspace

1. Ensure `.release-plz.toml` has `release = true` under `[workspace]` (this template sets it to `false` by default).
2. Remove `dist-workspace.toml`, `.github/workflows/release.yml`, and `.github/workflows/dist-generate.yml`.
3. Delete the `[profile.dist]` section from `Cargo.toml`.
4. In `.release-plz.toml`, set `git_release_enable = true` so `release-plz` creates GitHub Releases.

### Trusted Publishing (crates.io OIDC)

Enable trusted publishing on crates.io.
New crates cannot be published via OIDC the first time—run the first `cargo publish` manually.

### References

For more details, see [release-plz](https://release-plz.ieni.dev/docs) and [dist](https://axodotdev.github.io/cargo-dist/book).

## Pre-commit Hooks Setup

To enable pre-commit hooks in your repository, you need to install `pre-commit` by running the following command:

```bash
uvx pre-commit install --hook-type pre-commit --hook-type commit-msg --hook-type pre-push
```

## Commit Message Linting with Commitizen

This template repository enforces [Conventional Commits](https://www.conventionalcommits.org) in pre-commit hooks and CI, so your commit messages must follow that format.

You can maintain Conventional Commits manually, but automation tools such as Commitizen or git-cz can help.
Any tool is fine, but this repository uses [commitizen-tools/commitizen](https://github.com/commitizen-tools/commitizen) for checks, so it is recommended.

Install Commitizen:

```bash
uv tool install commitizen
```

Use Commitizen instead of `git commit`:

```bash
cz commit
```

For more details, see [Commitizen documentation](https://commitizen-tools.github.io/commitizen).

## License

Licensed under either of [Apache License, Version 2.0](LICENSE-APACHE) or [MIT license](LICENSE-MIT) at your option.

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in this software by you, as defined in the Apache-2.0 license, shall be dually licensed as above, without any additional terms or conditions.
