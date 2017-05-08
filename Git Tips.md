# Git Tips

## Export Git logs

Add a git command, so we do not need to re-type all this options each time. To do this edit **.git/config** file in your repo and add:

```txt
[alias]
    report = rev-list --oneline --pretty=format:'%cd | - %s%d' --graph --date=format:'%Y-%m-%d %H:%M' --all
```

Now you can obtains report just typing:

```txt
git report
```
