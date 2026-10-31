# introBot
로봇 프로그래밍 가상환경

MuJoCo와 관련 Python 패키지는 Dev Container의 `/opt/venv`에 설치됩니다.
컨테이너를 재빌드한 뒤 Python 실행과 VS Code 분석은 이 가상환경을 사용합니다.
`colcon build` 성공 여부와 Python 패키지 import 가능 여부는 별개이므로,
MuJoCo 설치 확인은 `python3 test.py`로 할 수 있습니다.
