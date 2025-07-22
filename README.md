# RNA-seq-analysis

# RNA-seq 분석 파이프라인 워크플로우 요약

## 전체 분석 흐름

```
01.SRA_download → 02.FastQC → 03.PRINSEQ → 04.Alignment → 05.Matrix → 06.Sample QC → 07.DEG
     ↓               ↓            ↓             ↓             ↓           ↓            ↓
   FASTQ          품질검사      전처리       정렬/정량화     발현행렬     샘플QC       차등발현
```

---

## 단계별 주요 입출력

| 단계 | 스크립트 | 주요 입력 | 주요 출력 |
|------|----------|-----------|-----------|
| **01** | SRA_download.sh | `SRR_Acc_List.txt` | `SRR*_1.fastq`, `SRR*_2.fastq` |
| **02** | FastQC_analysis.sh | `SRR*_1.fastq`, `SRR*_2.fastq` | `fastqc/multiqc_report.html` |
| **03** | PRINSEQ_cleaning.sh | `SRR*_1.fastq`, `SRR*_2.fastq` | `SRR*_good_1.fastq`, `SRR*_good_2.fastq` |
| **04** | Alignment.sh | `*_good_1.fastq`, `참조유전체` | `Con1~3_sorted.bam`, `Cd1~3_sorted.bam`, `Con1~Cd3/gene_abund.tab` |
| **05** | Matrix.sh | `*/gene_abund.tab` | `gene_count_matrix.csv`, `genes.TPM.matrix` |
| **06** | Sample_QC.sh | `genes.counts.matrix`, `samples.file` | `*.pdf` (PCA, 상관관계) |
| **07** | DEG_analysis.sh | `genes.counts.matrix,TMM.EXPR.matrix`, `samples.file` | `edgeR.*/` (차등발현 결과, 클러스터) |

---

## 필수 준비 파일

```bash
# 입력 파일들
SRR_Acc_List.txt                              # SRA 다운로드 목록
Oryza_sativa.IRGSP-1.0.dna.toplevel.fasta    # 참조 유전체
Oryza_sativa.IRGSP-1.0.61.chr.gtf            # 유전자 어노테이션

# 생성해야 할 파일
samples.file                                   # 샘플 정보 (TAB 구분)
```

**samples.file 형식:**
```
Cadmium	Cd1
Cadmium	Cd2  
Cadmium	Cd3
Control	Con1
Control	Con2
Control	Con3
```

---

## 최종 디렉토리 구조

```
project/
├── 스크립트들 (01~07.*.sh)
├── 원본 FASTQ들 (SRR*_1.fastq, SRR*_2.fastq)  
├── 전처리 FASTQ들 (*_good_1.fastq, *_good_2.fastq)
├── BAM 파일들 (Con1~3_sorted.bam, Cd1~3_sorted.bam)
├── StringTie 결과 (Con1~Cd3/ 디렉토리들)
├── 발현 행렬들 (gene_count_matrix.csv, genes.TPM.matrix)
├── QC 결과들 (fastqc/, *.pdf)
└── 차등발현 결과 (edgeR.*/diffExpr.*, clusters_fixed_P_30/)
```

---

## 핵심 결과 파일들

### 품질 관리
- `fastqc/multiqc_report.html` - 전체 품질 리포트
- `*.prcomp.principal_components.pdf` - PCA 플롯
- `*.sample_cor_matrix.pdf` - 샘플 상관관계

### 발현 데이터  
- `gene_count_matrix.csv` - 통계분석용 count 행렬
- 'TMM.EXPR.matrix' - DEG 분석용 행렬
- `genes.TPM.matrix` - 비교용 TPM 행렬

### 차등발현 분석
- `edgeR.*/genes.counts.matrix.Cadmium_vs_Control.edgeR.DE_results` - 전체 차등발현 결과
- `edgeR.*/diffExpr.P0.001_C1.*.subset` - 필터링된 유의한 유전자들
- `clusters_fixed_P_30/subcluster_*.matrix` - 발현 패턴별 유전자 클러스터

