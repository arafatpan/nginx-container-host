### 3.0 Submitting and Push to GitHub.com

In order to submit this assignment, you will create your own Git repository to store your Bootstrap pages and your Dockerfile. This should be all that I will need to pull your project, do a Docker build and view your pages being served. You should test this all the way through and make sure that your image works as expected and does not have broken links.

Take the screenshots you took along the way and place them in a folder in the project--call that folder "assignment-images" at a sibling-level to the web 'images' folder.

Your first step is to create a new repository on GitHub.com. My recommendation is to not create any license files or even the README.md file. It will make your first use of a remote Git repository a bit simpler to get synchronized with your local repository. In order for me to access it, **your repository should be public or private with myself added as a collaborator**. You can do it either way.

After you do this, you will need to convert your local folder that contains your webpages and Dockerfile into a Git repository.

To finish it off, the following commands worked for me. You will need to initialize the directory, add all of your files in your current directory to the repo, commit the changes and finally push it to the remote repo.

Run something like these in your terminal:

```
kevin@Elitebook-14:~/nginx-hosting$  echo "keschae nginx hosting assignment" >> README.md
kevin@Elitebook-14:~/nginx-hosting$ git init
Initialized empty Git repository in /home/kevin/nginx-hosting/.git/
kevin@Elitebook-14:~/nginx-hosting$ git add -A
kevin@Elitebook-14:~/nginx-hosting$ git commit -m "added all files"
kevin@Elitebook-14:~/nginx-hosting$ git remote add origin https://github.com/keschae/nginx-hosting.git

kevin@Elitebook-14:~/nginx-hosting$ git push -u origin master
Username for 'https://github.com': keschae@ilstu.edu
```

Your final screen capture should show the initialization of the Git repo and setting of the remote repository.

![grab screen snip here](./images/sm-camera.png)
