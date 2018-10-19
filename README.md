Sonar Gitea Plugin
===================

[![Build Status](https://travis-ci.org/lafriks/sonar-gitea-plugin.svg?branch=master)](https://travis-ci.org/lafriks/sonar-gitea-plugin)

Inspired by https://github.com/gabrie-allaigre/sonar-gitlab-plugin

# Changelog

## Version 0.0.1-SNAPSHOT

### Fixed
- Fork from Sonar GitLab Plugin

# Goal

Add to each **commit** Gitea in a global commentary on the new anomalies added by this **commit** and add comment lines of modified files.

**Comment commits:**

![Comment commits](doc/sonar_global.jpg)

**Comment line:**

![Comment line](doc/sonar_inline.jpg)

**Add build line:**

![Add buids](doc/builds.jpg)

**With quality gate global comment**

![qualitygate](doc/quality_gate.png)

**With generate code quality json file**

![qualitygate](doc/codequality.png)

**With generate SAST json file**

![qualitygate](doc/sast.png)

Works with Java, Php, Android, JavaScript, C#, etc..

# Usage

For SonarQube >= 7.0:

- Download last version https://github.com/lafriks/sonar-gitea-plugin/releases/download/0.0.1/sonar-gitea-plugin-0.0.1.jar
- Copy file in extensions directory `SONARQUBE_HOME/extensions/plugins`
- Restart SonarQube

## Command line

Example:

### Issues mode (Preview) 

With Maven

```shell
mvn --batch-mode verify sonar:sonar -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.analysis.mode=preview -Dsonar.gitea.commit_sha=$CI_COMMIT_SHA -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitea.project_id=$CI_PROJECT_ID
```

or for comment inline in all commits of branch:

```shell
mvn --batch-mode verify sonar:sonar -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.analysis.mode=preview -Dsonar.gitea.commit_sha=$(git log --pretty=format:%H origin/master..$CI_COMMIT_SHA | tr '\n' ',') -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitea.project_id=$CI_PROJECT_ID -Dsonar.gitea.unique_issue_per_inline=true 
```

With SonarScanner

```shell
sonar-scanner -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.analysis.mode=preview -Dsonar.gitea.commit_sha=$CI_COMMIT_SHA -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitea.project_id=$CI_PROJECT_ID
```

With SonarScanner and node

```shell
npm run sonar-scanner -- -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.analysis.mode=preview -Dsonar.gitea.commit_sha=$CI_COMMIT_SHA -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitea.project_id=$CI_PROJECT_ID
```

With Gradle

```shell
./gradlew sonarqube -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.analysis.mode=preview -Dsonar.gitea.commit_sha=$CI_COMMIT_SHA -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitea.project_id=$CI_PROJECT_ID
```

### Publish mode (Analyse)

With Maven

```shell
mvn --batch-mode verify sonar:sonar -Dsonar.host.url=$SONAR_URL -Dsonar.login=$SONAR_LOGIN -Dsonar.gitea.commit_sha=$CI_COMMIT_SHA -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME -Dsonar.gitea.project_id=$CI_PROJECT_ID -Dsonar.branch.name=$CI_COMMIT_REF_NAME
```

Works with `sonar-scanner` and `gradle`

## Plugins properties

| Variable | Comment | Type | Version |
| -------- | ----------- | ---- | --- |
| sonar.gitea.url | Gitea url | Administration, Variable |  |
| sonar.gitea.max_global_issues | Maximum number of anomalies to be displayed in the global comment |  Administration, Variable |  |
| sonar.gitea.user_token | Token of the user who can make reports on the project, either global or per project |  Administration, Project, Variable |  |
| sonar.gitea.project_id | Project ID in Gitea or internal id or namespace + name or namespace + path or url http or ssh url or url or web | Project, Variable |  |
| sonar.gitea.commit_sha | SHA of the commit comment | Variable |  |
| sonar.gitea.ref_name | Branch name or reference of the commit | Variable |  |
| sonar.gitea.max_blocker_issues_gate | Max blocker issue for build failed (default 0). Note: only for preview mode | Project, Variable |  |
| sonar.gitea.max_critical_issues_gate | Max critical issues for build failed (default 0). Note: only for preview mode | Project, Variable |  |
| sonar.gitea.max_major_issues_gate | Max major issues for build failed (default -1 no fail). Note: only for preview mode | Project, Variable |  |
| sonar.gitea.max_minor_issues_gate | Max minor issues for build failed (default -1 no fail). Note: only for preview mode | Project, Variable |  |
| sonar.gitea.max_info_issues_gate | Max info issues for build failed (default -1 no fail). Note: only for preview mode | Project, Variable |  |
| sonar.gitea.ignore_certificate | Ignore Certificate for access Gitea, use for auto-signing cert (default false) | Administration, Variable |  |
| sonar.gitea.comment_no_issue | Add a comment even when there is no new issue (default false) | Administration, Variable |  |
| sonar.gitea.disable_inline_comments | Disable issue reporting as inline comments (default false) | Administration, Variable |  |
| sonar.gitea.only_issue_from_commit_file | Show issue for commit file only (default false) | Variable |  |
| sonar.gitea.only_issue_from_commit_line | Show issue for commit line only (default false) | Variable |  |
| sonar.gitea.build_init_state | State that should be the first when build commit status update is called (default pending) | Administration, Variable |  |
| sonar.gitea.disable_global_comment | Disable global comment, report only inline (default false) | Administration, Variable |  |
| sonar.gitea.failure_notification_mode | Notification is in current build (exit-code) or in commit status (commit-status) (default commit-status) | Administration, Variable |  |
| sonar.gitea.global_template | Template for global comment in commit | Administration, Variable |  |
| sonar.gitea.ping_user | Ping the user who made an issue by @ mentioning. Only for default comment (default false) | Administration, Variable |  |
| sonar.gitea.unique_issue_per_inline | Unique issue per inline comment (default false) | Administration, Variable |  |
| sonar.gitea.prefix_directory | Add prefix when create link for Gitea | Variable |  |
| sonar.gitea.all_issues | All issues new and old (default false, only new) | Administration, Variable |  |
| sonar.gitea.query_max_retry | Max retry for wait finish analyse for publish mode | Administration, Variable |  |
| sonar.gitea.query_wait | Max retry for wait finish analyse for publish mode | Administration, Variable |  |
| sonar.gitea.quality_gate_fail_mode | Quality gate fail mode: error or warn (default error) | Administration, Variable |  |
| sonar.gitea.issue_filter | Filter on issue, if MAJOR then show only MAJOR, CRITICAL and BLOCKER (default INFO) | Administration, Variable |  |
| sonar.gitea.load_rules | Load rules for all issues (default false) | Administration, Variable |  |
| sonar.gitea.disable_proxy | Disable proxy if system contains proxy config (default false) | Administration, Variable |  |

- Administration : **Settings** globals in SonarQube
- Project : **Settings** of project in SonarQube
- Variable : In an environment variable or in the `pom.xml` either from the command line with` -D`

# Configuration

- In SonarQube: Administration -> General Settings -> Gitea -> **Reporting**. Set Gitea Url and Token

> If sonar.gitea.failure_notification_mode is commit-status then role is Developer else Reporter.

![Sonar settings](doc/sonar_admin_settings.jpg)

- In SonarQube: Project Administration -> General Settings -> Gitea -> **Reporting**. Set project identifier in Gitea

![Sonar settings](doc/sonar_project_settings.jpg)

# Templates

Custom global/inline comment : Change language, change image, change order, print all issues, etc

**Use FreeMarker syntax [http://freemarker.org/](http://freemarker.org/)**

## Variables

Usage : `${name}`

| name | type | description |
| --- | --- | --- |
| url | String | Gitea url |
| projectId | String | Project ID in Gitea or internal id or namespace + name or namespace + path or url http or ssh url or url or web |
| commitSHA | String[] | SHA of the commit comment. Get first `commitSHA[0]` |
| refName | String | Branch name or reference of the commit |
| maxGlobalIssues | Integer | Maximum number of anomalies to be displayed in the global comment |
| maxBlockerIssuesGate | Integer | Max blocker issue for build failed |
| maxCriticalIssuesGate | Integer | Max critical issue for build failed |
| maxMajorIssuesGate | Integer | Max major issue for build failed |
| maxMinorIssuesGate | Integer | Max minor issue for build failed |
| maxInfoIssuesGate | Integer | Max info issue for build failed |
| disableIssuesInline | Boolean | Disable issue reporting as inline comments |
| disableGlobalComment | Boolean | Disable global comment, report only inline |
| onlyIssueFromCommitFile | Boolean | Show issue for commit file only |
| commentNoIssue | Boolean | Add a comment even when there is no new issue |
| revision | String | Current revision |
| author | String | Commit's author for inline |
| lineNumber | Integer | Current line number for inline issues only |
| BLOCKER | Severity | Blocker |
| CRITICAL | Severity | Critical |
| MAJOR | Severity | Major |
| MINOR | Severity | Minor |
| INFO | Severity | Info |
| sonarUrl | String | Url of SonarQube |
| publishMode | Boolean | true if publish mode |
| qualityGate | QualityGate | QualityGate |
| OK | Status | Passed QualityGate & Condition status only Global Comment |
| WARN | Status | Warning QualityGate & Condition status only Global Comment |
| ERROR | Status | Failed QualityGate & Condition status only Global Comment |

## Functions

Usage : `${name(arg1,arg2,...)}`

| name | arguments | type | description |
| --- | --- | --- | --- |
| issueCount | none | Integer | Get new issue count |
| issueCount | Boolean | Integer | Get new issue count if true only reported else false only not reported |
| issueCount | Severity | Integer | Get new issue count by Severity |
| issueCount | Boolean, Severity | Integer | Get new issue count by Severity if true only reported else false only not reported |
| issues | none | List<Issue> | Get new issues |
| issues | Boolean | List<Issue> | Get new issues if true only reported else false only not reported |
| issues | Severity | List<Issue> | Get new issues by Severity |
| issues | Boolean, Severity | List<Issue> | Get new issues by Severity if true only reported else false only not reported |
| print | Issue | String | Print a issue line (same default template) |
| emojiSeverity | Severity | String | Print a emoji by severity |
| imageSeverity | Severity | String | Print a image by severity |
| ruleLink | String | String | Get URL for rule in SonarQube |

Usage : `${qualityGate.name(arg1,arg2,...)}`

| name | arguments | type | description |
| --- | --- | --- | --- |
| conditions | Function none argument | List<Condition> | Get all conditions |
| conditions | Function Status argument | List<Condition> | Get all conditions with Status|
| conditionCount | Function none argument | Integer | Get size of all conditions |
| conditionCount | Function Status argument | Integer | Get size conditions with Status|

### Types 

Usage : `${Issue.name}`

| name | type | description |
| --- | --- | --- |
| reportedOnDiff | Boolean | Reported inline |
| url | String | URL of file/line in Gitea |
| componentKey | String | Component key |
| severity | Severity | Severity of issue |
| line | Integer | Line (maybe null) |
| key | String | Key |
| message | String | Message (maybe null) |
| ruleKey | String | Rule key on SonarQube |
| new | Boolean | New issue |
| ruleLink | String | URL of rule in SonarQube |
| src | String | File source |
| rule | Rule | Rule information |

Usage : `${Issue.rule.name}`

| name | type | description |
| --- | --- | --- |
| key | String | Rule key |
| repo | String | Rule repository |
| name | String | Name of rule |
| description | String | Description of rule |
| type | String | CODE_SMELL, BUG, VULNERABILITY, SECURITY_HOTSPOT |
| debtRemFnType | String | Debt type |
| debtRemFnBaseEffort | String | Debt effort |

Usage : `${qualityGate.name}`

| name | type | description |
| --- | --- | --- |
| status | Status | Status of quality gate |

Usage : `${Condition.name}`

| name | type | description |
| --- | --- | --- |
| status | Status | Status of condition |
| actual | String | Actual value for metric |
| warning | String | Warning value |
| error | String | Error value |
| metricKey | String | Metric key |
| metricName | String | Metric name |
| symbol | String | Symbol of condition <,>,=,!= |

## Examples

### Global

```injectedfreemarker
<#if qualityGate??>
SonarQube analysis indicates that quality gate is <@s status=qualityGate.status/>.
<#list qualityGate.conditions() as condition>
<@c condition=condition/>

</#list>
</#if>
<#macro c condition>* ${condition.metricName} is <@s status=condition.status/>: Actual value ${condition.actual} is ${condition.symbol}<#if condition.status != OK && condition.message?? && condition.message?trim?has_content> (${condition.message})</#if></#macro>
<#macro s status><#if status == OK>passed<#elseif status == WARN>warning<#elseif status == ERROR>failed<#else>unknown</#if></#macro>
<#assign newIssueCount = issueCount() notReportedIssueCount = issueCount(false)>
<#assign hasInlineIssues = newIssueCount gt notReportedIssueCount extraIssuesTruncated = notReportedIssueCount gt maxGlobalIssues>
<#if newIssueCount == 0>
SonarQube analysis reported no issues.
<#else>
SonarQube analysis reported ${newIssueCount} issue<#if newIssueCount gt 1>s</#if>
    <#assign newIssuesBlocker = issueCount(BLOCKER) newIssuesCritical = issueCount(CRITICAL) newIssuesMajor = issueCount(MAJOR) newIssuesMinor = issueCount(MINOR) newIssuesInfo = issueCount(INFO)>
    <#if newIssuesBlocker gt 0>
* ${emojiSeverity(BLOCKER)} ${newIssuesBlocker} blocker
    </#if>
    <#if newIssuesCritical gt 0>
* ${emojiSeverity(CRITICAL)} ${newIssuesCritical} critical
    </#if>
    <#if newIssuesMajor gt 0>
* ${emojiSeverity(MAJOR)} ${newIssuesMajor} major
    </#if>
    <#if newIssuesMinor gt 0>
* ${emojiSeverity(MINOR)} ${newIssuesMinor} minor
    </#if>
    <#if newIssuesInfo gt 0>
* ${emojiSeverity(INFO)} ${newIssuesInfo} info
    </#if>
    <#if !disableIssuesInline && hasInlineIssues>

Watch the comments in this conversation to review them.
    </#if>
    <#if notReportedIssueCount gt 0>
        <#if !disableIssuesInline>
            <#if hasInlineIssues || extraIssuesTruncated>
                <#if notReportedIssueCount <= maxGlobalIssues>

#### ${notReportedIssueCount} extra issue<#if notReportedIssueCount gt 1>s</#if>
                <#else>

#### Top ${maxGlobalIssues} extra issue<#if maxGlobalIssues gt 1>s</#if>
                </#if>
            </#if>

Note: The following issues were found on lines that were not modified in the commit. Because these issues can't be reported as line comments, they are summarized here:
        <#elseif extraIssuesTruncated>

#### Top ${maxGlobalIssues} issue<#if maxGlobalIssues gt 1>s</#if>
        </#if>

        <#assign reportedIssueCount = 0>
        <#list issues(false) as issue>
            <#if reportedIssueCount < maxGlobalIssues>
1. ${print(issue)}
            </#if>
            <#assign reportedIssueCount++>
        </#list>
        <#if notReportedIssueCount gt maxGlobalIssues>
* ... ${notReportedIssueCount-maxGlobalIssues} more
        </#if>
    </#if>
</#if>
```

**Others examples for global :**
- [Template Default](templates/global/default.md) Current template
- [Template Default with Images](templates/global/default-image.md) Same template as default but with images
- [Template All Issues](templates/global/all-issues.md) Print all issues

### Inline

```injectedfreemarker
<#list issues() as issue>
<@p issue=issue/>
</#list>
<#macro p issue>
${emojiSeverity(issue.severity)} ${issue.message} [:blue_book:](${issue.ruleLink})
</#macro>
```

**Others examples for inline :**
- [Template Default](templates/inline/default.md) Current template
- [Template Default with Images](templates/inline/default-image.md) Same template as default but with images


# Tips


## Import Gitea SSL certifcate

- **On your server** import Gitea SSL certificate into the JRE used by SonarQube

If you don't already have you certificate on the SonarQube server, run `openssl s_client -connect mygitea.com:443 -showcerts > /home/${USER}/mygitea.crt`

Import it into your JRE cacerts (you can check from the "System Info" page in the Administration section of your sonarqube instance), running `sudo $JDK8/bin/keytool -import -file ~/mygitea.crt -keystore $JDK8/jre/lib/security/cacerts -alias mygitea`.

Restart your SonarQube instance.

## Make it works with a private project
- On a private project, with the provided command line upper in this document, you'll got an "IllegalStateException: Unable found project for" Exception.
- It's necessary to create a "Personal Access Tokens". It needs a user with the "developper" right in the project. The token can be created in the profile menu, check the api checkbox. 
- Then, use the following command line to run sonar thought gitea-ci 

``` batch
mvn --batch-mode verify sonar:sonar
      -Dsonar.host.url=http://<your_sonar_url>:9000
      -Dsonar.login=<your_sonar_login>
      -Dsonar.analysis.mode=preview
      -Dsonar.gitea.commit_sha=$CI_COMMIT_SHA
      -Dsonar.gitea.ref_name=$CI_COMMIT_REF_NAME
      -Dsonar.gitea.project_id=$CI_PROJECT_ID
      -Dsonar.gitea.url=http://<your_gitea_url>
      -Dsonar.gitea.user_token=<your_user_token>
```
