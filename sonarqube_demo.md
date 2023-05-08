# Including SonarQube in your Jenkins project

To test this, make your own HelloWorld project with some atrocious bugs, or use a project that gives results without trying too hard, e.g.:
* https://github.com/codeprimate/password-vault

Again, fork someone else's repo if you need to modify it.

1. (Do basic Jenkins job setup)
2. If Java, add a build step first; SonarQube scans class files rather than source code
4. Add Build Step: Execute SonarQube Scanner: Analysis properties:
    1. sonar.ProjectKey=_[project name with no spaces]_
    1. sonar.language=_[Main language used]_
    1. (Leave everything else as is)
1. Click Save

Back on project page, click Build Now.

Results: Project page > SonarQube link

To watch progress: click build > Console Output

[Next slide](pipe_as_code.md)
