# MSX_OPNAB_VGM_CONVERTER

# X68000 (YM2151/okim6258) → YM2151/YM2610B/YM2608 VGM 컨버터 
# MEGADRIVE (YM2612/SN76489) → YM2610B/YM2608 VGM 컨버터 

X68000 (YM2151/okim6258)와 
세가 메가드라이브/제네시스(YM2612 FM / SN76489 PSG) VGM 파일을, 
MSX용 사운드 카트리지인 **Neotron(YM2610B/OPNB)** 또는 **Makoto(YM2608/OPNA)** 로 재생할 수 있는 VGM 파일로 변환하는 컨버터.

---

## 1. 기본 사용법

```
vgm_convert.py [--ym2608] <input.vgm> <output.vgm>      단일 파일 변환
vgm_convert.py [--ym2608] <pattern...>                  일괄 변환 (와일드카드 * ? 가능)
                                                              출력 파일명: <원본이름>_2610b.vgm (또는 _2608)
vgm_convert.py [--ym2608] <directory>                   디렉터리 안의 모든 .vgm/.vgz를 재귀적으로 변환
vgm_convert.py [--ym2608] <inputs...> <output_dir>      여러 입력을 output_dir 폴더로 일괄 변환
                                                         (하위 폴더 구조 그대로 유지)
```

`.vgz`(gzip 압축 VGM)는 입력/출력 모두 지원됩니다.

### 예시

```
vgm_convert.py song.vgm song_out.vgm
vgm_convert.py *.vgm *.vgz
vgm_convert.py "Gunstar Heroes"
vgm_convert.py --ym2608 sonic??.vgm out/
vgm_convert.py vgm_collection/ converted/
```

---

## 2. 옵션


 `--ym2608`     : YM2610B(기본값) 대신 YM2608(OPNA)용 VGM으로 출력. 
 `--opt`        : YM2612 DAC/PCM을 원본 드라이버가 실제로 쓰던 재생 속도로 다운샘플링한 뒤 재인코딩(용량 절감, 음질 손실 없이 레이트만 원본에 맞춤). 
 `--ssg-vol N`  : SSG 볼륨 오프셋, 약 3dB 단위(소수점 스텝 가능, 예: `-6.25`). 
     기본값: `--ym2608`는 `-2`, YM2610B는 `-3.03`(YM2608 기준 약 70% 진폭 - Neotron의 SSG가 Makoto보다 FM 파트 대비 더 높게 출력되기 때문) 

 YM2610B는 디폴트 사항 (Neotron은 pcm을 위한 고정 용량 제한이 없음) 
 

---


## 6. 빠른 참고

```
# 기본(YM2610B, 44.1kHz 그대로)
vgm_convert.py song.vgm song_2610b.vgm

# YM2610B, 용량 절약(384KB 예산 자동 맞춤)
vgm_convert.py --opt song.vgm song_2610b_opt.vgm

# YM2608(Makoto), 항상 256KB에 맞춰 다운샘플링됨
vgm_convert.py --ym2608 song.vgm song_2608.vgm

# SSG 볼륨 수동 조정
vgm_convert.py --ym2608 --ssg-vol -4 song.vgm song_2608_quiet.vgm

# 폴더 전체 일괄 변환
vgm_convert.py --opt "게임폴더/" 변환결과/
```
