# Git 원격 모든 브랜치 Clone

1. 프로젝트 Clone
```git
git clone {git주소}
cd {프로젝트}
```

2. 로컬 브랜치 확인
```bash
git branch

# *master
```

3. 숨겨진 브랜치 확인
```bash
git branch -a

# master
# develop~
```

4. 원격에 있는 모든 브랜치를 가져오기
```bash
git config --global alias.clone-branches '! git branch -a | sed -n "/\/HEAD /d; /\/master$/d; /remotes/p;" | xargs -L1 git checkout -t'
```

5. 만들어진 alias 를 사용
```bash
git clone-branches
```
