---
layout: post
title: Выполнение git скриптов под агентом TFS
---
Иногда необходимо выполнять git скрипты на билд агенте TFS. 
<details>
  <summary>Пример</summary>
  На одном проекте на билд агенте происходил билд react бандла c последующим коммитом и пушем.
</details>  

На сайте microsoft.com есть вводная [статья](https://docs.microsoft.com/en-us/vsts/build-release/actions/scripts/git-commands?view=vsts), 
раскаживающая что и как делать.  
Перечислю основные моменты:
* Необходимо разрешить билд агенту комитить в ветку, для этого надо дать права `Contribute: Allow`
* Разрешить OAuth token для скриптов на вкладке опций
* Чтобы git не ругался при комитах что нет имени пользователя и email, необходимо комитить следующей командой:  
```
git -c "user.name=Build Agent" -c "user.email=build-agent@example.ru" commit -m "<commit message>"
```
* Команды `pull`, `push` и т.п. выполнять с ключом `-c http.extraheader`:  
```
git -c http.extraheader="AUTHORIZATION: bearer $(System.AccessToken)" pull
```
где `System.AccessToken` переменная TFS
