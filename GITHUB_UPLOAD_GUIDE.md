# 새 GitHub 저장소 업로드 안내

1. GitHub 웹사이트에서 **New repository** → 이름 예: `morai-highway-team-handoff` → **Private** → **Add a README file** 체크 → Create repository.
2. 이 대화에 생성된 저장소 주소(`https://github.com/계정/저장소`)를 보내 주세요.
3. 연결된 GitHub 도구로 `main` 기준 `team-handoff` 브랜치를 생성한 다음 이 묶음의 파일을 그 브랜치에 업로드할 수 있습니다.
4. 팀원 GitHub 계정을 Settings → Collaborators에 초대하고 `team-handoff` 브랜치를 안내하세요.

기존 저장소와 연결하지 마세요. 새 저장소의 `main`과 공유 브랜치를 분리해서 사용합니다.

## 로컬 Git 대안

새 저장소가 **README 포함** 상태라면:

```bash
git clone https://github.com/ACCOUNT/morai-highway-team-handoff.git
cd morai-highway-team-handoff
git switch -c team-handoff
# 압축 파일 안의 내용물을 현재 디렉터리에 복사
# 여기서 소스 변경 내역을 다시 검토하고:
git add .
git commit -m "Add MORAI highway control handoff snapshot"
git push -u origin team-handoff
```
