Exposing your commit email is easy; rewriting Git history is hard.

But there's a set-and-forget solution to ensure your Git privacy.

# The Core Principles

1. **Private Commit Emails.** Never commit with your personal or work email again! Both GitHub and GitLab provide automatic, unique no-reply commit email addresses that hide your identity while still correctly attributing contributions to your profile:
   * https://docs.github.com/en/account-and-profile/how-tos/email-preferences/setting-your-commit-email-address
   * https://docs.gitlab.com/user/profile/#use-an-automatically-generated-private-commit-email
2. **Privacy Guardrail.** Set `useConfigOnly = true` in your Git configuration to prevent falling back to your system username/hostname (e.g., `user@laptop.local`). If no email is set in the config, the commit will simply fail, prompting you to fix it.
3. **Automatic Switching.** Use the conditional `[includeIf]` block with `**/*hostname.com/**` as a powerful glob pattern to match both HTTPS (`https://`) and SSH (`git@`) remote URLs for the respective hosts. This forces Git to use the correct no-reply email based purely on the repository's remote URL.

# Final Config Files

You'll need the following configuration files. Replace all `PLACE_HOLDER` values with your actual information.

<!-- _The most up-to-date config version is now here: https://github.com/anton-staroverov/git-commit-email-privacy_ -->

## `.gitconfig` (Global Git Configuration)

<!-- GITCONFIG_EXAMPLE_START -->

```ini
# ====================================================================
# Global Git Configuration
#
# To use this example:
# 1. Save this file as ~/.gitconfig (most common location)
# 2. Replace all PLACE_HOLDER values (e.g., YOUR_FULL_NAME)
# 3. Repeat for .gitconfig-github and .gitconfig-gitlab as necessary
# ====================================================================

[user]
    # Set your default name for all commits.
    name = YOUR_FULL_NAME

    # CRITICAL: Prevents accidental exposure of system email if no
    # specific email is found in the conditional blocks below.
    useConfigOnly = true

# --------------------------------------------------------------------
# CONDITIONAL OVERRIDES
# These allow you to use different `user.email` based on the URL of
# the repository (e.g., work vs. personal, or GitHub vs. GitLab, etc.)
# --------------------------------------------------------------------

[includeIf "hasconfig:remote.*.url:**/*github.com/**"]
    path = .gitconfig-github

[includeIf "hasconfig:remote.*.url:**/*gitlab.com/**"]
    path = .gitconfig-gitlab
```

<!-- GITCONFIG_EXAMPLE_END -->

## `.gitconfig-github` (GitHub-Specific Configuration)

<!-- GITCONFIG_GITHUB_START -->

```ini
# ====================================================================
# GitHub-specific Git configuration
#
# To use this example:
# 1. Get your unique GitHub commit email: https://docs.github.com/en/account-and-profile/how-tos/email-preferences/setting-your-commit-email-address
# 2. Copy this file next to your `~/.gitconfig` and replace email below
# ====================================================================

[user]
    email = YOUR_GITHUB_ID+USERNAME@users.noreply.github.com
```

<!-- GITCONFIG_GITHUB_END -->

## `.gitconfig-gitlab` (GitLab-Specific Configuration)

<!-- GITCONFIG_GITLAB_START -->

```ini
# ====================================================================
# GitLab-specific Git configuration
#
# To use this example:
# 1. Get your unique GitLab commit email: https://docs.gitlab.com/user/profile/#use-an-automatically-generated-private-commit-email
# 2. Copy this file next to your `~/.gitconfig` and replace email below
# ====================================================================

[user]
    email = YOUR_GITLAB_ID-USERNAME@users.noreply.gitlab.com
```

<!-- GITCONFIG_GITLAB_END -->

# How to Verify

1. Clone a repository from GitHub/GitLab.
2. Run `git config user.email`. It will show your respective GitHub/GitLab no-reply email.

This simple solution ensures your privacy is protected and your commits are correctly attributed, regardless of which hosting platform you're working on.

Shouldn't this be the default configuration for every developer?

# Links

https://www.reddit.com/r/git/comments/1pf94nt/how_to_avoid_exposing_your_commit_email_private/

https://www.reddit.com/r/gitlab/comments/1pedw0w/setandforget_git_privacy_in_5_minutes_autoswitch/

# Credits

https://stackoverflow.com/a/74012889/5034198

---

_**UPD:** had to split the `.gitconfig` into multiple files to avoid issues with `[includeIf]`, as explained in https://stackoverflow.com/a/74012889/5034198_

_**UPD#2:** published https://github.com/anton-staroverov/git-commit-email-privacy_
