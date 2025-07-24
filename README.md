# 🐶 Puppi Scan

## 프로젝트 소개

**Puppi Scan**은 반려견의 피부 질환을 AI 기술로 진단하고, 맞춤형 정보를 진단서의 형태로 제공하는 웹 서비스입니다. 사용자는 반려동물의 피부 사진을 업로드하면, YOLOv5 객체 탐지 모델과 CNN 기반 분류기를 통해 피부 질환을 인식하고, Durable 라이브러리를 활용한 룰 기반 매핑과 GPT 기반 요약까지 연계된 사용자 맞춤형 진단서를 받을 수 있습니다.

## 기술 스택

* **Frontend**

  * Flask Template (HTML)
* **Backend**

  * Python Flask
  * Gunicorn (WSGI HTTP 서버)
  * Nginx (Reverse Proxy 및 SSL)
  * Certbot + Let's Encrypt (SSL 인증서 자동 갱신)
  * Supervisor (프로세스 관리)
* **AI & API & Library**

  * YOLOv5 (객체 인식)
  * OpenAI API (GPT 기반 설명 생성)
  * Durable (설명 저장 및 리포트 생성)
* **Infra**

  * AWS EC2 (Ubuntu 22.04)
  * Route 53 + Elastic IP + Domain: `https://puppi-scan.shop`
  * GitHub Actions (배포 자동화 예정)

## 데이터 셋

* **YOLO 학습용 이미지**: 반려견 피부 질환 종류별 수집 이미지 (클래스별 수동 라벨링 포함)
* **질병 설명 데이터**: 수의학 논문, 공공 데이터 기반 설명문 수집 및 정제
* **텍스트 학습 데이터**: OpenAI GPT API를 통한 응답 튜닝

## 시작 가이드

### 1. 프로젝트 클론

```bash
git clone https://github.com/PuppiScan/PuppiScan-Server.git
cd PuppiScan-Server
```

### 2. 라이브러리 Requirements

```bash
pip install -r requirements.txt
```

또는 주요 라이브러리 수동 설치:

```bash
pip install flask python-dotenv openai
```

### 3. Flask 로컬 실행 (개발 환경)

```bash
export FLASK_APP=app.py
flask run
```

### 4. Gunicorn 실행 (운영 환경)

```bash
gunicorn -w 4 -b 0.0.0.0:8000 app:app
```

## 시스템 아키텍처

![image](https://github.com/user-attachments/assets/5d1feca0-aa75-4018-9d8d-23cd0e2f64bb)

* 사용자 → 웹 브라우저를 통해 `https://puppi-scan.shop` 접속
* Route 53 → EC2 인스턴스의 Nginx로 라우팅
* Nginx → Gunicorn → Flask 앱으로 요청 전달
* Flask 앱 → YOLOv5 모델, OpenAI API, Durable 라이브러리 활용
* Certbot → Let's Encrypt로부터 SSL 인증서 자동 발급 및 갱신
* Supervisor → systemd를 통해서 Flask 앱 및 Gunicorn 서비스 자동 재시작
* GitHub → 코드 업데이트 자동 `git pull`
