# MySQL
#Database/MySQL
루트 패스워드: `{{2326}}`

기존에 설치해두었던 mysql이 제대로 동작하지 않아 아래의 가이드를 보고 삭제 및 재설치를 진행했다.
[배우신 분의 친절한 가이드](https://github.com/appkr/l5code/issues/4)

위의 방법도 잠시 경계할 필요가 있다. root의 비밀번호를 커스텀화 해서 변경해주는 작업을 했는데, 그냥 그대로 두는 것이 제일 좋은 방법 같다. 왜냐하면 본인이 [Cannot connect to Database server (mysql workbench) - Stack Overflow](https://stackoverflow.com/questions/7864276/cannot-connect-to-database-server-mysql-workbench)의 문제를 다시 겪었으며 `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';` 로 해결했기 때문이다.

결국 오류를 잡고 잡는 것의 결과는 재설치 및 비밀번호를 `password` 로 설정하는 것이었다. 배우신 분의 친절한 가이드는 재설치에 한해서만 참고하도록 하고 아래 부분에 나오는 계정에 대한 설명은 좀 더 심사숙고할 필요가 있다.

### Using home-brew
동작 중인 서버에 접속하는 방법이다. `mysql -u root -p -h 127.0.0.1 -P 3`
[Starting and Stopping Background Services with Homebrew](https://robots.thoughtbot.com/starting-and-stopping-background-services-with-homebrew) 게시물을 참조한다.


### start and stop MySQL server
**start**:
`sudo mysql.server start`
server Password: waca2326040!
root Password: Waca2326040!

**stop**:
`sudo mysql.server stop`

`mysql> quit` 명령어로 콘솔 내에서 탈출할 수 있다.

**restart**:
`sudo mysql.server restart`

**login**:
`mysql -u root -p`


