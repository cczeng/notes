# Change files - 查看文件更改  

## 查看某个文件的修改历史  

`git log -- [filename]`  

```
git log -- index.html
```  
上面会罗列出所有的修改历史,可以使用`git show`来查看   

```
git show eba08e3328afd26cbf6d92efb26fa9b83d47c2b1
// 或者使用 git show --stat 显示差异统计
git show --stat eba08e3328afd26cbf6d92efb26fa9b83d47c2b1
```  

如果要查看这次提交中某个文件的修改只需要后面加 `-- [path]` 参数:   


```
git show eba08e3328afd26cbf6d92efb26fa9b83d47c2b1 -- src/app/home/home.component.html
```


## 查看最近几次的文件更改记录  

```
git diff --name-only SHA1 SHA2
```  
也可以直接查看最新x次提交的更改:

```
git diff --name-only HEAD~10 HEAD~5
```

### 查看更改记录并显示差异统计  

使用: `--stat` 参数

```
git diff --stat HEAD~10 HEAD~5
```  
  
以及: `--numstat`

```
git diff --numstat HEAD~10 HEAD~5
```  

以及: `--shortstat`  

```
git diff --shortstat HEAD~10 HEAD~5
```

## 查看过去x天内文件更改记录  

```
git log --pretty=format: --name-only --since="2 days ago"
```

上面的方法会显示重复的文件更改,可以使用下面带有管道的方法:   

```
git log --pretty=format: --name-only --since="2 days ago" | sort | uniq
```  
