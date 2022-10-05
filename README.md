# hse22_hw1

## Скачивание папки с github

```bash
git clone https://github.com/vkmelnik/hse22_hw1.git
cd hse22_hw1
```
## Создание ссылок на файлы fastq

```bash
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq 
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq 
```

## Выбор чтений

```bash
seqtk sample -s 603 oil_R1.fastq 5000000 > sub1.fq
seqtk sample -s 603 oil_R2.fastq 5000000 > sub2.fq
seqtk sample -s 603 oilMP_S4_L001_R1_001.fastq 1500000 > sub3.fq
seqtk sample -s 603 oilMP_S4_L001_R2_001.fastq 1500000 > sub4.fq
```

## Оценка качества чтений

```bash
fastqc sub1.fq sub2.fq sub3.fq sub4.fq
multiqc sub1_fastqc.zip sub2_fastqc.zip sub3_fastqc.zip sub4_fastqc.zip
```
![alt text](https://github.com/vkmelnik/hse22_hw1/blob/main/images/multiqc1.png)

## Подрезание чтений

```bash
platanus_trim sub1.fq sub2.fq
platanus_internal_trim sub3.fq sub4.fq
```
Удаление ненужных файлов
```bash
rm sub1.fq
rm sub2.fq
rm sub3.fq
rm sub4.fq
```
Оценка качества по подрезанным файлам
```bash
fastqc sub1.fq.trimmed sub2.fq.trimmed sub3.fq.int_trimmed sub4.fq.int_trimmed
multiqc sub1.fq.trimmed_fastqc.zip sub2.fq.trimmed_fastqc.zip sub3.fq.int_trimmed_fastqc.zip sub4.fq.int_trimmed_fastqc.zip
```
![alt text](https://github.com/vkmelnik/hse22_hw1/blob/main/images/multiqc2.png)

## Сбор контигов

```bash
screen platanus assemble -o Pxut -f sub1.fq.trimmed sub2.fq.trimmed 2> assemble.log
```

## Сбор скаффолдов

```bash
screen platanus scaffold -o Pxut -c Pxut_contig.fa -IP1 sub1.fq.trimmed sub2.fq.trimmed -OP2 sub3.fq.int_trimmed sub4.fq.int_trimmed 2> scaffold.log
```

## Уменьшение количества gap-ов

```bash
screen platanus gap_close -o Pxut -c Pxut_scaffold.fa -IP1 sub1.fq.trimmed sub2.fq.trimmed -OP2 sub3.fq.int_trimmed sub4.fq.int_trimmed 2> gaps.log
```
Удаление ненужных файлов
```bash
rm sub1.fq.trimmed
rm sub2.fq.trimmed
rm sub3.fq.int_trimmed 
rm sub4.fq.int_trimmed
```
Скачивание всех полученых файлов с сервера
```bash
scp -i mykey -P 32222 -r vkmelnik@92.242.58.92:hse22_hw1 hse22_hw1
```
