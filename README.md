Exposing your commit email is easy; rewriting Git history is hard.

But there's a set-and-forget solution to ensure your Git privacy.

# The Core Principles

1. **Private Commit Emails.** Never commit with your personal or work email again! Both GitHub and GitLab provide automatic, unique no-reply commit email addresses that hide your identity while still correctly attributing contributions to your profile:
   * [https://docs.github.com/en/account-and-profile/how-tos/email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-on-github](https://docs.github.com/en/account-and-profile/how-tos/email-preferences/setting-your-commit-email-address#setting-your-commit-email-address-on-github)
   * [https://docs.gitlab.com/user/profile/#use-an-automatically-generated-private-commit-email](https://docs.gitlab.com/user/profile/#use-an-automatically-generated-private-commit-email)
2. **Privacy Guardrail.** Set `useconfigonly = true` in your Git configuration to prevent falling back to your system username/hostname (e.g., `user@laptop.local`). If no email is set in the config, the commit will simply fail, prompting you to fix it.
3. **Automatic Switching.** Use the conditional `[includeIf]` block with `**/*hostname.com/**` as a powerful glob pattern to match both HTTPS (`https://`) and SSH (`git@`) remote URLs for the respective hosts. This forces Git to use the correct no-reply email based purely on the repository's remote URL.

# Final Config

Copy and paste this content into your single global Git configuration file `~/.gitconfig`, **and don't forget replacing the PLACE\_HOLDERS:**

    # =====================================================================
    # Global Git Config
    # =====================================================================
    [user]
        name = YOUR_FULL_NAME
        
        # CRITICAL: Prevents accidental system email exposure if no
        # specific email is found in the conditional blocks below.
        useconfigonly = true
    
    # =====================================================================
    # Conditional Emails
    # =====================================================================
    [includeIf "hasconfig:remote.*.url:**/*github.com/**"]
        [user]
            email = YOUR_GITHUB_ID+USERNAME@users.noreply.github.com
    # =====================================================================
    [includeIf "hasconfig:remote.*.url:**/*gitlab.com/**"]
        [user]
            email = YOUR_GITLAB_ID-USERNAME@users.noreply.gitlab.com

# How to Verify

1. Clone a repository from GitHub/GitLab.
2. Run `git config user.email`. It will show your respective GitHub/GitLab no-reply email.

This simple, single-file solution ensures your privacy is protected and your commits are correctly attributed, regardless of which hosting platform you're working on.

Shouldn't this be the default configuration for every developer?

# Links

https://www.reddit.com/r/git/comments/1pf94nt/how_to_avoid_exposing_your_commit_email_private/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button

https://www.reddit.com/r/gitlab/comments/1pedw0w/setandforget_git_privacy_in_5_minutes_autoswitch/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button
