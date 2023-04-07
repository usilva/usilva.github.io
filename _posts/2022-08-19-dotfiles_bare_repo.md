---
title: dotfiles repo
date: 2022-08-19 00:00:00 -000
categories: [dotfiles,configurations]
tags: [desktop,dotfiles,configurations]
---


# **Use of dotfiles versioned as bare repository**

 If you doesn't know yet, dotfiles is a common name used to refer to the configuration files that are named starting with dot. aka .file_name this starts at unix_likes systems, and now days is used at Windows to.

 The dotfiles pattern came to be popular since users starts to save this files at version controls programs and share this repositories with community. Is an usual way to do this using symbolic links, so you create a symlink pointing to real file at your dotfiles repository, at anywhere you need to use it. This way you can save dotfiles folder at version control share and use it.
 
 But, create and maintain this symlinks are a hard task. So Nicola Paolucci made what i think is a better way to handle with this. You can check this at [his article][original-source]. Since i found his article istart to use this way. But now i need to do this at windows to so this is the reason i doing this.

 ## **How this works?**
 1. [Created a git bare repository.](#create-an-bare-repository)
    1. [Set to not show untracked files.](#select-what-is-tracked)
 2. [Create a command alias.](#create-an-alias-to-handle-with-this-repository)
 3. [Versioing your bare repository](#versioning-a-bare-repository)
 4. [Keeping the repository up to date](#keep-repository-up-to-date)
 5. [Restore the files from repository](#how-to-restore-my-saved-dotfiles)
    1. [Clone the repo](#clone-the-repo)
    2. [Create your alias](#create-an-alias-to-handle-with-this-repository)
    3. [Restore the files](#restore-the-files)
 6. [For multiplataform use one branch for each SO(Windows, Linux, Mac)](#make-it-multiplataform-using-branchs).
 
 ## Extras
 1. [How to list files tracked by this repository](#how-to-list-files-tracked-by-this-repository)
 2. [Multiplataform endlining](#multiplataform-endlining)

---

# **How to configure this (Step-by-step)**

 ## **Create an bare repository**
 > **WARNING**: If you plan to use it multiplataform(win, mac and linux), create the repository as discribed at this section than read [multiplataform section](#make-it-multiplataform-using-branchs) and create branchs before proceed.

 Create a folder to be the house of your dotfiles. *personaly i use .git when using a bare repo, is a habit and listed at git-scm as best practice.

 ```ps
 mkdir $HOME\.dotfiles.git
 cd $HOME\.dotfiles.git
 git init --bare 
 ```
 
  ### **Select what is tracked**
  Set repository to not show untracked files. 
  > **WARNING**: will need to add manualy all files you want to track. 
  ```ps
  git config --local status.showUntrackedFiles no
  ```

 ## **Create an alias to handle with this repository**
 > **TIP**: This alias makes possible to use it from anywhere at your file system. 
 
 Now configure an alias/function(windows) to a git command using $HOME as work tree and bare repo as git-dir, you name this alias as you want. This make easier to version control at this repo, you can simple use your alias instead git plus git-dir and work-tree parameters. For example, to get git repo status:
 
 Will use:
  ```ps
 <your_alias> status
 ```

 Instead:
 ```ps
 git --git-dir=<bare_repo_path> --work-tree=<work_tree_directory> status
 ```

  ### How to create
  At original article alias is named as 'config' but here i'll name as dotfiles.
  ### on Windows
  ```ps
  Function <your_alias> {git --git-dir=$HOME\.dotfiles.git\ --work-tree=$HOME $args}
  ```
  add this to your profile, and make this command permanent
  ```sh
  echo "Function <your_alias> {git --git-dir=$HOME\.dotfiles.git\ --work-tree=$HOME $args}" >> $profile
  ```

  ### on Unix-like
  ```sh
  alias <your_alias>='/usr/bin/git --git-dir=$HOME/.dotfiles.git/ --work-tree=$HOME'
  ```
  add this to your shell resource, and make this command permanent
  ```sh
  echo "alias <your_alias>='/usr/bin/git --git-dir=$HOME/.dotfiles.git/ --work-tree=$HOME'" >> .bashrc
  ```

 ## **Versioning a bare repository**
 Maybe yout want to save this files at your favorite git service(gitLab, github), so how to do it?
 ```ps
 <your_alias> remote add origin <your_service_URL>
 <your_alias> push origin
 ```
 
 ## **Keep repository up to date**
 Since the alias is configured, you can handle it using any normal git command and options. Only needed to use your **<your_alias>** instead of git, use it like bellow:

  ```ps
  <your_alias> add $HOME\.gitconfig
  ```  
  ```ps
  <your_alias> commit -m "adding my global git configuration file -> .gitconfig"
  ```
  ```ps
  <your_alias> status
  ```
  ```ps
  <your_alias> diff
  ```
  ```ps
  <your_alias> restore <file_name>
  ```

  ### **Sample: add vscode settings file**
  ### Windows
  This folder contains files that can be only used at Windows. Use %APP_DATA%\Code\User\settings.json at explorer to check your directory.
  ```ps
  <your_alias> add $HOME\AppData\Roaming\Code\User\settings.json
  ```
  ### Mac
  This folder contains files that can be only used at Mac.
  ```ps
  <your_alias> add $HOME/Library/Application\ Support/Code/User/settings.json
  ```
  ### Linux
  This folder contains files that can be only used at Linux systems.
  ```ps
  <your_alias> add $HOME/.config/Code/User/settings.json
  ```

 ## **How to restore my saved dotfiles?**
 Yeah! all god but how to restore my files? Simple:

  ### **Clone the repo**
  ```ps
  git clone --bare <your_service_URL>/.dotfiles.git $HOME/.dotfiles.git
  ```
  
  ### **Create your alias**
  Create the [alias](#create-an-alias-to-handle-with-this-repository) as described before.

  ### **Restore the files**
  List your branchs to select the correct form you.
  ```ps
  <your_alias> branch 
  ``` 
  Now is **WHEN THE MAGIC REALY HAPPENS!**. Use git checkout option to restore files from your specific operational system branch name bare repository into your work tree:
  ```ps
  <your_alias> checkout <your_operational_system>
  ```
 And now all yours dotfiles are back.

---

 ## **Make it multiplataform using branchs**
 To use this resource as multiplataform using branchs, you need to start one branch for each plataform you want to cover.
 So after create the folder/directory and init a bare repository in it, rename one branch and create the others. 

  ### Renaming one branch  
  ```ps
  git branch -m master windows
  ```
  ### Creating other branchs
  repeat this command for each plataform you wanna create.
  ```ps
  git branch <branch-name>
  ```


# **Good to know**
 
 ## **How to list files tracked by this repository?**
 Maybe you wanna check what files are at your repository tracked list, you can list using:
 ```ps
 <your_alias> ls-tree --full-tree -r --name-only HEAD
 ```
 
 ## **Multiplataform endlining**
 Since this project is to be used at Mac, Windows or Linux. Is a good call configure endlines at your source control, adopting git-scm adivise do this you could do it global.

 core.autocrlf = true for **Windows** systems 
 ```ps
 git config --global core.autocrlf = true
 ```
 core.autocrlf = input for **Unix-like** systems
 ```ps
 git config --global core.autocrlf = input
 ```

 You could use <your_alias> --local option to this only for this repository. Is up to you.

source: https://www.atlassian.com/git/tutorials/dotfiles

---

[original-source]: https://www.atlassian.com/git/tutorials/dotfiles