# Robot Delivery UI

Site:

```text
https://hisameogasahara.github.io/ros_webclient/
```

Debug:

```text
https://hisameogasahara.github.io/ros_webclient/debug.html
```

## 1. Jetson에서 미션 실행

Jetson 터미널에서:

```bash
~/turtlebot3_ws/scripts/start_mission_tmux.sh
```

tmux가 열리면 `urls` 창을 본다.

```text
Paste this WebSocket URL into the UI:
  wss://...trycloudflare.com
```

이 `wss://...trycloudflare.com` 주소를 복사한다.

## 2. 웹 UI에서 연결

브라우저에서 연다.

```text
https://hisameogasahara.github.io/ros_webclient/
```

상단 `Cloudflare WebSocket 주소` 입력칸에 Jetson에서 복사한 주소를 붙여넣고 `연결`을 누른다.

상태가 `연결됨`이 되면 주문을 보낸다.

## 3. 주문/회수

1. 방 `A` 또는 `B`를 선택한다.
2. 물품 `driver`, `block`, `pen`, `wrench` 중 하나를 선택한다.
3. `주문하기`를 누른다.
4. 배송 완료 후 필요한 경우 `사용 완료`를 누르고 회수 요청을 보낸다.

## 4. 연결만 확인

웹 UI 연결이 안 되면 debug 페이지를 연다.

```text
https://hisameogasahara.github.io/ros_webclient/debug.html
```

미션 실행 때 나온 같은 `wss://...trycloudflare.com` 주소를 넣고 `Connect`를 누른다.

연결이 되면 GitHub Pages -> Cloudflare -> Jetson WebSocket 경로는 살아 있다.

## 5. 종료

Jetson에서:

```bash
~/turtlebot3_ws/scripts/stop_mission_tmux.sh
```
