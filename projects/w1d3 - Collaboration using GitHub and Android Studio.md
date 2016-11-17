![Makers Institute](../images/logo-makersinstitute.png)

# Collaboration using GitHub and Android Studio

## Objective
Practice GitHub Fork and Pull Request using Android Studio

In this project you will Fork some remote repository to your remote repository. Then doing some edits using Android Studio and Push it back to your remote repository. After that, make a pull request to the other remote repository. If there is a merge conflict, you have to solve it.

</br>

### First: Fork a Repository
1. Open page [makersinstitute/MyFirstApp](https://github.com/makersinstitute/MyFirstApp). 
2. Fork that repository to your GitHub repository.

</br>

### Second: Open in Android Studio
1. Open Android Studio
2. Instead of creating a new project, choose *Checkout project from Version Control*.
3. Select **GitHub**, then login.
4. Copy the clone link from your forked repository, and paste it.
5. Select an appropriate folder location, and press ***Clone***
6. Open the Project.

</br>

### Third: Create a New Branch then edit some file
1. Open *VCS* menu, *Git* -> *Branches...*, then `+ New Branch`.
2. Give it a name.
3. Browse the Project File.
4. Open *activity_main.xml* under `res/layout/` directory 
5. Edit **TextView's** text from `"Hello World!"` to `"Hello <your first name>!"`
6. Commit the edit. 

### Fourth: Back to Master Branch, then edit the same file
1. Open *VCS* menu, *Git* -> *Branches...*, then under the local branch, *checkout* your Master Branch.
2. Browse the Project File
3. Open *activity_main.xml* under `res/layout/` directory 
4. Edit **TextView's** text from `"Hello World!"` to `"Hello <your last name>!"`
5. Commit the edit.

</br>

### Fifth: Merge it and resolve if it conflicted (it should be conflicted)
1. Make sure you are on Master Branch.
2. Open *VCS* menu, *Git* -> *Merge Changes...*
3. Choose branch to be merge with, then tap ***Merge***
4. You will see a new window that telling you have to resolve conflict. Tap ***Merge...***
5. Choose the version you want, make sure you see the green pop-up telling you that the merge is ok, then tap ***Apply***.
6. Commit the merge using Android Studio's made commit message.

</br>

### Sixth: Push to GitHub
1. Open *VCS* menu, *Git* -> *Push*
2. Everything should be ok now, then push to your Remote Repository.
2. Make sure it's successly pushed. Congratulations!

</br>

### Seventh: Pull Request
1. Let's contribute to the original repository.
2. Open *VCS* menu, *Git* -> *Create Pull Request*
3. Choose the original remote repository on *Base fork*
4. Give a title, and maybe descriptions.
5. Finally, tap `OK`.

</br>


# Congrats!
