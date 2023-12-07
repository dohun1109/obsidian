``` cli
	pwd : 현재 폴더의 위치를 확인 
	ls -a : 현재 폴더의 파일 목록을 확인합니다. (-a 옵션을 이용해 숨긴 파일도 볼 수 있다. )
	cd : home directory로 이동 
	cd < 폴더 이름> : 특정 위치의 폴더로 이동 
	cd ../ : 현재 폴더의 상위 폴더로 이동 
	mkdir <폴더 이름> : 현재 폴더의 아래에 새로운 폴더 생성 
	echo "String" : 화면에 ""안에 문자을 표시 
	echo "" > file.txt : file.txt 생성하고 파일내에 텍스트 작성
	'' >> '' : 작성된 곳 아래에 내용 추가 
	cat [파일명] : 파일 내용 뿌리기 
```

git init -b main -> main 브랜치 초기화 및 git 저장소 생성 

언스테이징 
git reset [파일명]


## git log 

git log -n<숫자> 
git log --oneline --graph --all --decorate 
--oneline : 한줄로 
--graph : 그래프 
--all : HEAD와 관계없는것도 다보여줌 
--decorate : 브랜치와 태그 참조를 간결하게 