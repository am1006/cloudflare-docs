---
pcx_content_type: concept
title: Git integration
---

# Git integration

Cloudflare supports connecting Cloudflare Pages to your GitHub and GitLab repositories to look for new changes to your project.

## Custom branches

Suppose you have a custom Git workflow that uses specific branches to represent your project's production build. In that case, you can specify a custom branch when creating (or managing an existing) project in the Pages dashboard by going to  **Settings** > **Builds & deployments** and clicking the **Configure Production deployments** button. You can change the production branch to any other branch in the dropdown menu under **production branch**.

You can also use [preview deployments](/pages/configuration/preview-deployments/) to preview how the new version of your project looks before merging into `production`. In addition, Pages allows you to configure which of your preview branches are built and deployed by using [branch build controls](/pages/configuration/branch-build-controls/).

To configure this in your Pages project go to **Settings** > **Builds & deployments** > **Configure preview deployment** and select **Custom branches**. Here you can specify branches you wish to include and exclude from automatic deployments in the provided configuration fields. To learn more refer to the [branch build controls](/pages/configuration/branch-build-controls/) documentation.


## Organizational access

You can deploy projects to Cloudflare Pages from your open-source team, company, or side project on both GitHub and GitLab.

### GitHub

When authorizing Cloudflare Pages to access a GitHub account, you can specify access to your individual account or an organization that you belong to on GitHub. In order to be able to add the Cloudflare Pages installation to that organization, your user account must be an owner or have the appropriate role within the organization (that is, the GitHub Apps Manager role). More information on these roles can be seen on [GitHub's documentation](https://docs.github.com/en/organizations/managing-peoples-access-to-your-organization-with-roles/roles-in-an-organization#github-app-managers).

{{<Aside type="warning" header="GitHub security consideration">}}

A GitHub account should only point to one Cloudflare account, however, this is not enforced. If you are setting up Cloudflare with GitHub for your organization, Cloudflare recommends that you limit the scope of the application to only the repositories you intend to build with Pages at the time that you set up your project. You can modify these permissions as you build more applications.

{{</Aside>}}

### GitLab

By authorizing Cloudflare Pages to access your GitLab account, you will automatically allow access to organizations, groups, and namespaces your GitLab account can access for use by Cloudflare Pages. Managing access to these organizations and groups is handled by GitLab.

## Removing access to your GitHub account

You can remove Cloudflare Pages' access to your GitHub account by viewing the [**Applications** page](https://github.com/settings/installations) on GitHub. Note that removing access to GitHub will also disable new builds, though the last build of your site will continue to be hosted via Cloudflare Pages.

## Removing access to your GitLab account

You can remove Cloudflare Pages' access to your GitLab account by navigating to **User Settings** > **Applications** > **Authorized Applications**. Find the applications called Cloudflare Pages and select the **Revoke** button to revoke access.

## Skipping a specific build via a commit message

Without any configuration required, you can choose to skip a deployment on an adhoc basis. By adding the `[CI Skip]`, `[CI-Skip]`, `[Skip CI]`, `[Skip-CI]` or `[CF-Pages-Skip]` flag as a prefix in your commit message, and Pages will omit that deployment. The prefixes are case insensitive.

## Reinstall a Git installation

When encountering Git integration related issues, one potential troubleshooting step is attempting to uninstall and reinstall the GitHub or GitLab application associated with the Cloudflare Pages installation. The process for each Git provider is provided below.

### GitHub

1. Go to the installation settings page on GitHub:
    1. `https://github.com/settings/installations` for individual accounts.
    1. `https://github.com/organizations/<YOUR_ORGANIZATION_NAME>/settings/installations` for organizational accounts.
2. If the Cloudflare Pages installation is there, click **Configure**, and click **Uninstall "Cloudflare Pages"** (if there is no "Cloudflare Pages" installation, the user doesn't need to do anything)
3. Go back to the **Workers & Pages** overview page at `https://dash.cloudflare.com/[YOUR_ACCOUNT_ID]/workers-and-pages`. Click **Create application** > **Pages** > **Connect to Git**
4. Click the **+ Add account** button, click the GitHub account you want to add, and then click **Install & Authorize**.
5. You should be redirected to the create project page with your GitHub account or organization in the account list.
6. Attempt to make a new deployment with your project which was previously broken.

### GitLab

1. Go to your application settings page on GitLab located here: https://gitlab.com/-/profile/applications
2. Click the "Revoke" button on your Cloudflare Pages installation if it exists.
3. Go back to the **Workers & Pages** overview page at `https://dash.cloudflare.com/[YOUR_ACCOUNT_ID]/workers-and-pages`. Click **Create application** > **Pages** > **Connect to Git**
4. Select the **GitLab** tab at the top, click the **+ Add account** button, click the GitLab account you want to add, and then click **Authorize** on the modal titled "Authorize Cloudflare Pages to use your account?".
5. You should be redirected to the create project page with your GitHub account or organization in the account list.
6. Attempt to make a new deployment with your project which was previously broken.

## Troubleshooting

### Project Creation

#### `This repository is being used for a Cloudflare Pages project on a different Cloudflare account.`

Using the same Github/Gitlab repository across separate Cloudflare accounts is disallowed. To use the repository for a Pages project in that Cloudflare account, you should delete any Pages projects using the repository in other Cloudflare accounts.

### Deployments

If you run into any issues related to deployments or failing, check your project dashboard to see if there are any SCM installation warnings listed as shown in the screenshot below.

![Pausing a deployment in the Settings of your Pages project](/images/pages/platform/git.dashboard-error.png)

To resolve any errors displayed in the Cloudflare Pages dashboard, follow the steps listed below.

#### `This project is disconnected from your Git account, this may cause deployments to fail.`

To resolve this issue, follow the steps provided above in the [Reinstalling a Git installation section](/pages/configuration/git-integration/#reinstall-a-git-installation) for the applicable SCM provider. If the issue persists even after uninstalling and reinstalling, contact support.

#### `Cloudflare Pages is not properly installed on your Git account, this may cause deployments to fail.`

To resolve this issue, follow the steps provided above in the [Reinstalling a Git installation section](/pages/configuration/git-integration/#reinstall-a-git-installation) for the applicable SCM provider. If the issue persists even after uninstalling and reinstalling, contact support.

#### `The Cloudflare Pages installation has been suspended, this may cause deployments to fail.`

Go to your GitHub installation settings:

* `https://github.com/settings/installations` for individual accounts
* `https://github.com/organizations/<YOUR_ORGANIZATION_NAME>/settings/installations` for organizational accounts

Click **Configure** on the Cloudflare Pages application. Scroll down to the bottom of the page and click **Unsuspend** to allow Cloudflare Pages to make future deployments.

#### `The project is linked to a repository that no longer exists, this may cause deployments to fail.`

You may have deleted or transferred the repository associated with this Cloudflare Pages project. For a deleted repository, you will need to create a new Cloudflare Pages project with a repository that has not been deleted. For a transferred repository, you can either transfer the repository back to the original Git account or you will need to create a new Cloudflare Pages project with the transferred repository.

#### `The repository cannot be accessed, this may cause deployments to fail.`

You may have excluded this repository from your installation's repository access settings.  Go to your GitHub installation settings:

* `https://github.com/settings/installations` for individual accounts
* `https://github.com/organizations/<YOUR_ORGANIZATION_NAME>/settings/installations` for organizational accounts

Click **Configure** on the Cloudflare Pages application. Under **Repository access**, ensure that the repository associated with your Cloudflare Pages project is included in the list.

#### `There is an internal issue with your Cloudflare Pages Git installation.`

This is an internal error in the Cloudflare Pages SCM system. You can attempt to [reinstall your Git installation](/pages/configuration/git-integration/#reinstall-a-git-installation), but if the issue persists, [contact support](/support/contacting-cloudflare-support/).

## Related resources

* [Branch build controls](/pages/configuration/branch-build-controls/#production-branch-control) -  Control which environments and branches you would like to automatically deploy to.