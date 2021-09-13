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

Username for 'https://github.com': afabiani
Password for 'https://afabiani@github.com': 
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


#### [Next Section: Put geonode-project in Production](GEONODE_PROJ_PROD.md)
