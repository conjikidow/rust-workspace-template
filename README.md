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

## Project Setup

Update your workspace metadata in `Cargo.toml`:

```toml
[workspace.package]
version = "0.1.0"
edition = "2024"
readme = "README.md"
license = "MIT OR Apache-2.0"  # Replace with your license
repository = "https://github.com/conjikidow/repo"  # Replace with your repository URL
```

## Pre-commit Hooks Setup

To enable pre-commit hooks in your repository, you need to install `pre-commit` by running the following command:

```console
uvx pre-commit install
```
