# hse21_hw1
 1. Создаем папку hw:
 mkdir hw
 cd hw
 2. Создаем ссылки в личной папке:
+ ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
 3. Выбираем случайные чтения с помощью команды seqtk (5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs):
 seqtk sample -s1021 oil_R1.fastq 5000000 > sub1.fastq
 seqtk sample -s1021 oil_R2.fastq 5000000 > sub2.fastq
 seqtk sample -s1021 oilMP_S4_L001_R1_001.fastq 1500000 > mp1.fastq
 seqtk sample -s1021 oilMP_S4_L001_R2_001.fastq 1500000 > mp2.fastq
 4. Oцениваем качество исходных чтений, используя программы fastqc и multiqc:
 mkdir fastqc, скачаем файлы с помощью Cyberduck
ls *.fastq | xargs -P 4 -tI{} fastqc -o fastqc {}

mkdir multiqc
multiqc -o multiqc fastqc
5. Используем программы platanus_trim и platanus_internal_trim, чтобы подрезать чтения и убрать праймеры:
platanus_trim sub1.fastq sub2.fastq 
platanus_internal_trim mp1.fastq mp2.fastq  
6. Удаляем исходные файлы:
 rm sub* mp*
 7. Теперь оцениваем качество подрезанных чтений, используя программы fastqc и multiqc:
mkdir fastqc_trimmed
ls sub* mp* | xargs -tI{} fastqc -o fastqc_trimmed {}
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
<img width="867" alt="Снимок экрана 2021-10-27 в 14 33 06" src="https://user-images.githubusercontent.com/91221560/139058062-f976673d-40c0-4a24-ad52-bd6cda57b944.png">
<img width="867" alt="Снимок экрана 2021-10-27 в 14 33 31" src="https://user-images.githubusercontent.com/91221560/139058091-2b42f018-194b-4d96-90a5-f1e68412f860.png">
<img width="867" alt="Снимок экрана 2021-10-27 в 14 34 28" src="https://user-images.githubusercontent.com/91221560/139058742-920980ee-eb44-4fdb-b9de-5142297aa4dc.png">
 
 <img width="947" alt="Снимок экрана 2021-10-27 в 14 44 48" src="https://user-images.githubusercontent.com/91221560/139059887-fb649756-1d14-4ad6-8cea-ca775615305f.png">
 
 <img width="862" alt="Снимок экрана 2021-10-27 в 14 58 41" src="https://user-images.githubusercontent.com/91221560/139061871-aa33fa86-7374-464e-9dd4-ab9238b84da6.png">

8. После ! [] (<img width="876" alt="Снимок экрана 2021-10-27 в 14 13 50" src="https://user-images.githubusercontent.com/91221560/139055855-f92bc872-1e6a-419a-884f-06831bbffbe6.png">)
9.<img width="861" alt="Снимок экрана 2021-10-27 в 14 24 22" src="https://user-images.githubusercontent.com/91221560/139056919-65103896-a720-4c56-81df-07767bcc3e8f.png">

<img width="861" alt="Снимок экрана 2021-10-27 в 14 25 18" src="https://user-images.githubusercontent.com/91221560/139057124-4fa464cb-317b-440e-9f9d-efea918851d8.png">

<img width="862" alt="Снимок экрана 2021-10-27 в 14 59 47" src="https://user-images.githubusercontent.com/91221560/139061912-e65684cf-3ff9-49af-bd77-6f5a43dc521b.png">





 
