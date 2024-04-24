## Laboratory work II

Данная лабораторная работа посвещена изучению систем контроля версий на примере **Git**.

```bash
$ open https://git-scm.com
```

## Tasks

- [ ] 1. Создать публичный репозиторий с названием **lab02** и с лиценцией **MIT**
- [ ] 2. Сгенирировать токен для доступа к сервису **GitHub** с правами **repo**
- [ ] 3. Ознакомиться со ссылками учебного материала
- [ ] 4. Выполнить инструкцию учебного материала
- [ ] 5. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
# Инициализируем переменные
$ export GITHUB_USERNAME=<имя_пользователя>
$ export GITHUB_EMAIL=<адрес_почтового_ящика>
$ export GITHUB_TOKEN=<сгенирированный_токен>
$ alias edit=<nano|vi|vim|subl>
```

```ShellSession
# Выполнение скрипта activate
$ cd ${GITHUB_USERNAME}/workspace
$ source scripts/activate
```

```ShellSession
# Создание файла .config и запись в него
$ mkdir ~/.config
$ cat > ~/.config/hub <<EOF
github.com:
- user: ${GITHUB_USERNAME}
  oauth_token: ${GITHUB_TOKEN}
  protocol: https
EOF
$ git config --global hub.protocol https
```

```ShellSession
# Создание папки lab02 и переход в нее
$ mkdir projects/lab02 && cd projects/lab02
# Инициализация пустого репозитория
$ git init
# Установка имени и email
$ git config --global user.name ${GITHUB_USERNAME}
$ git config --global user.email ${GITHUB_EMAIL}
# check your git global settings
$ git config -e --global
# Добавление удаленного репозитория
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02.git
# Сливание данных из удаленной ветки в текущую
$ git pull origin master
# Создание файла
$ touch README.md
# Вывод измененных данных
$ git status
# Добавление README.md
$ git add README.md
# Создание коммита
$ git commit -m"added README.md"
# Отправка изменений в главный репозиторий
$ git push origin master
```

Добавить на сервисе **GitHub** в репозитории **lab02** файл **.gitignore**
со следующем содержимом:

```ShellSession
*build*/
*install*/
*.swp
.idea/
```

```ShellSession
$ git pull origin master
# Просмотр истории коммитов
$ git log
```

```ShellSession
# Создание папок и запись в них новых файлов
$ mkdir sources
$ mkdir include
$ mkdir examples
$ cat > sources/print.cpp <<EOF
#include <print.hpp>

void print(const std::string& text, std::ostream& out)
{
  out << text;
}

void print(const std::string& text, std::ofstream& out)
{
  out << text;
}
EOF
```

```ShellSession
$ cat > include/print.hpp <<EOF
#include <fstream>
#include <iostream>
#include <string>

void print(const std::string& text, std::ofstream& out);
void print(const std::string& text, std::ostream& out = std::cout);
EOF
```

```ShellSession
$ cat > examples/example1.cpp <<EOF
#include <print.hpp>

int main(int argc, char** argv)
{
  print("hello");
}
EOF
```

```ShellSession
$ cat > examples/example2.cpp <<EOF
#include <print.hpp>

#include <fstream>

int main(int argc, char** argv)
{
  std::ofstream file("log.txt");
  print(std::string("hello"), file);
}
EOF
```

```ShellSession
$ edit README.md
```

```ShellSession
$ git status
$ git add .
$ git commit -m"added sources"
$ git push origin master
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=02
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER}.git tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

### Part I

