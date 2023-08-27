# привет! это шпаргалка по гиту
## здесь я соберу дополнение к первой шпаргалке из закрепа и в принципе пошаговые приколы
---
начинаем с создания репозитория и немного о том как его изменять: 
1. создаем папку ```mkdir <имя папки>``` и переходим в  нее
2. инициализируем папку через ```git init``` 
3. создаем внутри файл через ```touch <имя файла>```
4. и добавляем его на "сцену" с помощью ```git add <имя файла>```
5. редактируем его в любой проге (блокнот, вскод и тд)
6. проверяем состояние с помощью ```git status``` должно быть ```modified``` снова прописываем ```git add <...>``` и коммитим изменения с помощью ```git commit -m "место для описания"```
7. чтобы это все отобразилось на самом гитхабе также прописываем ```git push```

### но вообще......
перед тем как коммитить и что-то делать с файлом и его отображением на гитхабе нужно привязать репозиторий на компьютере к репозиторию на гитхабе
### как это сделать?
1. создадим ssh ключ!
#### важно! 
надо проверить его наличие. а это как сделать? записывайте рецепт!  
```$cd ~``` перешли в домашнюю директорию  
```ls -la .ssh/``` посмотрели есть ли ключи  
если есть штуки с .pub, то ключи есть, если не создавали их сами, то нужно снести и  

##### начнем же!
вводим ```$ ssh-keygen -t ed25519 -C "электронная почта, к которой привязан ваш аккаунт на GitHub"```   
терминал скажет сгенерированы ключи  
нажимаем ввод  
пункт с кодовой фразой можно пропустить, это лишняя запара  
ну вот и все!  
проверяем: ```ls -a ~/.ssh```   
должно быть два файла: один с .pub, другой без  
япи!

2. пора привязать ssh ключ к гитхабу!  

что нам нужно?  
скопировать содержимое ключа в буфер обмена: ``` $ clip < ~/.ssh/id_rsa.pub```   
для ed25519: ``` $ clip < ~/.ssh/id_ed25519.pub```

потом переходим на сайт гитхаба  
settings > ssh and gpg keys > new ssh key > называем ключ > тип ключа - authentication key > и в поле key вставляем все что скопировалось > нажимаем add ssh key  
проверим правильность: 
```$ ssh -T git@github.com```  
переходим на эту [ссылку](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints)  
если совпало вводим "yes"  
ура победа!  

## вторая практическая работа и вторая часть шпаргалки

### начнем с хэшей

#### что это?  

это набор цифр и букв возле слова commit при выведении логов с помощью ```git log```  
в общем и целом это описание коммита, но закодированное в забавный и непонятный набор символов  

что забавно: 
* если хеш получить дважды для одного и того же набора входных данных, то результат будет гарантированно одинаковый;
* если хоть что-то в исходных данных поменяется (хотя бы один символ), то хеш тоже изменится (причём сильно).

### рассмотрим в принципе составляющие логов

#### вот они сверху вниз
* сам коммит с его хэшем
* автор коммита
* дата и время коммита
* сообщение коммита

#### но хэши очень большие, как найти нужный коммит если их уже больше сотни?
используем сокращенные логи!  
```git log --oneline```  
тадам! у нас коротенькие строчки с сокращенным хэшем и сообщением коммита

**причем!** короткие хэши можно использовать точно также как и полные!

### немного про жизненный цикл файла

```mermaid 
graph TD;
    untracked -- "git add" --> staged;
    staged -- "git commit -m" --> tracked/comitted;
    tracked/comitted -- "git push" --> seen on github;
```