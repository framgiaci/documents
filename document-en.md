## Sun*CI Documents
`Sun*CI` is a CI/CD system developed by Sun Asterisk , beside the main features like the other CI/CD system, `Sun*CI` provided more features for assisting user in developing projects.Here's some instructions you'd better look before intergrate with `Sun*CI`
## Permission
Currently, `Sun*CI` is suporting user login to system using Github, Gitlab  (both https://gitlab.com/ and Sun* Gitlab), Bitbucket account, after logging in, by default your permission will be `User`. Your accessibility  to `Sun*CI` base on your permission as show below:

<table>
    <tr>
        <td rowspan="2" colspan='2'>Page</td>
        <td colspan="3">Permission</td>
    </tr>
    <tr>
        <td>Admin</td>
        <td>Mod</td>
        <td>User</td>
    </tr>
    <tr>
        <td colspan='2'>Home page</td>
        <td>+</td>
        <td>+</td>
        <td>+</td>
    </tr>
    <tr>
        <td colspan='2'>Available repository</td>
        <td>+</td>
        <td>+</td>
        <td>-</td>
    </tr>
    <tr>
        <td rowspan='5'>Admin page</td>
        <td>Add user</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Nodes</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Statistics</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Active repository</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    <tr>
        <td>Build list</td>
        <td>+</td>
        <td>-</td>
        <td>-</td>
    </tr>
    
</table>

   
## Homepage ( Active Repositories)
This page displays list of projects that you were added by repository's owner and list of repositories that you actived on `Sun*CI` system.

![home](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_010.png)

<table>
    <tr>
        <td>#</td>
        <td>Feature</td>
        <td colspan='2'>Description</td>
    </tr>
    <tr>
        <td rowspan='8'>1</td>
        <td rowspan='8'>Statistic</td>
        <td>Active project</td>
        <td>Total number of project actived</td>
    </tr>
    <tr>
        <td>Buid success</td>
        <td>Total number of build succeeded.</td>
    </tr>
    <tr>
        <td>Build error</td>
        <td>Total number of error build. Build status will be changed to error when at least one commmand fail.</td>
    </tr>
    <tr>
        <td>Build killed</td>
        <td>Total number of killed build. Build status will be changed to killed if there's no logs show up in a period of time</td>
    </tr>
    <tr>
        <td>Build skipped</td>
        <td>Total number of skipped build. If user force push in process of build. Build's status will be changed to skipped</td>
    </tr>
    <tr>
        <td>Build timeout</td>
        <td>Total number of build exceed time, status will be changed to build timeout</td>
    </tr>
    <tr>
        <td>Build running</td>
        <td>Total number of running build.</td>
    </tr>
    <tr>
        <td>Total User</td>
        <td>Total number of user registered</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Search</td>
        <td>Filling project field</td>
        <td>Search project by project's name</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Repository/project list</td>
        <td></td>
        <td></td>
    </tr>
</table>

## 3. Available repositories
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_012.png)
 
 <table>
    <tr>
        <td>#</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>1</td>
        <td>This button update the repository list</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Filter project by it's status ( All/Personal/Organization )</td>
    </tr>
    <tr>
        <td>3</td>
        <td>Search project by name</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Repository/Project list fetched from github account </td>
    </tr>
</table>

> You need to be an admin of repository to active project and create hook on github
> 
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_013.png)

> 
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_014.png)

## 4. Overview
Overview page display summary information, build statistic of repository.This page is shown up when you click on a project actived at homepage.
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_015.png)
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_016.png)

>1. The last build information
>2. Number of builds, success build and error build 
>3. This chart display errors of 10 recently builds. 
>4. This chart analyze builds in 10 days. 
## 5. Build
By clicking on each build, detail page will show up, detail page includes some details as below:
> Logs: Display log result of each command that you defined in config file.

> Coverage: Unit test result

> Violations: Detail information of code conventions violated in project.

> Metrics: Static analyzer for programming languages
> 
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_023.png)

<table>
    <tr>
        <td>#</td>
        <td>Description</td>
    </tr>
    <tr>
        <td>1</td>
        <td>Build detail, build's owner Etc.</td>
    </tr>
    <tr>
        <td>2</td>
        <td>Tabs display log, coverage, violation .Etc.</td>
    </tr>
    <tr>
        <td>3</td>
        <td>This options allow you to show logs line by line or scroll to the bottom of build logs block</td>
    </tr>
    <tr>
        <td>4</td>
        <td>Scroll to the end of build logs block</td>
    </tr>
    <tr>
        <td>5</td>
        <td>Logs detail after run all the commands</td>
    </tr>
    <tr>
        <td>6</td>
        <td>Scroll to the top of build logs block</td>
    </tr>
</table>


## 6. Setting & Notification
You can access `Sun*CI` setting by clicking the `setting tab` on each repository detail page (You must be an owner or admin of github repository to take this action).

![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/setting.png)

`Webhook setting` allow you to turn on or off which event github you want to run build.

`Build setting` : You can run build in some specific branch by filling on this field branch name and press Enter.

For example, when you fill `develop` and Enter, `Sun*CI` only will run build with push event to `develop` or when someone sends a pull request to this branch.

Beside, `Sun*CI` also brings out a Comment-On-Github feature. After every build finishes running. `Sun*CI` bot will let you know which lines get code conventions errors .

To access this feature, you need to click on `setting` tab and add `Sun*CI` bot to your repository.

![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Selection_022.png)

Notification allow user to send message via Chatwork, Slack or Email after every build finishes running.

To do this, you need to follow instructions as follows:
> Chatwork:
> 1. Add bot's key
> 2. Add room id. Remember to add the bot on step 1 to this chatwork room.
> 3. In case you just want to send message with some specific branch or specific github's event, follow instruction below :
![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/chatwork.png)

> Slack:
> 1. Add slack hook ( include `slack webhook` and `slack display name`
> 2. Add room 

### 6.1. Remote  configuration
By activating this feature, you are using remote configuration yml file instead of `framgia-ci.yml` on your repository.

![available](https://raw.githubusercontent.com/framgiaci/documents/master/images/Remote_Configuration.png)

To access this feature, click on `Use remote configuration` and custom your own configuration by write on editor on `Sun*CI` system.

