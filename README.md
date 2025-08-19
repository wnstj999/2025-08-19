# 2025-08-19 수업내용 정리 

##### 브랜치 기능 테스트
브랜치 기능 테스트 2
PJH 테스트 문단입니다.

git fetch 는 원격 저장소(remote repository)에서 최신 브랜치, 태그, 커밋 정보를 가져오되, 현재 작업 중인 로컬 브랜치에는 적용하지 않는 명령어입니다.

즉, 원격 저장소의 최신 상태를 확인할 수 있게 해주지만, 내 작업 트리에 직접 반영되지는 않습니다.

주요 특징

원격 저장소의 최신 commit, branch, tag 정보를 로컬에 다운로드.

하지만 현재 작업 중인 branch(head)는 그대로 둠.

git merge 나 git pull 을 통해야만 로컬 브랜치에 반영됨.

비교

git fetch: 가져오기만 함 (내 로컬 작업에는 영향 없음).

git pull: git fetch + git merge (가져온 걸 현재 브랜치에 합침).

예시
## 원격 저장소(origin)의 모든 브랜치 가져오기
git fetch origin

## 특정 브랜치만 가져오기
git fetch origin main

## 원격 브랜치와 비교
git log HEAD..origin/main
git diff origin/main


👉 보통 협업할 때 git fetch로 먼저 원격 상태를 확인하고, 충돌 가능성을 체크한 다음 git merge 또는 git rebase로 내 브랜치에 반영하는 식으로 사용합니다.

원할 때 제가 그림으로 "fetch → merge → pull" 차이를 그려드릴 수도 있어요. 그럴까요?

나의 말:
git pull
ChatGPT의 말:

git pull 은 사실상

👉 git fetch + git merge (기본 동작)
👉 또는 git fetch + git rebase (옵션 설정 시)

를 한 번에 실행하는 명령어예요.

동작 과정

원격 저장소에서 최신 커밋 가져옴 (git fetch)

현재 내가 checkout 중인 로컬 브랜치에 그 커밋을 합침 (git merge 또는 git rebase)

기본 사용법
## 원격 저장소(origin)에서 현재 브랜치 이름과 같은 브랜치 가져와서 자동 merge
git pull

## 원격 저장소 origin의 main 브랜치 가져와서 현재 브랜치에 병합
git pull origin main

## 가져온 뒤 merge 말고 rebase로 정리
git pull --rebase origin main

차이 예시

git fetch
→ 원격 저장소의 새 커밋만 가져옴 (내 로컬 브랜치 변화 없음).

git pull
→ 원격 커밋을 가져와 내 현재 브랜치에 바로 합쳐버림.

주의점

git pull을 바로 하면 충돌(conflict)이 발생할 수 있음 → 그래서 많은 팀에서는 항상 git fetch 후 git merge 또는 git rebase를 수동으로 하는 걸 권장해요.

git pull --rebase를 쓰면 히스토리가 깔끔해지지만, 이미 푸시된 커밋을 rebase하면 꼬일 수 있음.

원하시면 제가 fetch vs pull 동작을 그림으로 정리해드릴까요?

나의 말:
powershell
ChatGPT의 말:

PowerShell은 마이크로소프트에서 만든 셸(shell) & 스크립트 언어입니다.
기존 cmd.exe(명령 프롬프트)보다 강력하게 설계되어, 단순한 명령 실행뿐만 아니라 객체 지향적인 자동화 작업이 가능해요.

특징

객체(Object) 기반
→ 기존 cmd나 bash는 문자열만 다루지만, PowerShell은 .NET 객체를 그대로 전달해서 더 정밀한 처리가 가능.

호환성
→ Windows 기본 제공, 최근에는 크로스 플랫폼 (Linux, macOS) 지원 (PowerShell Core).

자동화
→ 시스템 관리, 배치 작업, DevOps 스크립팅에 자주 사용.

명령어(alias)
→ 익숙한 UNIX 명령어나 cmd 명령어와 호환되는 별칭 제공 (ls, cat, dir 등).

기본 명령어 예시
## 현재 디렉토리 보기
Get-Location     # alias: pwd

## 디렉토리 목록
Get-ChildItem    # alias: ls, dir

## 파일 내용 출력
Get-Content file.txt   # alias: cat

## 프로세스 보기
Get-Process     # alias: ps

## 서비스 보기
Get-Service

## 변수 사용
$var = "Hello PowerShell"
Write-Output $var

스크립트 실행

확장자: .ps1

실행 정책 때문에 스크립트가 막힐 수 있음 → 아래로 허용 가능:

Git 브랜치 전략, 딱 정리
1) 전략 3종 비교

Trunk‑Based (추천, 소규모/속도 우선)

기본: main 하나 + 짧은 feature/… 브랜치

원칙: 작은 PR(≤200줄), 1–2일 내 머지, 기능 플래그로 미완성 숨김

장점: 충돌·릴리스 부담 최소, CI/CD 친화

GitHub Flow (웹서비스/상시배포)

main에서 feature/… 따고 → PR → 리뷰 → 머지 → 배포

배포는 main 머지와 거의 동시

Git Flow (릴리스 주기 뚜렷/온프렘)

브랜치: main(배포), develop(통합), feature/…, release/…, hotfix/…

장점: 릴리스 준비·핫픽스 절차 명확 (대신 브랜치 많아짐)

2) 상황별 추천

팀 작고 배포 자주 → Trunk‑Based

웹서비스, 배포 자동화 완료 → GitHub Flow

장기 QA/버전 유지보수 필요(예: 고객사별 릴리스) → Git Flow

3) 공통 규칙(전략과 무관하게)

네이밍

feature/<JIRA-123>-짧은-설명

hotfix/<버그키>

release/<vX.Y.Z>

보호 규칙 (예: GitHub)

main(그리고 develop 사용 시): 직접 푸시 금지, 최소 1–2명 리뷰, 필수 CI 통과, squash merge 권장

PR 룰

작게, 설명/테스트 포함, Draft → Ready for review 흐름

버저닝/태그

SemVer: vMAJOR.MINOR.PATCH

배포 커밋에 태그: git tag -a v1.4.2 -m "2025-08-19 release"; git push --tags

✅ 정리노트: GitHub Collaborators (협력자 설정)

위치: GitHub 저장소 → Settings → Access → Collaborators

역할: 저장소에 외부 사용자 초대

Owner(소유자): 모든 권한

Collaborator: 지정된 저장소에 push, issue, PR 등 권한 부여

권한 부여 과정

저장소 소유자가 Collaborators 메뉴에서 초대

이메일/알림으로 초대 수락

해당 사용자는 해당 저장소에 접근 가능

📌 팀 관리 팁

소규모 프로젝트 → Collaborators로 직접 추가

조직(Organization) 기반 협업 → 팀 단위 권한 관리 추천 (Admin, Maintain, Write, Triage, Read)
