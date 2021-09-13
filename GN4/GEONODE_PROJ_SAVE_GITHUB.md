# Save Changes to GitHub

## Prepare the GeoNode Fork
- Go to GitHup `https://github.com/GeoNode/geonode` and select the correct version/branch linked to the `geonode-project`
   
```shell
cat requirements.txt 
# -e git+https://github.com/GeoNode/geonode.git@3.2.x#egg=GeoNode
```

- Click on the `Fork` button and select the target `repository`

    ![image](https://user-images.githubusercontent.com/1278021/133079530-5d34ab88-73ab-4db8-a119-539d0aa90242.png)

- Wait for the process to finish... at the end you will be able to see GeoNode on your own `repository` with the same branches and the same commits of `upstream`

    ![image](https://user-images.githubusercontent.com/1278021/133079926-15e9dedb-6809-4a78-9ee2-ea930ac1dbfb.png)

## Add the Fork and Branch to the Code
- Click on the `Code` dropdown and copy the new `fork URL`

    ![image](https://user-images.githubusercontent.com/1278021/133080180-f4127cfe-3ee6-4153-97c9-15c3faadaa6e.png)

- Go back to the shell and move to

    `cd /opt/geonode`
    
- Type the following commands

```shell
# Add the new GitHub remote address
git remote add <a name> <the fork URL>   <-- e.g.: git remote add afabiani https://github.com/afabiani/geonode.git

# Update the .git references
git fetch --all

# Create a new branch
git checkout -b geonode_training_32x

# Double check you switched to the correct branch
git branch
  3.2.x
* geonode_training_32x

# Push the new branch to your fork
git push <name of the remote> <name of the branch  <-- e.g: git push afabiani geonode_training_32x
```

- Example
```shell
git push afabiani geonode_training_32x

Total 0 (delta 0), reused 0 (delta 0)
remote: 
remote: Create a pull request for 'geonode_training_32x' on GitHub by visiting:
remote:      https://github.com/afabiani/geonode/pull/new/geonode_training_32x
remote: 
To https://github.com/afabiani/geonode.git
 * [new branch]          geonode_training_32x -> geonode_training_32x
```

![image](https://user-images.githubusercontent.com/1278021/133082197-eccaadf5-49d6-47c9-a5fd-00061287990f.png)

## Generate a Push Token
- GitHub will ask you to provide a `user/password` to perform `pushes/commits`, but it will force you to create a `token`
- In order to create (or refresh) a `token`, go to `settings`

    ![image](https://user-images.githubusercontent.com/1278021/133082730-f7addd68-fe8b-4b86-9f0b-bfa40eb916a7.png)

- Click on `Developer Settings`

    ![image](https://user-images.githubusercontent.com/1278021/133083004-53b1392c-22c5-4eaf-b98d-d16aeeac2db6.png)

- Generate a new `token`

    ![image](https://user-images.githubusercontent.com/1278021/133083825-9e5f942f-80fc-45a8-afe7-cbd008cbf449.png)

- Provide a `name`, and `expiration date` and select the `privileges`, the ones below will allow you to push commits to the repo

    ![image](https://user-images.githubusercontent.com/1278021/133084043-609f2567-b770-49bd-aa46-e21ac873f6eb.png)

- Copy the new `token` and store it somewhere, otherwise you will need to refresh it

    ![image](https://user-images.githubusercontent.com/1278021/133084221-e1b1f69f-df7b-45c3-a726-37027ffc0386.png)

## Commit the Changes to GitHub
- Check the repository status

```shell
cd /opt/geonode
git status

On branch geonode_training_32x
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   geonode/base/api/serializers.py
	modified:   geonode/base/forms.py
	modified:   geonode/base/models.py

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	geonode/base/migrations/0061_resourcebase_custom_md.py
	geonode/base/migrations/0062_auto_20210910_1445.py

no changes added to commit (use "git add" and/or "git commit -a")
```

- Add the `unchecked` files to the `commit`

```shell
git add geonode/base/migrations/0061_resourcebase_custom_md.py geonode/base/migrations/0062_auto_20210910_1445.py

git status

On branch geonode_training_32x
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   geonode/base/migrations/0061_resourcebase_custom_md.py
	new file:   geonode/base/migrations/0062_auto_20210910_1445.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   geonode/base/api/serializers.py
	modified:   geonode/base/forms.py
	modified:   geonode/base/models.py
```

- Create a `commit` and `push` the changes

```shell
git commit -a -m " - Added a custom_md field to the GeoNode ResourceBase metadata"
```
```shell
git status

On branch geonode_training_32x
nothing to commit, working tree clean
```
```shell
git log

commit b359f1c49a1196727a6af9fa916cb11b6d28dbdd (HEAD -> geonode_training_32x)
Author: afabiani <alessio.fabiani@geo-solutions.it>
Date:   Mon Sep 13 14:24:52 2021 +0100

     - Added a custom_md field to the GeoNode ResourceBase metadata

commit 911a0f14eb7204c622c807ba877b4a2dce3e3caa (afabiani/geonode_training_32x, 3.2.x)
Author: Toni <toni.schoenbuchner@csgis.de>
Date:   Tue Aug 31 17:57:04 2021 +0200

    added jcaceres85
    
    (cherry picked from commit 5553d1882718d38c742e6e4a86bf1ad51f14fb15)
    
    # Conflicts:
    #       .clabot
...
```

 - Push the changes to the remote repostory

```shell
git push <name of the remote> <name of the branch  <-- e.g: git push afabiani geonode_training_32x
```

- Example
```shell
git push afabiani geonode_training_32x

Enumerating objects: 19, done.
Counting objects: 100% (19/19), done.
Delta compression using up to 2 threads
Compressing objects: 100% (11/11), done.
Writing objects: 100% (11/11), 1.34 KiB | 684.00 KiB/s, done.
Total 11 (delta 9), reused 0 (delta 0)
remote: Resolving deltas: 100% (9/9), completed with 8 local objects.
To https://github.com/afabiani/geonode.git
   911a0f14e..b359f1c49  geonode_training_32x -> geonode_training_32x
```

## Create a Rpository for the geonode-project
The steps are more or less the same as above, except that this time we have to create a **new repository** from scratch instead of **forking** an existing one.

- Create a new repository for the `geonode-project`

```shell
/opt/geonode-project/my_geonode

# Initialise an empty Git repository
git init
```

   * Go to `https://github.com/<name>?tab=repositories` (e.g. `https://github.com/afabiani?tab=repositories`),  and create a new `repository`
   
      ![image](https://user-images.githubusercontent.com/1278021/133092826-17a6a932-7765-4f8c-8b17-5a4d49124f5a.png)

   * Provide a name
   
      ![image](https://user-images.githubusercontent.com/1278021/133092987-bfe3a8d6-ac1f-43b3-91ce-314730d9aa9a.png)

   * Copy the link
   
      ![image](https://user-images.githubusercontent.com/1278021/133093132-ee0a9415-917b-4b38-b00e-a7888882f1f3.png)

- Add the `repository` to the `geonode-project`

```shell
git remote add <a local name> https://github.com/<fork name>/my_geonode.git  <-- e.g.: git remote add afabiani https://github.com/afabiani/my_geonode.git

# Update the refs
git fetch --all

# Create a new branch
git checkout -b main

# Check the status of the local files and add all of them to the commit
git status
git add -A
git status

# Create a new commit along with a message
git commit -a -m " - My GeoNode"

# Finally push to the repository
git push afabiani main
```



#### [Next Section: Put geonode-project in Production](GEONODE_PROJ_PROD.md)
