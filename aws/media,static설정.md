# media설정

	sudo vi /etc/nginx/sites-available/app
	
```
location /media/{
	alias /srv/app/media/;
}
```

** 이미지 업로드시 Permission Denied 시 **
`sudo chmod 777 media`로 미디어 폴더의 권환을 바꿔준다

## static
`sudo vi /etc/nginsx/sites-available/app`
```
location /static/{
	alias /srv/app/static_root/;
}
```

##데이터베이스 삭제

**삭제**
`sudo -u postgres dropdb <db name>`

**생성**
`sudo -u postgres createuser -s -P <username>`

