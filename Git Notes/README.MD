# How to move a repository from Bitbucket to Github


The steps consists of:

1. Setup an empty Github repository as an upsteam remote to your current Bitbucket repository
2. Push your branches to the new repository
3. Clean up the old repository
   
   
   The detailed steps can be found in the referenced article.
   Referenced article link: [How to Move a Private Repository from Bitbucket to Github](https://medium.com/collaborne-engineering/how-to-migrate-a-private-repository-from-bitbucket-to-github-6cddedd5d73)

--

# To delete all local branches except main


git branch | grep -v "main" | xargs git branch -D


Source: [Stackoverflow - Delete all local Git branches](https://stackoverflow.com/a/26152200)
