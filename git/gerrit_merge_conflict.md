# gerrit merge conflict

1.
````
git pull origin master
````
or
````
git fetch origin master
````
2.
````
git checkout [review]
````
3.
````
git status
````
4.
````
git rebase master
````
5. fix conflicts

6.
````
git add *
git rebase --continue
````

7.
````
git push origin [checkout review]
````
