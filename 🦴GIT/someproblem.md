

# 关于全局配置设置
```
git config global user.name ""
git config global user.email ""
```
# 关于解决gitignore的配置问题
 ```
git rm -r --cached . // 删除本地缓存 
git add . // 添加要提交的文件 
git commit -m "update .gitignore" // 更新本地的缓存 
git config core.excludesfile .gitignore //让配置文件生效 

//那个.gitignore文件可以用编辑器打开 

*.obsidian //即忽略该文件
*.trash //这个是obsidian自带的垃圾桶 也不要推上去

 ```
# 基本提交流程
```
git init
git reset
git add --all 
git status //just for check
git commit -m '' //the 'm' means message
git push origin master || git push origin -u 'master'  
```
# 远程库的管理
```
git remote -v //just for check 
git remote add origin url 
//'url' ex:https://github.com/odddouglas/odddouglas-s-repository.git
```