1. Создайте пустой репозиторий на сервисе github.com (или gitlab.com, или bitbucket.com).
```
# Создание папки lab02homework и переход в нее
$ mkdir projects/lab02homework && cd projects/lab02homework
# Инициализация пустого репозитория
$ git init
# Добавление удаленного репозитория
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab02homework.git
# Сливание данных из удаленной ветки в текущую
$ git pull origin master
```
2. Выполните инструкцию по созданию первого коммита на странице репозитория, созданного на предыдещем шаге.
3. Создайте файл `hello_world.cpp` в локальной копии репозитория (который должен был появиться на шаге 2). Реализуйте программу **Hello world** на языке C++ используя плохой стиль кода. Например, после заголовочных файлов вставьте строку `using namespace std;`.
```
$ touch hello_world.cpp
$ cat > hello_world.cpp <<EOF
> #include <iostream>
> 
> using namespace std;
> 
> int main() {
>     std::cout << "hello world!";
> }
> EOF
```
4. Добавьте этот файл в локальную копию репозитория.
```
$ git add hello_world.cpp
```
5. Закоммитьте изменения с *осмысленным* сообщением.
```
$ git commit -m"added hello_world.cpp"
[master 0bca908] added hello_world.cpp
 1 file changed, 7 insertions(+)
 create mode 100644 hello_world.cpp
$ git push origin
```
6. Изменитьте исходный код так, чтобы программа через стандартный поток ввода запрашивалось имя пользователя. А в стандартный поток вывода печаталось сообщение `Hello world from @name`, где `@name` имя пользователя.
```
edit hello_world.cpp
```
7. Закоммитьте новую версию программы. Почему не надо добавлять файл повторно `git add`?
```
$ git commit -m"change hello_world.cpp"
На ветке master
Изменения, которые не в индексе для коммита:
	изменено:      hello_world.cpp

нет изменений добавленных для коммита
$ git add hello_world.cpp 
$ git commit -m"change hello_world.cpp"
[master 2272e89] change hello_world.cpp
 1 file changed, 9 insertions(+), 1 deletion(-)
```
8. Запуште изменения в удалёный репозиторий.
```
$ git push origin master
```
9. Проверьте, что история коммитов доступна в удалёный репозитории.
```
$ git log
```

### Part II

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. В локальной копии репозитория создайте локальную ветку `patch1`.
```
$ git checkout -b patch1
Переключено на новую ветку «patch1»
```
2. Внесите изменения в ветке `patch1` по исправлению кода и избавления от `using namespace std;`.
```
$ edit hello_world.cpp 
```
3. **commit**, **push** локальную ветку в удалённый репозиторий.
```
$ git add hello_world.cpp 
$ git commit -m"change hello_world.cpp"
[patch1 0bcb31d] change hello_world.cpp
 1 file changed, 3 insertions(+), 4 deletions(-)
$ git push origin patch1
```
4. Проверьте, что ветка `patch1` доступна в удалёный репозитории.
5. Создайте pull-request `patch1 -> master`.
6. В локальной копии в ветке `patch1` добавьте в исходный код комментарии.
7. **commit**, **push**.
8. Проверьте, что новые изменения есть в созданном на **шаге 5** pull-request
9. В удалённый репозитории выполните  слияние PR `patch1 -> master` и удалите ветку `patch1` в удаленном репозитории.
10. Локально выполните **pull**.
```
$ git pull
Ваша конфигурация указывает, что нужно слить изменения со ссылкой
«refs/heads/patch1» из внешнего репозитория, но такая ссылка не была получена.
$ git checkout master
$ git pull
```
11. С помощью команды **git log** просмотрите историю в локальной версии ветки `master`.
```
$ git log master
```
12. Удалите локальную ветку `patch1`.
```
$ git branch -d patch1
```

### Part III

**Note:** *Работать продолжайте с теми же репоззиториями, что и в первой части задания.*
1. Создайте новую локальную ветку `patch2`.
```
git checkout -b patch2
Переключено на новую ветку «patch2»
```
2. Измените *code style* с помощью утилиты [**clang-format**](http://clang.llvm.org/docs/ClangFormat.html). Например, используя опцию `-style=Mozilla`.
```
$ clang-format -i -style=Mozilla hello_world.cpp
```
3. **commit**, **push**, создайте pull-request `patch2 -> master`.
```
$ git add .
$ git commit -m"changed codestyle in hello_world.cpp"
$ git push origin patch2
```
4. В ветке **master** в удаленном репозитории измените комментарии, например, расставьте знаки препинания, переведите комментарии на другой язык.
```
$ git checkout master
$ edit hello_world.cpp
```
5. Убедитесь, что в pull-request появились *конфликтны*.
6. Для этого локально выполните **pull** + **rebase** (точную последовательность команд, следует узнать самостоятельно). **Исправьте конфликты**.
```
$ git pull
$ edit hello_world.cpp
$ git add .
$ git commit -m"fix"
$ git rebase master
$ edit hello_world.cpp
$ git add .
$ git rebase --continue
$ git pull origin master
```
7. Сделайте *force push* в ветку `patch2`
```
$ git push -f origin patch2
```
8. Убедитель, что в pull-request пропали конфликтны. 
9. Вмержите pull-request `patch2 -> master`.

## Links

- [hub](https://hub.github.com/)
- [GitHub](https://github.com)
- [Bitbucket](https://bitbucket.org)
- [Gitlab](https://about.gitlab.com)
- [LearnGitBranching](http://learngitbranching.js.org/)

```
Copyright (c) 2015-2019 The ISC Authors
```
