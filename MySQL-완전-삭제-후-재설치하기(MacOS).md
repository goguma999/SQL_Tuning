## MySQL 완전 삭제 후 재설치하기(MacOS)
### MySQL 프로세스 확인
(homebrew로 설치했을 경우)<br>
'''
brew services list
brew services stop mysql
'''
### MySQL 삭제하기
설치 경로 확인하기<br>
'''
which mysql
/opt/anaconda3/bin/mysql
'''
homebrew로 삭제하기<br>
'''
brew uninstall --ignore-dependencies --force mysql@8.0
'''
관련 파일 삭제하기<br>
'''
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
'''
Homebrew 캐시 제거<br>
'''
brew cleanup mysql
brew cleanup --prune=all
'''
완료 후 컴퓨터를 재부팅한다.<br>
### MySQL Community Server 8.0.4 버전으로 재설치
https://dev.mysql.com/downloads/mysql/ <br>
macOS14(ARM, 64-bit), DMG Archive <br> 
