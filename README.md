# CardCannon

This application is used to manage Turing's student facing projects. Instructors can write and maintain project stories in this repo, and use the scripts provided to populate a GitHub repo with issues. These issues can then be easily imported into a GitHub project as user stories. Once a GitHub project is created, instructors can use [Conveyor Belt](https://github.com/jmejia/conveyor-belt) to provide each student with a copy of the project board to be used for project management.

## Setup

1. Clone this repository
1. Run `bundle install` from the command line
1. Run `bundle update` from the command line

### GitHub Personal Access Token

In order to use the GitHub API, you will need to provide a personal access token. In order to obtain the token:

1. Log in to your GitHub account
1. Go to `Settings/Developer Settings/Personal access tokens`
1. Click `Generate new token`
1. Enter a name, for example `card cannon`
1. Select all the `repo` scopes
1. Click `Generate token`
1. Copy the generated token.

Once you have obtained the token, you can add it to this application:

1. Open `slam.rb` in this repository.
1. Find the line near the top of the file `personal_access_token = ''`
1. Inside the empty string, paste your personal access token.

It is not recommended that you commit or push you personal access token.

## Usage

### Writing a Project

Make a folder for your project name, like "little_shop", in this repository.

Within that folder, create one or more files ending in .ini. Each .ini file represents an epic. Each story within that file is part of the epic. Epics will be processed alphabetically, so it is recommended that you start your file names with numbers to specify the order of epics.

You will also need to include a `story-checklist.md` file; this markdown will be appended to every user story (but not epics) so GitHub can utilize markdown checkboxes for progress. You can leave this file blank if you want but it must exist.

#### INI file structure

Structure your INI files in the following format.

Each "slug" is a unique identifier for the story or epic. It can be anything you want.

```
[epic-slug]
title = Epic: Whatever
labels: whatever
story_text: line 1 of story
    line 2 of story
    line 3 of story
    etc

[story-slug-2]
title = A Very Whatever Story
labels: whatever
child_of: epic-slug
story_text: line 1 of story
    line 2 of story
    line 3 of story
    etc

[story-slug-3]
title = A Very Whatever Story
labels: whatever
child_of: epic-slug
depends_on: story-slug-2
story_text: line 1 of story
    line 2 of story
    line 3 of story
    etc
```

#### Labels

Labels are used mostly for project management tools to 'link' common-themed stories together across epics. For example, multiple epics might deal with a particular resource or module.

The "build-cannon.py" script will also add two labels to each non-epic story: 'to do' and 'backlog'.

#### Rationale/Excuses:

- I use an equal (`=`) on the title line in case my titles need colons, like `Epic: Whatever`
- the INI section markers are slug names, these get turned into ID values later and the strings are discarded
- the script will process INI files in whatever natural order your OS reads them, typically alphabetically

#### More about stories.md

The Python script also builds one big concatenated `stories.md` in the root of this project's folder where each epic is printed as-is, includes a markdown line separator of `---`, and numbers each story to look like this:

```
[ ] done

User Story 17
User Profile Show Page

As a registered user
When I visit my own profile page
Then I see all of my profile data on the page except my password
And I see a link to edit my profile data
```

### Populating a repo with issues

Once you have created your project locally in .ini format, you can populate a repo with issues by following these steps:

1. run `python build-cannon.py <project directory>` where `<project directory>` is a folder in this repo that contains you ini files with user stories. You should see "outputing all stories per Waffle Cannon" if it was successful.
    * ex `python build-cannon.py test_project`
    * Requires Python 2. If you do not have Python 2 you will need to install it.
1. run `ruby slam.rb <project directory> <GitHub user>/<GitHub repo name>` where `<GitHub user>` is the user or organization that owns the repository and `<GitHub repo name>` is the name of the repository.
    * ex `ruby slam.rb test_project turingschool-examples/test_project`. This would assume there is a repository at `github.com/turingschool-examples/test_project`
1. Navigate to the GitHub repository. You should see your user stories in the repo's issues.

### Creating a GitHub Project

Once you have populated a repo with issues, you can create a project:

1. In the GitHub repository, select the `projects` tab
1. Click `Create a Project`. Give your project a name.
1. Add some columns to the project board, for example `to do`, `in progress`, `done`, etc.
1. Click `Add cards`
1. You should now be able to drag your user stories into your project.

Once your project is set up, you can use [Conveyor Belt](http://conveyorbelt.herokuapp.com/) to provide students with a forked repo and copy of the project board to streamline project management.
