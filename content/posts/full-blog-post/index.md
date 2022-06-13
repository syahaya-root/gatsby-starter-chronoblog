---
title: How to Push to Git
cover: ./git.png
date: 2022-06-13
description: Git.
tags: ['post']
---

- Before you continue the operation, It is recommended to fill in the author data first.
```
git config --global user.name "username" ==> this for your username
git config --global user.email "your_email" ==> this for your mail
```
### Create a new repository in the Web Github

- This reference for create new repository in Github :
( https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository )

- After you create new repository in Github Web, you can clone the repository with command:
```
git clone <URL_Repository>
```

- Enter the the directory
```
cd name_directory
```

- You can start Git process with command:

```
git init
```

- For connect your Github project with your local directory, you can use command:
```
git remote add origin <project link> (make sure it is .git)
```

- Please add the file to be transferred to Github Project.
```
git add . ("." it is for all file in the directory)
```

- Then, you can check the all file in status git with command:
```
git status
```

-  Make the commit, so that you know which files were committed.
```
git commit -m "comment" (comment for description)
```

- After that, you ready for the push.
```
git push -u origin master ("master" is default branch)
```

Note: if you use the 2FA (Second Factor Authentication) you can change the URL remote in the file ".git/config", so like this:
```
url = https://oauth2:ACCESS_TOKEN@gitlab.com/yourself/name_project.git
```
