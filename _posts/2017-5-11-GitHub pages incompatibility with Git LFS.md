---
layout: post
title: "GitHub pages don't get along with Git LFS"
date: 2017-5-11 06:11:11.000000000 -06:00
tags: [GitHub Pages]
---
I maintain the website as usuall trying to update a post and some pictures. but when I push the commit to the GitHub Pages repo and try to preview the page, it is not displaying all the pictures properly.

Only thing I see is the deadlink for each picture which is pretty weird. I do not know what is wrong with it and I think it may have something to do with the relative page path or what.

# **Here is the comparision**
![Local](/assets/gp/Homepage_local.png)
![Server](/assets/gp/Homepage_GH.png)


Then I go modifying all the relative path letting it point to the base url that I added to my config yml file. Then even wrose thing occured that my homepage, side panel and every single picture resource is not avaliable now even it works perfectly on local Jekyll server.

Damn, ok, I have another way around that I can go back to previous version and problem should be solved. Yeah, but life is full of disappointments, it just failed dispaying right pic resouces again even the path is pointing to the right place and the pics are vivid in my repository.

## **It drives me up the wall.** 
I start scanning every single line of code about img display since every other part is functional. But the irritating thing is that why the heck this is working perfectly on local machine but not on remote server.

I was *desperate* for few days and try to get some valuable info from stackoverflow or GibHub but not even one solution can be applied to my case.

When I am going to accept the fact that I am not be able to share my wonderful images on my homepage and have to tolorate the ugly as hell user interface in my life. 

Suddenly, My eyes were caught by few lines of strange message saying you file was stored as lfs something when I commit and push the new imgaes files to my repo for doing some beat a dead horse job.

Is it something wrong with the LFS stuff? Questions arose...

I go checking the LFS comfiguration. I noticed that each time I do some experiment uploading any large files like images, GitHub will automatically convert the normal files to LFS type. Then I wonder if there is a problem with the LFS.

After googling the term and the machanism it worked, I finally got the answer why I am not be able to dispaly my images now. 


When we push our files to the remote repository, github will detect and determine if this file needs to be stored as lfs type. I have not idea what the algorithms they use under the hood, but the thing is that onece you file was considered to be a **LARGE FILE**, it will be stored in **Large File System** which means that they in fact dont store the actual image file but the reference pointing to it. 

I am playing with the references rather than the real images for few days, that is why I cannot view any images.

![what???](/assets/gp/what.gif)

## Solution ##
remove the remote "infected" images in the directory
```
git rm -r --cached my-images-directory

```
then go to .gitattribute delete the rules about lfs
```
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
```
I dont wanna it back anymore.

Then push these images again, it wont be stored as lfs anymore.
Problem solved.

![happy](/assets/gp/happy.gif)












