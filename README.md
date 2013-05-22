Getting this set up in CI

download and install jenkins msi
add proxy details (jse\username) copy server details from intrernet options
add git and msbuild plugins
add MSBuild installation - in Jenkins > Manage Jenkins > Configure, under MSBuild installations. Path to build something like C:\Windows\Microsoft.NET\Framework64\v4.0.30319

Add new job for the app:
Select Git under Source Code Management
Give path to Git repo
Branch to build - master
Poll every minute - */1 * * * * (not ideal - rather use hook)
Add build step > Execute Windows Batch Command
- del results.trx
Add build step > Build a Visual Studio project or solution using MSBuild.
- Select MSBuild version you set up earlier
- No build file unless required
- Pass sln file as argument
Add build step > Execute Windows Batch Command
- "C:\Program Files (x86)\Microsoft Visual Studio 11.0\Common7\IDE\mstest.exe" /testcontainer:AwesomeApp.Tests\bin\Debug\AwesomeApp.Tests.dll /resultsfile:results.trx
Add post-build action > Publish MSTest test result report
- Test report TRX file = results.trx (or whatever file specified above)



** look into git hooks - change file in .git/hooks. need to install something like curl though curl http://localhost:8080/git/notifyCommit?url="C:\Users\rouanw\Documents\projects\AwesomeApp"

Seems easy enough on GitHub - http://www.foraker.com/hudson-github-hooks/
https://wiki.jenkins-ci.org/display/JENKINS/GitHub+Plugin

msbuild AwesomeApp.sln
mstest /testcontainer:AwesomeApp.Tests/bin/Debug/AwesomeApp.Tests.dll 
/resultsfile:results.trx

note:
/safeRestart restart manually