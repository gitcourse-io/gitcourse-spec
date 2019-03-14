# GitCourse Spec  
## GitCourse Project Structure  
Each GitCourse project has a `course.json` in it's root directory, `course.json` defines all the configurations related to the course like title, author, scenarios and so on.  
  
One typical directory structure of GitCourse is shown as follows:  
```$xslt  
- Project_root  
|- course.json
|- scenario1
	|-- step1.md
	|-- step2.md
	|-- backgound.sh
	|-- check-step1.sh
|- ...other files or directories  
```  
## What's in `course.json`?  
First let's see a simple example of `course.json`:  
```json  
{
  "version": "1", 
  "title": "GitCourse Example", 
  "author": "GitCourse Players", 
  "description": "This is an example of GitCourse project", 
  "scenarios": [
    {
      "title": "This is a scenario",
      "steps": [
        {
          "title": "First Step", 
          "text": "first-step.md"
        }, 
        {
          "title": "Second Step", 
          "text": "second-step.md"
        }
      ]
    }, {
      "title": "This is another scenario",
      "steps": [
        {
          "title": "First Step", 
          "text": "another-first-step.md"
        }, 
        {
          "title": "Second Step", 
          "text": "another-second-step.md"
        }
      ]
    }
  ]
}
```  
As you can see, the `course.json` file is a simple json object, and the content is super clear and human readable.  
  
The meaning of each field is described as follows:  
  

| field | type | default value | description |
|--|--|--|--|
| version | string  | "1" | The gitcourse parser version to use, we support "1" currently|
| title | string | "" | The title of the course |
| author | string | "" | The author of the course |
| description | string | "" | The description of the course |
| scenarios | array | [] | The scenarios included in the course, the order of each scenario corresponds to the order when rendered as a GitCourse |

## Scenario
The `scenarios` in `course.json` is an array of `Scenario` which contains necessary configurations to build an interactive scenario. For example:
```json
{
      "title": "This is a scenario",
      "description": "The description",
      "environment": "git",
      "scripts": {
	    "background": "bg.sh",
	    "foreground": "fg.sh"
	  },
      "steps": [
        {
          "title": "First Step", 
          "text": "first-step.md"
        }, 
        {
          "title": "Second Step", 
          "text": "second-step.md"
        }
      ]
    }
```
The available properties is listed as follows:

| field | type | default value | description |
|--|--|--|--|
| title | string  | - | The title which shows when scenario opened|
| description | string | - | The description of this scenario |
| environment | enum {'ubuntu', 'git', 'mysql', 'docker'} | 'ubuntu' | The container environment for the scenario, we support limited environments currently |
| scripts | object | {} | The scripts runs automatically when scenario starts, eg: {background: 'background.sh', foreground: 'foreground.sh} |
| steps | array | [] | The steps in this scenario, the order of step corresponds to the order when rendered, the properties of step please refer to details bellow |

## Step
The `steps` field in `scenario` object is a list of `Step` object, for example:
```json
{
          "title": "First Step", 
          "text": "myscenario/first-step.md",
          "scripts": {
            "before": "myscenario/before.sh",
            "after": "myscenario/after.sh"
          },
          "check": "myscenario/check.sh"
}
```
The available properties is listed as follows:

| field | type | default value | description |
|--|--|--|--|
| title | string  | - | The title the step|
| text | string | - | The text content file of this step, it should be a file path relative to project root directory |
| scripts | object | {} | The scripts executed automatically at certain times，eg: {before: 'before.sh', after: 'after.sh'}, here `before` means this script will run  when user switch to this step, `after` means this script will run when user switch to another step from this step. The value of scripts object is a path to the script file which is relative to project root directory.|
| check | string | '' | A script that check some test is already succeed, if the script output "1" to stdout, the user can goes to next step, otherwise the user cannot goes to next step.|
