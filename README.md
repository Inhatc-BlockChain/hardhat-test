# Hardhat 3 + ethers.js 실습 (hardhat-test)

하나의 프로젝트 폴더(hardhat-test) 안에서 컨트랙트를 하나씩 추가하며 실습합니다.
(Counter.sol → 다음 실습에서 Storage.sol, … 을 같은 폴더에 추가)

## 받는 방법

### 방법 1: ZIP 으로 받기 (git 설치 필요 없음, 권장)
1. 이 저장소 화면의 초록색 **Code** 버튼 → **Download ZIP**
2. 압축 파일을 `C:\blockchain` 에 풀기 (zip 안에서 바로 열지 않기)
3. 풀린 폴더 이름이 `hardhat-test-main` 이면 → `hardhat-test` 로 이름 바꾸기
4. VS Code → File → Open Folder → `C:\blockchain\hardhat-test`

### 방법 2: git clone 으로 받기 (git 이 설치된 경우)
```
cd C:\blockchain
git clone https://github.com/Inhatc-BlockChain/hardhat-test.git
cd hardhat-test
```
- 나중에 교수자가 Starter 를 수정하면 `git pull` 로 받기

### 받은 후 (두 방법 공통)
```
npm ci
```

### 올리면 안 되는 것
- **개인 키를 코드나 파일에 쓰지 않기** → `npx hardhat keystore set …` 으로만 저장
- `node_modules/`, `artifacts/`, `cache/`, `ignition/deployments/` 는 `.gitignore` 에 등록되어 있어 올라가지 않음
- 자기 저장소에 올리기 전에 `git status` 로 올라갈 파일 확인

## 0. 준비
- Node.js **22.13 이상** 필요 → 터미널에서 `node -v` 로 확인
- 이 폴더를 `C:\blockchain\hardhat-test` 처럼 **짧은 경로**에 복사해서 사용
  - OneDrive, Google Drive 같은 동기화 폴더 안에서는 실습하지 않기 (node_modules 동기화 문제)
- PowerShell 에서 `npm` / `npx` 실행 시 "스크립트를 실행할 수 없으므로…" 오류가 나면 한 번만 실행:
  ```
  Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
  ```

## 1. 설치 (프로젝트 폴더에서, 처음 한 번만)
```
npm ci
```
- `node_modules/` (약 200MB) 가 생김. 컨트랙트를 추가해도 다시 설치할 필요 없음
- 다른 사람에게 전달할 때는 `node_modules/` 를 빼고 전달 → 받은 사람이 `npm ci`

## 2. 명령 요약 (Counter 기준)
| 하는 일 | 명령 | 짧은 명령 |
|---|---|---|
| 컴파일 (모든 .sol) | `npx hardhat build` | `npm run build` |
| 테스트 (모든 테스트) | `npx hardhat test` | `npm test` |
| 로컬 노드 실행 (터미널 1) | `npx hardhat node` | `npm run node` |
| 로컬 배포 (터미널 2) | `npx hardhat ignition deploy ignition/modules/Counter.ts --network localhost` | `npm run deploy:local` |
| ethers.js 호출 | `node scripts/interact.js` | `npm run interact` |
| 0에서 1 빼기 (V2) | `node scripts/subtract.js` | |
| Sepolia 배포 | `npx hardhat ignition deploy ignition/modules/Counter.ts --network sepolia` | `npm run deploy:sepolia` |

## 3. 작성할 파일 (Counter)
- `contracts/Counter.sol`
- `ignition/modules/Counter.ts`
- `scripts/interact.js`, `scripts/subtract.js`
- (부록) `test/Counter.ts`

## 4. 컨트랙트 추가하기 (예: Storage)
| 할 일 | 파일 / 명령 |
|---|---|
| ① 컨트랙트 작성 | `contracts/Storage.sol` (contract 이름은 다른 파일과 겹치지 않게) |
| ② 배포 모듈 작성 | `ignition/modules/Storage.ts` (모듈 이름 `"StorageModule"` 도 겹치지 않게) |
| ③ 컴파일 | `npx hardhat build` → `Compiled 2 Solidity files` |
| ④ 배포 | `npx hardhat ignition deploy ignition/modules/Storage.ts --network localhost` |
| ⑤ 호출 | `scripts/interact-storage.js` 작성 → `node scripts/interact-storage.js` |
| (부록) 테스트 | `test/Storage.ts` → 이 파일만 실행: `npx hardhat test test/Storage.ts` |

### 주의
- 한 파일이라도 문법 오류가 있으면 **전체 build 실패** (`HHE910: Compilation failed`)
- 같은 contract 이름이 두 파일에 있으면 `HHE1001: There are multiple artifacts …`
- `--reset` 은 그 네트워크의 **모든** 배포 기록을 지움 → 다른 컨트랙트(Storage 등)도 다시 배포해야 함
