#Clone the forked repository using HTTPS : 
    git clone <your-fork-url>

#Change directory into the cloned repository :
    cd 2025/git/01_Git_and_Github_Basics

#Inside the cloned repository, created a new directory for this challenge :
    mkdir week-4-challenge
    cd week-4-challenge

#Initialize the directory as a new Git repository:
    git init

#Stage the file :
    git add info.txt

#Commit the file with a descriptive message:    
    git commit -m "Initial commit: Add info.txt with introductory content"

#Configure Remote URL with PAT and Push/Pull
    git remote add origin https://<your-username>:<your-PAT>@github.com/<your-username>/90DaysOfDevOps.git
    
#If a remote named origin already exists, update it with:
    git remote set-url origin https://<your-username>:<your-PAT>@github.com/<your-username>/90DaysOfDevOps.git

#Push your current branch (typically main) and set the upstream:
    git push -u origin main
    
#Verify your configuration by pulling changes:
    git pull origin main

#Check your commit history using:
        git log

#Create a branch called feature-update:
    git branch feature-update

#Alternatively, you can use:
    git checkout feature-update

