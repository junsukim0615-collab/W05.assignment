# 컴퓨터네트워크 5주차 과제

원 과제: [Week 5 — Addresses, Subnets, NAT, DHCP](https://github.com/codingchild2424/2026-lecture-network-practice/tree/main/w05-ip-nat)

이 저장소의 루트는 원 과제의 `w05-ip-nat/` 폴더에 해당한다.

## 제출 확인표

| 요구사항 | 파일 | 확인 결과 |
|---|---|---|
| Task 1 CIDR 및 최장 prefix 매칭 | `task1_forward.py` | 공식 검증 10/10 |
| 서로 다른 네트워크 주소 2개 | `out/addresses.json` | 2026-09-29 / 2026-10-19 측정 |
| 본인 DHCP DORA 캡처 | `out/dhcp.pcapng` | 프레임 2–5, 동일 Transaction ID |
| 주소·NAT·DHCP 분석 | `out/report.md` | A2–A5, B3, C2–C4 작성 |
| 빠른 LPM 구현 | `task3_lpm.py` | 20000개 질의 모두 기준과 일치 |
| 벤치마크 결과 | `out/bench.txt` | 1341.2배 향상, strong |
| 과제별 관찰 기록 | `out/observation.md` | Task 1–3 작성 |

## 실행

```powershell
python task1_forward.py --verify
python bench.py --yours
python test_tasks.py
```

`bench.py`와 `test_tasks.py`는 제공된 원본을 사용했다. DHCP 검증을 위해 Wireshark의 tshark가 PATH에 있어야 한다. `out/validation.txt`에는 검증 당시의 출력이 있다. 벤치마크 속도는 컴퓨터 부하에 따라 달라진다.

주소를 다시 수집하려면 다른 네트워크에 연결한 상태에서 `python task2_myaddr.py --collect "새 라벨"`을 실행한다. 기존 기록에 추가되므로 같은 네트워크의 중복 기록을 만들지 않도록 주의한다. `--report`는 제공된 코드에서 구현되지 않은 옵션이며, 분석 결과는 `out/report.md`에 별도로 작성했다.

모든 필수 제출 파일을 갖췄다. NAT의 정확한 단계 수는 관측 자료로 확정할 수 없어 보고서에 근거와 한계를 명시했다. 업로드와 별개로 수업 제출 시스템에 저장소 링크를 제출해야 한다면 해당 절차는 별도로 수행해야 한다.
