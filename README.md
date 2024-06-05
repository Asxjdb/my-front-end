# my-front-end

使用github action自动部署 
[https://blog.arisa.moe/blog/2022/220407-github-pages](https://blog.arisa.moe/blog/2022/220407-github-pages)

bug: `remote: Permission to Asxjdb/my-front-end.git denied to github-actions[bot].`

解决：添加
```yml
    permissions:
      contents: write
```

[https://stackoverflow.com/questions/72851548/permission-denied-to-github-actionsbot](https://stackoverflow.com/questions/72851548/permission-denied-to-github-actionsbot)

