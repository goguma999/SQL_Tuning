# MySQL 완전 삭제 후 재설치하기(MacOS)
## MySQL 프로세스 확인
(homebrew로 설치했을 경우)<br>
```
brew services list
brew services stop mysql
```
(다운로드 파일로 설치했을 경우)<br>
```
시스템 환경설정 -> MySQL
"STOP MySQL Server"
```
## MySQL 삭제하기
설치 경로 확인하기<br>
```
which mysql
/opt/anaconda3/bin/mysql
```
homebrew로 삭제하기<br>
```
brew uninstall --ignore-dependencies --force mysql@8.0
```
관련 파일 삭제하기<br>
```
sudo rm -rf /usr/local/mysql
sudo rm -rf /usr/local/bin/mysql
sudo rm -rf /usr/local/var/mysql
sudo rm -rf /usr/local/Cellar/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /tmp/mysql.sock.lock
sudo rm -rf /tmp/mysqlx.sock.lock
sudo rm -rf /tmp/mysql.sock
sudo rm -rf /tmp/mysqlx.sock
sudo rm ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
```
Homebrew 캐시 제거<br>
```
brew cleanup mysql
brew cleanup --prune=all
```
완료 후 컴퓨터를 재부팅한다.<br>
## homebrew로 재설치하기
```
brew install mysql
mysql --version
brew services start mysql
brew services list
```
## 초기 설정
```
mysql_secure_installation
```
1. 비밀번호 복잡도 검사<br>
VALIDATE PASSWORD COMPONENT can be used to test passwords  
and improve security. It checks the strength of password  
and allows the users to set only those passwords which are  
secure enough. Would you like to setup VALIDATE PASSWORD component?  - NO
Change the password for root ? - NO
&nbsp;
2. 익명의 사용자 삭제<br>
Remove anonymous users? - Y
&nbsp;
3. root계정 원격 접속 차단<br>
Disallow root login remotely? - Y
&nbsp;
4. Remove test database and access to it? - Y
&nbsp;
5. Reload privilege tables now? - Y
&nbsp;


