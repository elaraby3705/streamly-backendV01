# 🤝 Contributing to Streamly Backend

We welcome and appreciate all contributions to the **Streamly Backend** project! This document outlines the guidelines, rules, and best practices for submitting code, reporting bugs, and suggesting enhancements.

By participating in this project, you are expected to uphold a professional and constructive demeanor.

## 🌟 Getting Started

### 1. Set Up Your Local Environment

To ensure your contributions are testable and consistent, please follow the local setup instructions provided in the main `README.md`.

**Key Requirements:**
* **Docker** and **Docker Compose** must be running.
* You need to configure a `.env` file with necessary credentials (Stripe/Mux/DB) for feature testing.

### 2. Find a Task

All work is tracked and managed using **GitHub Issues** and **Milestones**.

* **Before starting:** Always **comment on the issue** you plan to work on to inform others and prevent duplicate effort.
* **Tags:** Look for issues tagged with `good first issue` (for new contributors), `bug` (for fixes), or `enhancement` (for new features).

## 💻 Contribution Flow (Pull Requests)

All code contributions should be submitted via a **Pull Request (PR)** to the `main` branch.

### 1. Fork, Clone, and Branch

1.  **Fork** the repository to your own GitHub account.
2.  **Clone** your fork locally.
3.  **Create a Branch:** Create a new, descriptive branch for your feature or fix using prefixes like `feat/`, `fix/`, or `docs/`.
    ```bash
    git checkout -b fix/issue-10-payment-bug
    ```

### 2. Code Standards and Testing

* **Style:** Adhere to Python's PEP 8 standards and project-specific formatting (e.g., using `black` if configured).
* **Testing:** **All features and bug fixes must include corresponding unit and/or integration tests** in the `/tests` directory.
* **Run Tests:** Before pushing, ensure your changes haven't introduced regressions:
    ```bash
    # Command to run tests inside your Docker environment
    docker-compose exec app pytest
    ```

### 3. Commit Messages

Commit messages should be clear, concise, and follow the **Conventional Commits** specification. This helps us generate clean changelogs.

* **Format:** `type(scope): subject`
* **Types:**
    * `feat`: A new feature (e.g., `feat(users): implement JWT token refresh`).
    * `fix`: A bug fix (e.g., `fix(webhook): handle missing user in log processing`).
    * `docs`: Documentation only changes.
    * `refactor`: Code change that neither fixes a bug nor adds a feature.
    * `chore`: Routine tasks (e.g., updates to build scripts or dependencies).

### 4. Open a Pull Request (PR)

1.  Push your changes to your fork.
2.  Open a new **Pull Request** from your branch to the `streamly-backend/main` branch.
3.  **In the PR Description:**
    * **Link the Issue:** Use keywords like `Closes #10` to automatically link and close the issue upon merge.
    * **Summary:** Explain the problem solved and the implementation details.
    * **Testing:** Confirm that the local setup and tests have passed.

A project maintainer will review your PR and guide it through the merging process.

## 🐛 Reporting Bugs & ✨ Suggesting Enhancements

If you find a bug or have a suggestion, please use the dedicated templates in the GitHub Issues section.

### Bug Report Template
* **Goal:** Provide enough detail for reproduction.
* **Required Info:** Clear Steps to Reproduce, Expected Behavior, Actual Behavior, and Environment Details.

### Feature Request Template
* **Goal:** Explain the value and necessity of the new feature.
* **Required Info:** The problem being solved, the proposed solution, and examples of how it would be used.