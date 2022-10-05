# Git Objects
Git is a content-addressable filesystem. It means that at the core of Git is a simple key-value data store. What this means is that you can insert any kind of content into a Git repository, for which Git will hand you back a unique key you can use later to retrieve that content.

As a demonstration, let’s look at the plumbing command `git hash-object`, which takes some data, stores it in your `.git/objects` directory (the object database), and gives you back the unique key that now refers to that data object.

```bash
$ echo 'test content' | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4
```


Blob - это предстваление файлов в файловой системе
Tree - это представление каталогов файловой системы

