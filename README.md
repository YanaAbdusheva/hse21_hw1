# hse21_hw1
1. Создаем папку hw1 и копируем туда исходные файлы с сервера:
```
mkdir hw1
cd hw1
ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
```
2. Выбираем случайно 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs с личным random seed (1116):
```
seqtk sample -s1116 oil_R1.fastq 5000000 > pe_R1.fastq
seqtk sample -s1116 oil_R2.fastq 5000000 > pe_R2.fastq
seqtk sample -s1116 oilMP_S4_L001_R1_001.fastq 1500000 > mp_R1.fastq
seqtk sample -s1116 oilMP_S4_L001_R2_001.fastq 1500000 > mp_R2.fastq
```
3. Удаляем исходные файлы:
```
rm -r oil_R1.fastq
rm -r oil_R2.fastq
rm -r oilMP_S4_L001_R1_001.fastq
rm -r oilMP_S4_L001_R2_001.fastq
```
4. Оценка качества исходных чтений (fastQC) и получение по ним общей статистики (multiQC):
```
mkdir fastqc
ls *.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}

mkdir multiqc
multiqc -o multiqc fastqc
```
5. С помощью программ platanus_trim и platanus_internal_trim подрезаем чтения по качеству и удаляем праймеры:
```
platanus_trim pe_R1.fastq pe_R2.fastq 
platanus_internal_trim mp_R1.fastq mp_R2.fastq  
```
6. Удаляем ненужные исходные файлы:
```
rm pe_R1.fastq
rm pe_R2.fastq
rm mp_R1.fastq
rm mp_R2.fastq
```
7. Оценка качества подрезанных чтений и получение по ним общей статистики с помощью программы fastQC и multiQC :
```
mkdir trimmed_fastq
mv -v *trimmed trimmed_fastq/
```
```
mkdir trimmed_fastqc
ls trimmed_fastq/* | xargs -P 4 -tI{} fastqc -o trimmed_fastqc {}
```
```
mkdir trimmed_multiqc
multiqc -o trimmed_multiqc trimmed_fastqc
```
До:

![image](https://user-images.githubusercontent.com/93256219/139110718-e28af320-f379-49de-a9e5-a673a509cce9.png)

После (уменьшилась длина последовательностей, процент GC у paired-end подрос):

![image1](https://user-images.githubusercontent.com/93256219/139110930-a350973c-1794-4863-8c35-8f9301113d7c.png)


До:

![image3](https://user-images.githubusercontent.com/93256219/139112046-21dc1576-0af6-4add-a8e5-edbac3b68268.png)

После (улучшилось качество чтений):

![image4](https://user-images.githubusercontent.com/93256219/139112203-6623ea86-9d66-4649-9838-b3ad7b7ee20a.png)

До:

![image5](https://user-images.githubusercontent.com/93256219/139112515-4264efee-4a7c-4a4d-849b-ed1f6cf33844.png)

После (почти полностью были удалены адаптеры):

![image6](https://user-images.githubusercontent.com/93256219/139112540-60877e91-d162-4e19-803c-35b26c47df35.png)

