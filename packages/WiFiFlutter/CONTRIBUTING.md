# Contributing to WiFiFlutter

<a href="https://github.com/flutternetwork/WiFiFlutter/actions?query=workflow%3Aall_plugins">
    <img src="https://github.com/flutternetwork/WiFiFlutter/workflows/all_plugins/badge.svg" alt="all_plugins GitHub Workflow Status"/>
</a>


## 1. Things you will need

- Linux, Mac OS X, or Windows.
- [git](https://git-scm.com) (used for source version control).
- An ssh client (used to authenticate with GitHub).
- An IDE such as [Android Studio](https://developer.android.com/studio) or [Visual Studio Code](https://code.visualstudio.com/).
- [Flutter](https://flutter.dev/) with proper [IDE configuration](https://docs.flutter.dev/get-started/editor).
- [`flutter_plugin_tools`](https://pub.dev/packages/flutter_plugin_tools) locally activated.
- [`clang-format`](https://clang.llvm.org/docs/ClangFormat.html) (available via brew on macOS, apt on Ubuntu, maybe via llvm on chocolatey for Windows)

## 2. Forking & cloning the repository

- Ensure all the dependencies described in the previous section are installed.
- Fork `https://github.com/flutternetwork/WiFiFlutter` into your own GitHub account. If
  you already have a fork, and are now installing a development environment on
  a new machine, make sure you've updated your fork so that you don't use stale
  configuration options from long ago.
- If you haven't configured your machine with an SSH key that's known to github, then
  follow [GitHub's directions](https://help.github.com/articles/generating-ssh-keys/)
  to generate an SSH key.
- `git clone git@github.com:<your_name_here>/WiFiFlutter.git`
- `git remote add upstream git@github.com:flutternetwork/WiFiFlutter.git` (So that you
  fetch from the master repository, not your clone, when running `git fetch`
  et al.)

## 3. Environment Setup

WiFiFlutter uses [Melos](https://github.com/invertase/melos) to manage the project and dependencies.

To install Melos, run the following command from your SSH client:

```bash
pub global activate melos
```

Next, at the root of your locally cloned repository bootstrap the projects dependencies:

```bash
melos bootstrap
```

The bootstrap command locally links all dependencies within the project without having to
provide manual [`dependency_overrides`](https://dart.dev/tools/pub/pubspec). This allows all
plugins, examples and tests to build from the local clone project.

> You do not need to run `flutter pub get` once bootstrap has been completed.

## 4. Running an example

Each plugin provides an example app which aims to showcase the main use-cases of each plugin.

To run an example, run the `flutter run` command from the `example` directory of each plugins main
directory. For example, for WiFi For IoT example:

```bash
cd packages/wifi_iot/example
flutter run
```

Using Melos (installed in step 3), any changes made to the plugins locally will also be reflected within all
example applications code automatically.

## 4. Running tests

WiFiFlutter comprises of a number of tests for each plugin, either end-to-end (e2e) or unit tests.

### Unit tests

Unit tests are responsible for ensuring expected behavior whilst developing the plugins Dart code. Unit tests do not
interact with platform, and mock where possible. To run unit tests for a specific plugin, run the
`flutter test` command from the plugins root directory. For example, WiFi For IoT platform interface tests can be run
with the following commands:

```bash
cd packages/wifi_iot
flutter test
```

<!-- TODO enable this when e2e tests aded
### End-to-end (e2e) tests

E2e tests are those which directly communicate with platform APIs, whose results cannot be mocked. These tests run directly from
an example application. To run e2e tests, run the `flutter drive` command from the plugins main `example` directory, targeting the
entry e2e test file.

```bash
cd packages/wifi_iot/example
flutter drive --target=./test_driver/wifi_iot_e2e.dart
```

To run tests against web environments, run the command as a release build:

```bash
cd packages/wifi_iot/example
flutter drive --target=./test_driver/wifi_iot_e2e.dart --release -d chrome
```
-->

### Using Melos

To help aid developer workflow, Melos provides a number of commands to quickly run
tests against plugins. For example, to analyze all plugins at once,
run the following command from the root of your cloned repository:

```bash
melos run analyze
```

A full list of all commands can be found within the [`melos.yaml`](https://github.com/flutternetwork/WiFiFlutter/blob/master/melos.yaml)
file.

## 5. Contributing code

We gladly accept contributions via GitHub pull requests.

Please peruse the
[Flutter style guide](https://github.com/flutter/flutter/wiki/Style-guide-for-Flutter-repo) and
[design principles](https://flutter.io/design-principles/) before
working on anything non-trivial. These guidelines are intended to
keep the code consistent and avoid common pitfalls.

To start working on a patch:

1. `git fetch upstream`
2. `git checkout upstream/master -b <name_of_your_branch>`
3. Hack away!

Once you have made your changes, ensure that it passes the internal analyzer & formatting checks. The following
commands can be run locally to highlight any issues before committing your code:

```bash
# Run the analyze check
melos run analyze

# Format code
melos run format
```

Assuming all is successful, commit and push your code:

1. `git commit -a -m "<your informative commit message>"`
2. `git push origin <name_of_your_branch>`

To send us a pull request:

- `git pull-request` (if you are using [Hub](http://github.com/github/hub/)) or
  go to `https://github.com/flutternetwork/WiFiFlutter` and click the
  "Compare & pull request" button

Please make sure all your check-ins have detailed commit messages explaining the patch.

When naming the title of your pull request, please follow the [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0-beta.4/)
guide. For example, for a fix to the WiFi For IoT plugin:

`fix(wifi_iot): fixed a bug!`

Plugins tests are run automatically on contributions using GitHub Actions. Depending on
your code contributions, various tests will be run against your updated code automatically.

Once you've gotten an LGTM from a project maintainer and once your PR has received
the green light from all our automated testing, wait for one the package maintainers
to merge the pull request.

You must complete the
[Contributor License Agreement](https://cla.developers.google.com/clas).
You can do this online, and it only takes a minute.
If you've never submitted code before, you must add your (or your
organization's) name and contact info to the [AUTHORS](AUTHORS) file.

### The review process

Newly opened PRs first go through initial triage which results in one of:

- **Merging the PR** - if the PR can be quickly reviewed and looks good.
- **Closing the PR** - if the PR maintainer decides that the PR should not be merged.
- **Moving the PR to the backlog** - if the review requires non trivial effort and the issue isn't a priority; in this case the maintainer will:
    - Make sure that the PR has an associated issue labeled with "plugin".
    - Add the "backlog" label to the issue.
    - Leave a comment on the PR explaining that the review is not trivial and that the issue will be looked at according to priority order.
- **Starting a non trivial review** - if the review requires non trivial effort and the issue is a priority; in this case the maintainer will:
    - Add the "in review" label to the issue.
    - Self assign the PR.

### The release process

We push releases manually, using [Melos](https://github.com/invertase/melos)
to take care of the hard work.

Changelogs and version updates are automatically updated by a project maintainer
(via [Melos](https://github.com/invertase/melos)). The new version is automatically
generated via the commit types and changelogs via the commit messages.

Some things to keep in mind before publishing the release:

- Has CI ran on the master commit and gone green? Even if CI shows as green on
  the PR it's still possible for it to fail on merge, for multiple reasons.
  There may have been some bug in the merge that introduced new failures. CI
  runs on PRs as it's configured on their branch state, and not on tip of tree.
  CI on PRs also only runs tests for packages that it detects have been directly
  changed, vs running on every single package on master.
- [Publishing is
  forever.](https://dart.dev/tools/pub/publishing#publishing-is-forever)
  Hopefully any bugs or breaking in changes in this PR have already been caught
  in PR review, but now's a second chance to revert before anything goes live.
- "Don't deploy on a Friday." Consider carefully whether or not it's worth
  immediately publishing an update before a stretch of time where you're going
  to be unavailable. There may be bugs with the release or questions about it
  from people that immediately adopt it, and uncovering and resolving those
  support issues will take more time if you're unavailable.

### Run a release...

#### ...with automatically generated change logs

1) Switch to `master` branch locally.
2) Run 'git pull origin master'.
3) Run `git fetch --all` to make sure all tags and commits are fetched.
4) Run `melos version`. This will auto commit, update the changelogs using git commit messages from the last release, and also tag the release for versioning.
5) Run `git push --follow-tags` to push the auto commits and tags to the remote repository.
6) Run `melos publish` to dry run and confirm all packages are publishable.
7) Run `melos publish --no-dry-run` to now publish to Pub.dev.

#### ...with manual change log edits

To run a release and manually edit the change log, do the following steps:

1) Switch to `master` branch locally.
2) Run 'git pull origin master'.
3) Run `git fetch --all` to make sure all tags and commits are fetched.
4) Run `melos version --no-git-tag-version`. This will skip auto commiting and git tagging, leaving your git tree dirty with version bumps and changelog entry changes.
5) Update the `CHANGELOG.md` files that you manually want to edit/reword.
6) Add and commit all the `CHANGELOG.md` and `pubspec.yaml` files that were modified by `melos version` (using the standard release commit message, e.g. `chore(release): publish packages`).
7) `melos publish` to dry run and confirm all packages are publishable.
8) `melos publish --no-dry-run --git-tag-version` to now publish to Pub.dev (`--git-tag-version` will add missing git tags since we skipped tagging in step 1).
9) Push your changes to `master`; `git push --follow-tags`.

### Graduate packages

Sometimes you may need to 'graduate' a package from a 'dev' or 'beta' (versions tagged like this: `0.10.0-dev.4`) to a stable version. Melos can also be used
to graduate multiple packages using the following steps:

1) Switch to `master` branch locally.
2) Run 'git pull origin master'.
3) Run `git fetch --all` to make sure all tags and commits are fetched.
4) Run `melos version --graduate` to prompt a list of all packages to be graduated (You may also specifically select packages using the scope flag like this: `--scope="*wifi_iot*"`)
5) Run `git push --follow-tags` to push the auto commits and tags to the remote repository.
6) Run `melos publish` to dry run and confirm all packages are publishable.
7) Run `melos publish --no-dry-run` to now publish to Pub.dev.