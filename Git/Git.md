## https://htmlacademy.ru/blog/git/git-console

## ![f93c1fad46d0f99cc091f756494572c9.jpg](../../_resources/f93c1fad46d0f99cc091f756494572c9.jpg)

### …or create a new repository on the command line

```
echo "# program" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:MikAn79/program.git
git push -u origin main
```

### …or push an existing repository from the command line

```
git remote add origin git@github.com:MikAn79/program.git
git branch -M main
git push -u origin main
```

## Добавить имя (введите его внутри кавычек):

`git config --global user.name "MikAn79"`

## Добавить электронную почту (замените email@example. com на вашу почту):

`git config --global user.email mikan979@gmail.com`

`git init `

инициализация репозитория