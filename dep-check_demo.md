# Dependency-Check demo

To test it out, make your own HelloWorld project (though this will take extra work to find and include vulnerable libraries), or use an older or DEPRECATED project that gives results without trying too hard, e.g.:
* https://github.com/OwlCarousel2/OwlCarousel2
* https://github.com/williamfiset/DEPRECATED-data-structures

Be sure to fork someone else's repo if you need to add to it rather than mucking up their work.

1. (Do basic Jenkins job setup)
2. Build Steps: add Invoke Dependency-Check
3. Post-build Actions: add Publish Dependency-Check results
4. Click Save.

Back on project page, click Build Now.

Results: Project page > Latest Dependency-Check 

To watch progress: click build > Console Output

<br /><br /><br /><br />

[Next slide](sast.md)
