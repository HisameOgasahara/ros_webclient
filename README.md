# Robot Delivery UI for GitHub Pages

Site: https://hisameogasahara.github.io/ros_webclient/

Debug: https://hisameogasahara.github.io/ros_webclient/debug.html

GitHub Pages에 올릴 정적 웹 UI 프로젝트다.

## 구성

- `index.html`: 주문/회수 UI. `driver`, `block`, `pen`, `wrench`와 A/B 방 흐름을 포함한다.
- `debug.html`: Cloudflare quick tunnel/WebSocket 연결만 확인하는 echo 테스트 UI.
- `.nojekyll`: GitHub Pages가 정적 파일을 그대로 서빙하도록 둔다.

## WebSocket 주소

GitHub Pages는 HTTPS로 서비스되므로 Jetson 브릿지에는 가급적 `wss://`로 접속해야 한다.

권장 구조:

```text
GitHub Pages HTTPS UI
  -> wss://YOUR-BRIDGE-DOMAIN.example.com
  -> Cloudflare Tunnel 또는 reverse proxy
  -> Jetson localhost:3000
  -> rtreebot delivery_bridge
```

`index.html`의 서버 설정 모달에는 아래 둘 다 입력할 수 있다.

```text
wss://YOUR-BRIDGE-DOMAIN.example.com
ws://YOUR-JETSON-LAN-IP:3000
```

단, GitHub Pages에서 열린 HTTPS 페이지는 브라우저 정책상 일반 `ws://` 연결이 막힐 수 있다. 운영/시연용은 `wss://`를 우선 사용한다.

## 무료 Cloudflare quick tunnel

고정 도메인 없이 무료로 쓸 때는 Jetson에서 tunnel을 실행한 뒤 출력된 임시 주소를 UI에 입력한다.

```bash
cloudflared tunnel --url http://localhost:3000
```

출력 예:

```text
https://example-random-name.trycloudflare.com
```

UI 서버 주소에는 아래처럼 입력한다.

```text
wss://example-random-name.trycloudflare.com
```

`trycloudflare.com` 주소는 실행할 때마다 바뀔 수 있다.

## 네트워크 디버깅

실제 로봇 명령을 보내기 전에 WebSocket 연결만 확인하려면 `debug.html`을 연다.

Jetson의 `work_ref_jetson_ready/auxiliary_code_backup/network_connectivity`에는 echo 테스트 서버와 quick tunnel 실행 스크립트가 있다.

```bash
cd ~/ros_project/auxiliary_code_backup/network_connectivity
chmod +x start_echo_quicktunnel.sh
./start_echo_quicktunnel.sh
```

스크립트가 출력하는 아래 형식의 주소를 `debug.html`의 WebSocket URL에 넣는다.

```text
wss://example-random-name.trycloudflare.com
```

`debug.html`에서 `Connect` 후 `Send`를 눌렀을 때 `received: echo: ...`가 나오면 GitHub Pages -> Cloudflare -> Jetson WebSocket 경로는 살아 있다.

이 echo 테스트는 기본 `3001` 포트를 사용한다. 실제 delivery bridge 기본 포트 `3000`과 동시에 헷갈리지 않게 분리해 둔 것이다.

## 배포

이 폴더를 별도 GitHub repository로 만든 뒤 GitHub Pages를 켠다.

예시:

```bash
git init
git add .
git commit -m "Initial robot delivery UI"
git branch -M main
git remote add origin https://github.com/HisameOgasahara/ros_webclient.git
git push -u origin main
```

GitHub repository Settings > Pages에서 `main` branch root를 선택한다.

## 운영 메모

무료 quick tunnel을 쓸 때는 Jetson에서 bridge/tunnel을 실행할 때마다 새 `wss://...trycloudflare.com` 주소를 UI 설정창에 입력한다.

브릿지는 데모용 최소 검증만 한다. 별도 token은 없고, UI가 보내는 `create_order`/`retrieve_item` 요청 중 A/B 방과 `driver`, `block`, `pen`, `wrench`만 처리한다.
