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

#### [Next Section: Put geonode-project in Production](GEONODE_PROJ_PROD.md)
