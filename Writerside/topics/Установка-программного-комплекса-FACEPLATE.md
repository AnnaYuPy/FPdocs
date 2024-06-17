# Установка программного комплекса FACEPLATE

При установке FACEPLATE на операционные системы семейства Linux может возникнуть ошибка следующего вида:

```bash
/faceplate/bin/faceplate: error while loading shared libraries: libtinfo.so.5: cannot open shared object file: No such file or directory
```
Эта проблема вызвана отсутствием библиотеки libtinfo.so.5 в системе. Для решения этой проблемы выполните соответствующие команды в зависимости от используемого дистрибутива Linux:

- SUSE: Выполните команду (используйте sudo, только если установка выполняется от имени root):
```bash
sudo zypper install libncurses5
```
- Fedora: Выполните команду (используйте sudo, только если установка выполняется от имени root):
```bash
sudo yum install ncurses-compat-libs
```
- Ubuntu: Выполните команду (используйте sudo, только если установка выполняется от имени root):
```bash
sudo apt-get install libncurses5
```
Эти команды установят необходимые библиотеки и позволят продолжить установку FACEPLATE без ошибок.





