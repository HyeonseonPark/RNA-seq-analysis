# Bioinformatics를 위한 Linux 기초
## 명령어 구조와 실습

---

## 1. Linux 명령어의 기본 구조

### 📋 표준 형태
```bash
명령어 [옵션] [입력파일/경로] [> 출력파일]
```

### 실제 예시들
```bash
# 기본형: 명령어만
ls
pwd

# 옵션 추가
ls -l
ls -la

# 파일 지정
cat sequence.fasta
head -10 data.txt

# 결과를 파일로 저장
grep "ATG" sequence.fasta > start_codons.txt
```

---

## 2. 파일 시스템 탐색

### 기본 탐색 명령어
```bash
# 현재 위치 확인
pwd

# 디렉토리 내용 보기
ls                    # 간단히
ls -l                 # 상세정보
ls -la                # 숨김파일 포함
ls *.fasta            # 특정 확장자만

# 디렉토리 이동
cd /home/user/data
cd ..                 # 상위 디렉토리
cd ~                  # 홈 디렉토리
```

### 파일 정보 확인
```bash
# 파일 크기와 라인 수
wc filename.txt
wc -l filename.txt    # 라인 수만
wc -c filename.txt    # 문자 수만
wc -w filename.txt    # 단어 수만

# 파일 타입 확인
file sequence.fasta
file *.txt
```

---

## 3. 파일 내용 보기

### 전체 내용 보기
```bash
cat sequence.fasta              # 전체 내용
less sequence.fasta             # 페이지 단위로 (q로 종료)
more sequence.fasta             # 페이지 단위로
```

### 부분 내용 보기
```bash
head sequence.fasta             # 처음 10줄
head -5 sequence.fasta          # 처음 5줄
tail sequence.fasta             # 마지막 10줄
tail -20 sequence.fasta         # 마지막 20줄

# 특정 줄 범위
sed -n '10,20p' sequence.fasta  # 10-20번째 줄
```

---

## 4. 텍스트 검색과 필터링

### 기본 검색
```bash
# 특정 문자열 찾기
grep "ATG" sequence.fasta
grep -i "atg" sequence.fasta    # 대소문자 구분 안함
grep -c "ATG" sequence.fasta    # 개수만 세기
grep -n "ATG" sequence.fasta    # 줄 번호 포함

# 여러 파일에서 검색
grep "GAATTC" *.fasta
grep -l "GAATTC" *.fasta        # 파일명만 출력
```

### 고급 검색
```bash
# 특정 패턴 제외
grep -v ">" sequence.fasta      # '>'가 없는 줄만

# 정규표현식 사용
grep "^ATG" sequence.fasta      # ATG로 시작하는 줄
grep "TAG$" sequence.fasta      # TAG로 끝나는 줄
grep "[ATGC]\{50,\}" seq.fasta  # 50자 이상의 염기서열
```

---

## 5. 파일 조작과 편집

### 파일 복사, 이동, 삭제
```bash
# 복사
cp sequence.fasta backup.fasta
cp *.fasta backup_folder/

# 이동/이름변경
mv old_name.fasta new_name.fasta
mv *.txt text_files/

# 삭제 (주의!)
rm unwanted.txt
rm -i *.tmp                     # 확인 후 삭제
```

### 텍스트 처리
```bash
# 정렬
sort sequences.txt
sort -r sequences.txt           # 역순
sort -n numbers.txt             # 숫자 정렬

# 중복 제거
uniq sequences.txt
sort sequences.txt | uniq       # 정렬 후 중복 제거

# 문자 변환
tr 'atgc' 'ATGC' < sequence.txt # 소문자를 대문자로
tr -d '\n' < sequence.txt       # 줄바꿈 제거
```

---

## 6. 파이프라인과 리다이렉션

### 기본 개념
```bash
# 리다이렉션 (출력을 파일로)
ls > file_list.txt
grep "ATG" seq.fasta > start_codons.txt
command >> file.txt             # 파일에 추가

# 파이프 (명령어 연결)
cat sequence.fasta | grep -v ">" | wc -c
ls -l | grep "fasta" | wc -l
```

