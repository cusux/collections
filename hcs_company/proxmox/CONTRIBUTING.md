# Contributing

We follow [Ansible Guidelines](https://link_to_be_provided) in all our contributions and interactions within this repository.

If you are a committer, also refer to the [Working with Git and GitLab Guidelines](https://link_to_be_provided).

## Issue tracker

Whether you are looking for an opportunity to contribute or you found a bug and already know how to solve it, please go to the [issue tracker](https://github.com/cusux/collections/hcs_company/issues).
There you can find feature ideas to implement, reports about bugs to solve, or submit an issue to discuss your idea before implementing it which can help choose a right direction at the beginning of your work and potentially save a lot of time and effort.
Also somebody may already have started discussing or working on implementing the same or a similar idea,
so you can cooperate to create a better solution together.

## Looking for an idea to implement

If you came up with a new feature, it is always worth creating an issue before starting to write code to discuss the idea with the community first.
If you are going to implement the feature yourself, say it in the issue explicitly to avoid working in parallel with somebody else.

## Step-by-step guide how to get into development quickly

We assume that you use Linux as a work environment (you can use a virtual machine as well) and have `git` installed.

1. If possible, make sure that you have installed `docker`. While you can also run tests without Docker, this makes it a lot easier since you do not have to install the precise requirements, and tests are running properly isolated and in the exact same environments as in CI. You often can also use `podman` with the `docker` executable shim, so if you have that you probably do not need to install `docker`.

2. Clone the [ansible-core](https://github.com/ansible/ansible) repository:

```bash
git clone https://github.com/ansible/ansible.git
```

Instead of installing ansible-core from source, you can also work with an already existing installation of Ansible, ansible-base or ansible-core. Simply skip steps 2 and 3 in that case.

3. Go to the cloned repository and prepare the environment:

```bash
cd ansible && source hacking/env-setup
pip install -r requirements.txt
cd ~
```

4. Create a directory whereas to store development code (e.g.):

```bash
mkdir -p ~/development
```

5. Fork the `https://github.com/cusux/collections/hcs_company/` repository through the GitLab web interface.

6. Clone the forked repository from your profile to the created path:

If you prefer to use the SSH protocol:

```bash
git clone git@github.com:cusux/collections.git ~/development
```

7. Go to your new cloned repository, to the folder holding this Proxmox collection.

```bash
cd ~/development/collections/ansible_collections/hcs_company/proxmox
```

8. Be sure you are in the main branch:

```bash
git status
```

9. Show remotes. There should be the `origin` repository only:

```bash
git remote -v
```

10. Add the `upstream` repository:

```bash
git remote add upstream git@github.com:cusux/collections.git
```

11. Update your local `main` branch:

```bash
git fetch upstream
git rebase upstream/main
```

12. Create a branch for your changes:

```bash
git checkout -b name_of_my_branch
```

13. We recommend you start with writing integration tests if applicable.

Note: If there are any difficulties with writing the tests or you are not sure if the case can be covered, feel free to skip this step.
If needed, other contributors can help you with it later.

All integration tests are stored in `tests/integration/targets` subdirectories.
Go to the subdirectory containing the name of module you are going to change.
For example, if you are fixing the `ceph_volume` module, its tests are in `tests/integration/targets/test_ceph_volume/tasks`.

The `main.yml` file holds test tasks and includes other test files.
Look for a suitable test file to integrate your tests or create and include a dedicated test file.
You can use one of the existing test files as a draft.

When fixing a bug, write a task which reproduces the bug from the issue.

Put the reported case in the tests, then run integration tests with the following command:

```bash
ansible-test integration test_ceph_volume --docker -vvv
```

If the tests do not want to run, first, check you complete step 3 of this guide.

If the tests ran successfully, there are usually two possible outcomes:
a) If the bug has not appeared and the tests have passed successfully, ask the reporter to provide more details. The bug can be not a bug actually or can relate to a particular software version used or specifics of local environment configuration.

b) The bug has appeared and the tests has failed as expected showing the reported symptoms.

14. Fix the bug.

15. Run `flake8` against a changed file. If it is `plugins/modules/ceph_volume.py`:

```bash
flake8 plugins/modules/ceph_volume.py
```

It is worth installing and running `flake8` against the changed file(s) first.
It shows unused imports, which is not shown by sanity tests (see the next step), as well as other common issues.
Optionally, you can use the `--max-line-length=160` command-line argument.

16. Run sanity tests:

```bash
ansible-test sanity plugins/modules/ceph_volume.py --docker
```

If they failed, look at the output carefully - it is usually very informative and helps to identify a problem line quickly.
Sanity failings usually relate to wrong code and documentation formatting.

17. Run integration tests:

```bash
ansible-test integration test_ceph_volume --docker -vvv
```

There are two possible outcomes:
a) They have failed. Look at the output of the command.
Fix the problem place in the code and run again.
Repeat the cycle until the tests pass.

b) They have passed. Remember they failed originally? Our congratulations! You have fixed the bug.

18. Commit your changes with an informative but short commit message:

```bash
git add /path/to/changed/file
git commit -m "ceph_volume: fix crash when ..."
```

19. Push the branch to the `origin` (your fork):

```bash
git push origin name_of_my_branch
```

20. Go to the `upstream` (https://xffgit1t.xfftest.lan/kpn/collections).

21. Go to `Merge requests` tab and create a merge request.

GitLab is tracking your fork, so it should see the new branch in it and automatically offer
to create a merge request. Sometimes GitLab does not do it and you should click the `New merge request` button yourself.
Then choose `compare branches and continue` under the `New merge request` title.
Choose your project and the new branch you pushed in the right drop-down list.
Confirm. Fill out the pull request template with all information you want to mention.
Put "Fixes + link to the issue" in the merge request's description.
Put "[WIP] + short description" in the merge request's title. It's often a good idea to mention the name of the module/plugin you are modifying at the beginning of the description.
Click `Create merge request`.

22. Add a [changelog fragment](https://docs.ansible.com/ansible/devel/community/development_process.html#changelogs) to the `changelog/fragments` directory. It will be published in release notes, so users will know about the fix.

Commit and push it:

```bash
git add changelog/fragments/myfragment.yml
git commit -m "Add changelog fragment"
git push origin name_of_my_branch
```

23. The CI tests will run automatically on Red Hat infrastructure after every commit.

You will see the CI status in the bottom of your pull request.
If they are green, remove "[WIP]" from the title. Mention the issue reporter in a comment and let contributors know that the pull request is "Ready for review".

24. Wait for reviews. You can also ask for review on IRC in the #ansible-community channel.

25. If the pull request looks good to the community, committers will merge it.

For details, refer to the [Ansible developer guide](https://docs.ansible.com/ansible/latest/dev_guide/index.html).

If you find any inconsistencies or places in this document which can be improved, feel free to raise an issue or pull request to fix it.
