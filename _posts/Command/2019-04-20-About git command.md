---
title: "Git 관련 Usefule Command"
categories: Command 
---

## Git Upstream 설정
주로 Contribute을 할 때 Fork을 하고 Contribute을 하고 있는 도중에 origin 저장소가 업데이트 된 경우 Upstream을 설정하여 업데이트된 내용을 가져올 때 사용합니다.
즉, origin 저장소와 내가 fork한 저장소 간의 동기화를 위해 사용합니다.

origin 저장소를 upstream 저장소로 설정합니다. 일단 remote 저장소를 확인하기 위해 다음 명령어를 사용합니다.

`git remote -v`

저장소를 확인하고 다음과 같은 명령어를 사용하여 origin 저장소를 upstream에 추가합니다.

`git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git`

이제 **fork한 저장소(내 저장소)와 origin 저장소를 merge합니다.**
다음 명령어를 실행하면 위에서 추가한 upstream 저장소를 fetch합니다.

`git fetch upstream`

다음 명령어를 사용하여 fork한 저장소의 master branch로 이동한 후 upstream의 master와 merge한다.
따로 branch을 건드리지 않는 이상 master 상태에 있습니다.

`git checkout master`

`git merge upstream/master`


## Git Local config 설정
Server 컴퓨터를 사용할 때 하나의 User로 다수가 git을 이용하는 경우 특정 프로젝트 내에서 다음과 같이 설정하여
특정 프로젝트 내에서만정 내 git 계정을 사용할 수 있습니다.

즉, --global로 설정하는 경우 모든 프로젝트에 영향을 미치므로 local로 설정하는 방법입니다.

`git config --local user.name "Eon Kim"`

`git config --local user.email kimiron518@gmail.com`

```
    git config --local --list
    user.email=kimiron518@gmail.com
    user.name=Eon Kim
```