### 실용적인 파이프라인 예시
```bash
# FASTA 파일에서 실제 서열 길이 계산
cat sequence.fasta | grep -v ">" | tr -d '\n' | wc -c

# 모든 FASTA 파일의 서열 개수 확인
ls *.fasta | xargs -I {} sh -c 'echo -n "{}: "; grep -c ">" {}'

# 특정 서열 패턴의 빈도 분석
grep -o "ATG" sequences.fasta | wc -l
```

---

## 7. 실제 Bioinformatics 작업 예시

### FASTA 파일 다루기
```bash
# FASTA 파일 기본 정보
grep -c ">" sequences.fasta     # 서열 개수
grep ">" sequences.fasta        # 헤더 정보만

# 서열 길이 분포 확인
awk '/^>/ {if (seq) print length(seq); seq=""; next} {seq=seq$0} END {if (seq) print length(seq)}' sequences.fasta | sort -n
```

### 품질 관리
```bash
# 빈 줄 제거
grep -v "^$" input.fasta > clean.fasta

# 특정 길이 이상의 서열만 추출
awk '/^>/ {header=$0} !/^>/ {seq=seq$0} /^$/ {if (length(seq)>=100) {print header; print seq} header=""; seq=""}' sequences.fasta
```

### 데이터 요약
```bash
# 염기 구성 분석
grep -v ">" sequence.fasta | tr -d '\n' | fold -w1 | sort | uniq -c

# GC 함량 계산 (간단한 방법)
seq=$(grep -v ">" sequence.fasta | tr -d '\n')
gc=$(echo $seq | tr -cd 'GCgc' | wc -c)
total=$(echo $seq | wc -c)
echo "scale=2; $gc * 100 / $total" | bc
```

---

## 8. 실습 과정

### 단계 1: 환경 설정
```bash
# 작업 디렉토리 생성
mkdir bioinformatics_practice
cd bioinformatics_practice

# 샘플 파일 다운로드 또는 생성
# (강사가 미리 준비한 파일들)
```

### 단계 2: 기본 탐색
```bash
# 파일 목록 확인
ls -la

# 각 파일의 기본 정보
file *
wc -l *
```

### 단계 3: 내용 분석
```bash
# FASTA 파일 구조 파악
head -20 sample.fasta
grep -c ">" sample.fasta

# 특정 서열 검색
grep -i "promoter" annotations.txt
grep "ATG" coding_sequences.fasta
```

### 단계 4: 데이터 처리
```bash
# 헤더만 추출
grep ">" sequences.fasta > headers.txt

# 서열만 추출
grep -v ">" sequences.fasta > sequences_only.txt

# 결과 저장
grep "ribosomal" annotations.txt > ribosomal_genes.txt
```

---

## 9. 유용한 팁들

### 명령어 기록과 재사용
```bash
history                         # 이전 명령어 목록
!!                             # 바로 이전 명령어 실행
!grep                          # 가장 최근 grep 명령어 실행
```

### 효율적인 작업
```bash
# Tab 키로 자동완성 활용
ls seq<Tab>                    # 파일명 자동완성
cat -n<Tab>                    # 옵션 자동완성

# 와일드카드 활용
*.fasta                        # 모든 FASTA 파일
seq_?.txt                      # seq_1.txt, seq_2.txt 등
seq_[12].txt                   # seq_1.txt, seq_2.txt만
```

### 자주 하는 실수들
```bash
# 잘못된 예시들과 올바른 방법
ls > *.txt                     # 잘못됨
ls *.txt                       # 올바름

grep ATG sequence.fasta > sequence.fasta  # 원본 파일 손상!
grep ATG sequence.fasta > filtered.fasta  # 올바름
```

---

## 10. 다음 단계로

이 기초를 익힌 후에는:
- **고급 텍스트 처리**: awk, sed 명령어
- **스크립트 작성**: bash 스크립팅
- **생물정보학 도구**: BLAST, samtools, bedtools 등
- **파이프라인 구축**: 여러 도구를 연결한 분석 워크플로우

---

## 기여하기

이 자료에 대한 개선 사항이나 추가 내용이 있으시면 이슈나 풀 리퀘스트를 통해 기여해주세요.

## 라이센스

이 자료는 교육 목적으로 자유롭게 사용할 수 있습니다.
