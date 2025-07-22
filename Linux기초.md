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


## 라이센스

이 자료는 교육 목적으로 자유롭게 사용할 수 있습니다.
