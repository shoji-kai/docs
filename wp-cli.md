# wp-cli

## WP-CLIのインストール

```
% curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
% php wp-cli.phar --info
% chmod +x wp-cli.phar
% sudo mv wp-cli.phar /usr/local/bin/wp
```

## タブ補完を有効にする

```
% curl -LO https://github.com/wp-cli/wp-cli/raw/master/utils/wp-completion.bash
% autload -U +X bashcompinit && bashcompinit
% source wp-completion.bash
```

## WordPressのダウンロード

```
% sudo chown -R vagrant. /var/www/html
% wp core download locale=ja --path=/var/www/html/blog
Creating directory '/var/www/html/blog/'.
Downloading WordPress 4.9.4 (ja)...
Using cached file '/home/vagrant/.wp-cli/cache/core/wordpress-4.9.4-ja.tar.gz'...
Success: WordPress downloaded.
```

## MySQLの設定

1. 文字コードの変更

```
% sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
[mysqld]
:
character-set-server = utf8mb4
% sudo vim /etc/mysql/conf.d/mysql.cnf
[mysql]
default-character-set = utf8mb4
% sudo systemctl restart mysql
% mysql -uroot -p -e 'show variables like "%char%"'
Enter password:
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8mb4                    |
| character_set_connection | utf8mb4                    |
| character_set_database   | utf8mb4                    |
| character_set_filesystem | binary                     |
| character_set_results    | utf8mb4                    |
| character_set_server     | utf8mb4                    |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
```

2. WordPress用データーベース、ユーザの作成

```
% mysql -uroot -p
mysql> create database wordpress;
mysql> grant all on wordpress.* to 'wordpress'@'localhost' identified by 'wordpress';
mysql> grant all on wordpress.* to 'wordpress'@'127.0.0.1' identified by 'wordpress';
mysql> flush privileges;
```

## wp-config.phpの生成 

```
% wp core config --dbhost=localhost --dbname=wordpress --dbuser=wordpress --dbpass=wordpress
```

## WordPress用テーブルの作成
```
% wp core install --url=vm-wp-cli.local/blog --title="WP-CLI" --admin_user=admin --admin_password=admin --admin_email=sho2kai@gmail.com
```
