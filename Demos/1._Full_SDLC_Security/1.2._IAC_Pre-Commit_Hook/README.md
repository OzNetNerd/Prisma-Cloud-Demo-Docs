# 1.2 IAC Pre-Commit Hook
## Capabilities

This demo covers the following capabilities:

1. `tfplan` scanning

2. Automation using **pre-commit**

## Why are they important?

While [Terraform Config](../1.1._IAC_IDE_Integration/) scanning can identify a large number of cloud misconfigurations, it has one major weakness - it cannot identify risks that rely on dynamic input. For example, a user passing in an API key as a variable.

Prisma Cloud eliminates this weakness by scanning **both** the Terraform config, **and** the `tfplan`. This maximises productivity and security because:

1. As we saw in the previous demo, the former provides immediate feedback and one click fixes

2. The latter scans the final version of the config. Crucially, this includes the real values that are passed in as dyanmic inputs

In this demo, we're going to close the gap on our Terraform scanning by triggering `tfplan` scans automatically with a **pre-commit hook**. As we will see in a moment, this is a very effective way to prevent accidental secret & password leaks.

## Secnario: Uncovering Hidden Risks

We flicked through our [Terraform Config misconfigurations](../1.1._IAC_IDE_Integration/) in the previous demo. While we'll need to rectify these issues before we push the code to prod, we're operating within the business' security policies. They state that:

* For **Dev** environments:

    * We must ensure **no secrets** are committed to version control

* For **Prod** environments:

  * We must ensure **no secrets** are committed to version control

  * We must fix all `HIGH` **and** `CRITICAL` misconfigurations and vulnerabilities

To increase our productivity, we'll automate these checks. To do this, we'll trigger Checkov's `tfplan` scan through a **pre-commit hook**. This will automatically and immediately prevent us from accidentally uncompliant code.

#### Steps

1. Because we only want to see the results locally at this stage, we temporarily clear our Checkov API environment variable:

```
BC_API_KEY=
```

2. Go to the **"banking-app"** directory:

```
cd $DEMO_USER_DIR/banking-app/
```

3. Review the contents of the `.pre-commit-config.yaml` file. This code will run each time we try to make a commit. (See [this page](https://www.checkov.io/2.Basics/CLI%20Command%20Reference.html) for all possible options.)

```
cat .pre-commit-config.yaml
```

4. Install the pre-commit hook:

```
pre-commit install
```

**Note:** We can view the script pre-commit generated here:

```
cat .git/hooks/pre-commit
```

5. Now that we've got our automated checks in place, let's prepare our commit:

```
git add .
git status
```

**Note:** The `git status` command shows us which files are going to be part of this commit.

6. All looks good! Let's go ahead and commit our change.

```
git commit -am 'Our initial Dev app deployment with all secrets removed'
```

7. ooops! As per Checkov's output, it turns out we do in fact have secrets stored in our `variables.tf` file.

Thanks to our pre-commit hook, we not only know where the secrets are, but it prevented us from committing the secrets into our git history. We can confirm this by re-issuing the following command:

```
git status
```

As we can see in the output of the command, we still have **"No commits yet"** and all of our files are **Changes to be committed**.


8. Remove the `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` lines from the `$DEMO_USER_DIR/banking-app/infra/variables.tf` file.

9. Let's now add our updated `variables.tf` and see how our commit goes this time:

```
git commit -am 'Our initial Dev app deployment with all secrets actually removed this time!'
```

10. As a final check to ensure that we actually did avoid committing our secrets to the git history, we issue the following command:

```
git log --oneline
```

We see that our history only has the commit that we made after the secrets were removed. Success!