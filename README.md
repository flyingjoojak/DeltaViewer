# 🎮 Delta Viewer (Delta Force Item Value Overlay)

**게임 내 복잡한 아이템 가치를 빠르고 직관적으로 계산해주는 오버레이 도구입니다.**

게임 플레이 중 아이템의 총 가격과 칸 수를 자동으로 인식하여, **"1칸당 가치(칸성비)"**를 실시간으로 계산해 화면에 띄워줍니다. 파밍 효율을 극대화하기 위해 개발되었습니다.

---

## ✨ 주요 기능 (Key Features)

1.  **실시간 이미지 분석 (Real-time Analysis)**
    *   **OCR (광학 문자 인식)**: Tesseract 엔진을 활용하여 아이템의 가격 정보를 정확하게 추출합니다.
    *   **컴퓨터 비전 (Computer Vision)**: OpenCV를 사용하여 아이템이 차지하는 인벤토리 칸 수를 자동으로 인식합니다. (윤곽선 검출 및 템플릿 매칭 알고리즘 사용)

2.  **직관적인 오버레이 UI**
    *   게임 화면 위에 **투명한 오버레이**를 띄워 정보를 보여줍니다.
    *   마우스 커서 위치를 추적하여 시선을 방해하지 않는 최적의 위치에 정보를 표시합니다.
    *   **핵심 정보 강조**: 파밍 결정에 가장 중요한 **'칸당 가격'**을 크게 표시합니다.

3.  **간편한 조작 & 최적화**
    *   **단축키 (`z`)**: 원하는 아이템에 마우스를 올리고 키 하나만 누르면 즉시 분석합니다.
    *   **스마트 스캔**: 스캔 순간 오버레이를 잠시 숨겨 OCR 인식률을 높이고 간섭을 방지합니다.
    *   **이미지 매칭 모드**: 자주 등장하는 칸 모양을 미리 학습시켜 인식 정확도를 100%에 가깝게 보정할 수 있습니다.

---

## 🛠️ 기술 스택 (Tech Stack)

*   **Language**: Python 3.10+
*   **GUI**: Tkinter (Overlay Interface)
*   **Computer Vision**: OpenCV (Slot Detection), NumPy
*   **OCR**: Tesseract-OCR (Price Text Extraction)
*   **Input Handling**: Pynput, Keyboard (Global Hotkeys)
*   **Screen Capture**: MSS (Fast Screenshot)
*   **Packaging**: PyInstaller (Standalone Executable)

---

## 🚀 설치 및 실행 (Installation & Run)

### 1. 간편 실행 (추천 - Portable)
**별도의 설치 과정 없이 바로 실행할 수 있는 방법입니다.**
1.  배포된 압축 파일을 다운로드 후 압축을 풉니다.
2.  폴더 내에 `DeltaForceOverlay.exe`와 `tesseract` 폴더가 함께 있는지 확인합니다.
3.  `DeltaForceOverlay.exe`를 실행하면 바로 작동합니다. (Tesseract 설치 불필요)

### 2. 직접 설치 및 실행 (개발자용)
소스 코드를 직접 수정하거나 실행하고 싶은 경우입니다.

**요구 사항 (Prerequisites):**
*   Python 3.10+
*   [Tesseract-OCR](https://github.com/UB-Mannheim/tesseract/wiki) (직접 설치 필요)

**실행 방법:**
1. zip 파일 압축 해제
2. Tesseract-OCR 설치후 압축 해제 폴더로 복사해주세요.
2-1. ex. C:\Program Files\Tesseract-OCR >> C:\Users\main\Downloads\DeltaViewer\tesseract
3. DeltaViewer.exe 실행

## 📖 사용 가이드 (User Guide)

1.  프로그램을 실행하면 잠시 후 **"Overlay Ready"** 메시지가 마우스 근처에 나타납니다.
2.  게임 내에서 가치를 확인하고 싶은 아이템 중앙에 마우스를 올립니다.
3.  **`z` 키**를 누릅니다.
4.  오버레이에 계산된 정보가 표시됩니다.
    *   **P/S (큰 글씨)**: 1칸당 가격 (효율)
    *   Total: 총 가격
    *   Slots: 차지하는 칸 수
5.  프로그램 종료는 **`Shift + z`**를 누르면 됩니다.
<img width="405" height="216" alt="image" src="https://github.com/user-attachments/assets/b0c2f98c-c6a4-4eb3-bc70-8c06e7378a0e" />
<img width="397" height="215" alt="image" src="https://github.com/user-attachments/assets/0ba6ffc0-532e-4350-b7a0-e0e9539bbee8" />

---

## 🔍 기술적 챌린지 및 해결 (Challenges & Solutions)

*   **OCR 오인식 문제**: 초기에 숫자 `1`을 `l`로 읽거나 콤마(`,`)를 점(`.`)으로 읽는 문제가 있었습니다.
    *   👉 **해결**: Tesseract의 `whitelist` 설정을 통해 숫자와 기호만 읽도록 제한하고, 정규식(Regex)을 활용한 **'최대 정수 추출 알고리즘'**을 적용하여 노이즈 속에서도 정확한 가격을 찾아내도록 개선했습니다.
    
*   **오버레이 간섭**: 오버레이가 떠 있는 상태에서 재스캔 시, 오버레이의 글자를 OCR이 읽어버리는 문제가 발생했습니다.
    *   👉 **해결**: 스캔 트리거가 발동되면 **오버레이를 0.1초간 숨긴 후** 화면을 캡처하고, 다시 결과를 띄우는 방식으로 간섭을 원천 차단했습니다.

*   **다양한 해상도 대응**: 사용자마다 게임 해상도가 달라 고정된 좌표로는 한계가 있었습니다.
    *   👉 **해결**: 마우스 커서를 기준으로 일정 영역(`400x300px`)을 **동적 캡처(ROI)**하도록 설계하여 해상도에 구애받지 않고 작동하도록 구현했습니다.

---

**Developed by flyingjoojak**
