# Unity SDK Tester

## Goal

This project defines a simple Unity project with only one meaningful dependency: the [Unity SDK](https://github.com/TradeliteOrganization/unity-sdk) for the games connected to the [Tradelite Solutions](https://tradelite.de/) Platform. The corresponding Docker image defines a standalone process that invokes the Unity Editor runtime in _batch mode_, that transforms the XML report in a human readable HTML report, and that notifies contributor about the test run outcome.

## Requirements

The following dependencies are documented in the `Dockerfile`:

- `Unity`: Unity editor to be used in batch mode, to run the tests for the project, and producing a NUnit test report
- `xsltproc`: default XSLT processor available on Linux and MacOS
- `nunit3-junit.xml`: XSL transform definition from the [NUnit Transforms](https://github.com/nunit/nunit-transforms.git) project to generate the JUnit test report
- `xunit-viewer`: [NodeJS program](https://github.com/lukejpreston/xunit-viewer) to generate the visual HTML test report from the JUnit test report
- `git`
- `curl`

## Main Sequence

```zsh
> cd <project-folder>
> git pull origin main
> .../Unity/Hub/Editor/2020.3.25f1/Unity.app/Contents/MacOS/Unity -runTests -batchmode -projectPath . -testPlatform EditMode -testResults /tmp/nunit-test-results.xml
> curl https://raw.githubusercontent.com/nunit/nunit-transforms/master/nunit3-junit/nunit3-junit.xslt > /tmp/nunit3-junit.xslt
> xsltproc /tmp/nunit3-junit.xslt /tmp/nunit-test-results.xml > /tmp/junit-results.xml
> npx xunit-viewer --results=/tmp/junit-results.xml --title "Tradelite Unity SDK" --save=/tmp/report.html

The sequence above does not handle the test run errors, does not define any notification mechanism. Check the Dockerfile for more details.