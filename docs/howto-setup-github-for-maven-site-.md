# Site

Execute the following commands in the repository.

```shell
git checkout --orphan site
rm .git/index
git clean -fdx
echo ".idea" > .gitignore
echo "site" > index.html
git add . 
git commit -m "initializing site" 
git push origin site
git checkout main
```

Perform the following steps in the GitHub repository.

* go to GitHub repository > setting > pages
* in the source section
    * set branch site
    * set folder /root