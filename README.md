# KMTC -3W Booking Dashboard

사내 전용 (`@ekmtc.com` 도메인 계정만 접근) -3주전 부킹 현황 대시보드.

## 접근 방법

1. 배포 URL 접속 (GitHub Pages)
2. `@ekmtc.com` Google 계정으로 로그인
3. Drive 폴더 권한이 있어야 데이터 로드됨 (`@ekmtc.com` 도메인 전체에 공유되어 있음)

## 아키텍처

```
GitHub Pages (public/private)       : HTML/JS 코드만 (데이터 없음)
    ↓ Google OAuth (@ekmtc.com 로그인)
Google Drive (비공개 폴더)          : 대시보드 JSON 데이터
    ↑ 매일 09:30 자동 업로드
로컬 PC (Windows Task Scheduler)    : sync_cha_snapshot.py --archive 10
    ↑ 매일 09:03 차장님 파이프라인이 Drive 에 업로드
Tableau → 차장 파이프라인 → Google Drive (차장 소유 폴더)
```

## 데이터 보안

- 데이터는 이 repo 에 **없음** (Drive 에만 존재)
- 외부인이 repo 소스를 봐도 JS 코드만 보임
- Drive 파일은 `@ekmtc.com` 도메인 전용 공유 → 외부 Google 계정 접근 차단
- 관리자 변경은 Drive 공유 설정만 조정하면 즉시 반영

## 배포 과정

1. Google Cloud Console 에서 OAuth Client ID 발급
2. `index.html` 의 `OAUTH_CLIENT_ID` 교체
3. GitHub Pages 활성화 (Settings → Pages → main branch)
4. 배포 URL 을 Client ID 의 "Authorized JavaScript origins" 에 등록

## 데이터 갱신

- 매일 09:30 로컬 PC 에서 `sync_cha_snapshot.py --archive 10` 실행
- 결과: 차장 parquet pull → 우리 스냅샷 생성 → Drive 자동 업로드
- 대시보드는 최신 Drive 데이터를 즉시 반영 (재배포 불필요)

## 문의

lhjin@ekmtc.com
