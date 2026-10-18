# 컴퓨터네트워크 5주차 — 실습 결과 제출

이 저장소에는 실제 구현 코드, 본 컴퓨터에서 수집한 두 네트워크 기록과 DHCP 캡처, 결과 분석을 담았다. 저장소 루트가 원 과제의 `w05-ip-nat/` 폴더에 해당한다.

## 완성된 결과 보고서

| 실습 | 결과 보고서 | 핵심 결과 |
|---|---|---|
| Task 1 — 서브넷과 경로 선택 | [Task 1 구현 결과](task1.md) | CIDR 검사·범위 계산·최장 prefix 매칭 구현, 10/10 통과 |
| Task 2 — 주소·NAT·DHCP | [Task 2 실험 결과](out/report.md) | 네트워크 2곳 비교, 실제 DORA, 임대 시간 7200초 |
| Task 3 — 빠른 경로 탐색 | [Task 3 성능 개선 결과](task3.md) | 20000개 질의 오답 0개, 1341.2배 향상 |
| 과제별 관찰 | [observation.md](out/observation.md) | 경계 조건·NAT 근거·조회 복잡도와 메모리 해석 |

루트의 [task2.md](task2.md)도 완성된 Task 2 보고서다. 원 과제 안내는 [docs/assignment](docs/assignment/)에 참고 자료로 보관했다.

## 측정 결과 요약

- 네트워크 1: Wi-Fi IPv4 `172.16.1.29/24`, 게이트웨이 `172.16.1.1`, 외부 IPv4 `163.152.233.1`.
- 네트워크 2: Wi-Fi IPv4 `192.168.0.14/24`, 게이트웨이 `192.168.0.1`, 외부 IPv4 `59.27.56.98`.
- DHCP: 프레임 2–5의 Transaction ID `0xd97f0f72`가 같으며 Discover→Offer→Request→Ack가 확인됐다. Offer/Ack의 임대 시간은 7200초다.
- LPM: 5000개 경로 전체 비교를 최대 7개 prefix 길이 조회로 줄였다. 이번 20000개 질의의 평균 해시 조회는 2.6129회였다.

NAT 단계 수는 관측 자료로 확인 가능한 범위와 상위 장비 확인이 필요한 범위를 보고서에서 구분했다. SSID나 장소 이름을 추측해 기록하지 않았다.

## 제출 파일과 근거

| 파일 | 내용 |
|---|---|
| [task1_forward.py](task1_forward.py) | 완성된 서브넷 계산 및 포워딩 구현 |
| [addresses.json](out/addresses.json) | 서로 다른 두 네트워크의 원본 수집 기록 |
| [dhcp.pcapng](out/dhcp.pcapng) | 본 컴퓨터에서 직접 캡처한 DHCP 패킷 |
| [report.md](out/report.md) | 주소 계산 과정, NAT 분석, 네트워크 비교 및 DHCP 분석 |
| [task3_lpm.py](task3_lpm.py) | 완성된 prefix 길이별 해시 조회 구현 |
| [bench.txt](out/bench.txt) | 공식 벤치마크 출력: 1341.2배, strong |
| [observation.md](out/observation.md) | Task 1–3 관찰 결과 |

추가 근거: [DHCP 추출값](out/dhcp-summary.tsv), [전체 검증 결과](out/validation.txt), [제출 형식 검사](out/format-check.txt), [경계 조건·조회 횟수·메모리 측정값](out/implementation-checks.json).

## 재현 방법

```powershell
python task1_forward.py --verify
python task2_myaddr.py --report
python bench.py --yours
python test_tasks.py
```

`--report`는 이미 작성한 결과 보고서를 출력하며 새 측정을 수행하지 않는다. 새 네트워크 수집은 `python task2_myaddr.py --collect "새 라벨"`로 수행한다. 새로운 기록을 수집한 경우 보고서도 해당 측정에 맞춰 갱신해야 한다.

전체 테스트에서 DHCP 패킷을 검사하려면 Wireshark의 tshark가 PATH에 있어야 한다. 저장된 검증 결과는 **8 passed, 0 failed, 0 skipped**다. `bench.py`와 `test_tasks.py`는 제공된 원본을 유지했다.

원 과제: [Week 5 Lab](https://github.com/codingchild2424/2026-lecture-network-practice/tree/main/w05-ip-nat).
