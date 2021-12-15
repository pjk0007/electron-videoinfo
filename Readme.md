# ELECTRON

## ffmpeg, fluent-ffmpeg module

### 설명

nodejs 에서 비디오 파일 위치를 통해 비디오 정보를 반환한다.

### 설치

1. ffmpeg 설치
   window에서는 chocolately를 이용하면 편하게 설치 가능

```powershell
choco install ffmpeg
```

2. fluent-ffmpeg 설치

```powershell
npm install --save fluent-ffmpeg
```

### 사용

ffmpeg의 ffprobe함수를 이용하여 metadata를 확인할 수 있다.

```javascript
// index.js
ipcRenderer.on("video:metadata", (event, duration) => {
  document.querySelector("#result").innerHTML = `Video is ${duration.toFixed(
    2
  )} seconds`;
});
```

## ipc통신

Electron <-> mainWindow 간의 통신을 할 수 있다.

1. mainWindow -> Electron
   ipcRenderer.send("이벤트이름", 데이터)

```javascript
// javascript in index.html (webpages)
ipcRenderer.send("video:submit", path);
```

2. Electron에서 ipc 수신
   ipcMain.on("이벤트이름", 함수)

```javascript
// index.js (electron app)
ipcMain.on("video:submit", (event, path) => {
  ffmpeg.ffprobe(path, (err, metadata) => {
    console.log(metadata);
    c;
  });
});
```

3. Electron -> mainWindow
   mainWindow.webContents.send("이벤트이름", 데이터)

```javascript
// index.js (electron app)
mainWindow.webContents.send("video:metadata", metadata.format.duration);
```

4. mainWindow에서 ipc 수신
   ipcRenderer.on("이벤트이름", 함수)

```javascript
// javascript in index.html (webpages)
mainWindow.webContents.send("video:metadata", metadata.format.duration);
```
